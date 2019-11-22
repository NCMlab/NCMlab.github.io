---
layout: post
title: Peer-review Classroom exercise
categories: teaching
---

# Peer Review Assignment

As part of my **Research Methods course** I would like the students to learn about peer-review. I discuss peer review in class and how it works and how I, as a scientist, engage with peer review. For work on their own time I have them review the CIHR peer-review checklist and encourage them to work through the module on peer review that the CIHR provides. 

In my class of about 90 students they have a term paper to write in groups of three. I ask that the students turn in a rough draft of their introduction and research question. I therefore get one paper per group which is 30 papers. In order for them to learn about peer review, I take these papers and redistribute them around the class. I take each of these 30 papers and send them out to other students NOT from their group. These students then have to provide a review of the paper and to fill out the term paper rubric. Since I have three students per group, each group paper gets feedback from three different students. Therefore, all groups get **feedback from three different points of view** that they can use to improve their own work. The peer review process also provides students the opportunity to see how at least one other group frames their research question and formulates their arguments. 

As for me, all of my students get feedback and it is not from me or a TA. 

Once the students turn in their peer review, these marked up papers get emailed back all members of the original groups that wrote the paper. Making all of this happen requires a fair bit of *magic*. Well **python code and an Excel spreadsheet**. The key thing that makes this all work is the use of python code for sending email for me. This is done via my gmail account. Google isn’t happy about this and you have to modify your permissions since Google thinks you are spamming!

Below are the steps I use to do this assignment and the code I used. Also please note that my University uses Brightspace as their electronic classroom.
## Download Student List
Download a list of all students in the class, their email address and what group they belong to. With Brightspace, this is done by exporting the grades. Make sure to include their name, email address and group number when preparing this. Also, unselect all grade items since you do not need that info for this step.

## Format the File
Format this file a bit. It will have columns for Last Name, First Name, Email and a column with the name of whatever your group assignment is. The groups are not written as numbers but as “Group ##.” The word “Group” and the space after it need to be removed. To do this create a new column called Groups, then execute the following command on each row that has student data in it. The command is:
```
	=numbervalue(right(E2, len(E2)-6))
```
This will take a fixed number of characters starting from the right of whatever is in cell E2. The number of characters to take is based on the length of the text in the cell MINUS the length of what you want to remove. In this case it is 6 (G-R-O-U-P- ). Note the space at the end there! The numbervalue command wrapping around this just converts the extract text to a number.

Sort this column from smallest to biggest. 
Also change the column names so they do NOT include spaces. The spaces mess up the computer programs we will use later. 

## Assign Groups
The first thing to do here is to make sure that you have a contiguous list of groups. Check to see if you have any skipped groups. If so make a note of them.
Make a new column named **GroupAssigned.** Create a list of numbers from 2 to the number of groups you have, and then 1. If there are any skipped groups, then remove them from this list of numbers. Copy and paste this below itself three times (This is the same number of students per group). This assigns each individual person the paper **from a different group.** Check this assigned group column to see if any students have been assigned their own paper. This will usually happen at least once.

Create this “checking” new column and use this command in it
```
=if(F2=G2,1,0)
```
This will give a value of one if their group and their assigned paper’s group are the same. Find these values and fix them manually.

## Download the Papers
Go to Brightspace and download the assigned papers. Make sure that you have downloaded all of the papers. You may need to change the number of papers displayed per page, which is a little pull down menu at the bottom of the list. Once they have been downloaded, unzip the file.

## Rename the Submissions and Append the Rubric
The students have been instructed to NOT put their names on their papers. This is to make the assignment a double blind peer review. All the papers that have been downloaded now need to be renamed according to their group number. This is done with the following python code:

```python
import os
import shutil
import pandas as pd
# This is a folder containing one folder per submission
BaseDir = PATH TO FOLDER CONTAINING PAPERS
# This is a CSV file containing names and group membership
# The file was edited so that students only have one last name
DataFile = os.path.join(BaseDir,'HSS3101BGroupList.xlsx')

Data = pd.read_excel(DataFile)

# Set the index to last name
Data.set_index("LastName",inplace = True)
Data.head()

SubmissionFolders = next(os.walk(BaseDir))[1]

for dirpath in SubmissionFolders:
    dirName = os.path.split(dirpath)[-1]
    ExtractName = dirName.split(' - ')
    LastName = dirName.split(' - ')[1].split(' ')[-1]# This changes between class A and B
    CurrentGroup = Data.loc[LastName]
    print('%s\t%d'%(LastName,CurrentGroup.Group))
    # Take the file in this folder and copy it out to a main folder
    FileFound = os.listdir(dirpath)[0]
    FileFoundExt = os.path.splitext(FileFound)[-1]
    inFile = os.path.join(BaseDir,dirpath,FileFound)
    OutFilename = 'Group%03d%s'%(CurrentGroup.Group, FileFoundExt)
    outFile = os.path.join(BaseDir, OutFilename)
    shutil.copyfile(inFile, outFile)

```

## Email all files
The renamed files now need to be emailed to the students that were assigned. This involves using Google Email and creating an automated email and attaching two files to it. When you run this Google complains and tries very hard to block this automated emailing.

```python
import os
import shutil
import smtplib 
from email.mime.multipart import MIMEMultipart 
from email.mime.text import MIMEText 
from email.mime.base import MIMEBase 
from email import encoders 
import pandas as pd
 # NOTE WHAT YOU TYPE HERE WILL NOT BE HIDDEN!
PASSWORD = input('Type in your password: ')
  
BaseDir = '/Users/jason/Dropbox/steffenercolumbia/UOttawa/Teaching/HSS3101/Fall2019'
Section = 'SectionA'
OutFolder = os.path.join(BaseDir, Section, 'OutFolder')

DataFileName = 'HSS3101APeerReviewAssignment.xlsx'
DataFile = os.path.join(BaseDir, Section, DataFileName)

#DataFile = os.path.join(BaseDir,'HSS3101AGroupList.xlsx')
# DataFile = os.path.join(BaseDir,'ResendB.xlsx')
Data = pd.read_excel(DataFile)
# Set the index to last name
# Data.set_index("LastName",inplace = True)
Data.head()

#row = next(Data.iterrows())

count = 0

for index, row  in Data.iterrows():
    # Check for the filetype and only resend the problem ones
    
    if count >-99:
        print(row['Email'])
    
        fromaddr = "steffener@gmail.com"
        # toaddr = "steffener@gmail.com"
        toaddr = row['Email']
        
        EmailBody = "Dear %s %s,\nAttached please find the file that you are to peer review. " % (row['FirstName'],row['LastName'])
        EmailBody = EmailBody+'Please review this paper, fill out the attached rubric, offer feedback, \
            comments and suggestions in the paper. Once you have made your edits, submit the TWO files for \
            your Peer review assignment on Brightspace.\nThank you\n\nProf Steffener\n\nPlease note that this \
            is an automated email so do not respond to it.'
        EmailBody = EmailBody
         
        # instance of MIMEMultipart 
        msg = MIMEMultipart() 
        # storing the senders email address   
        msg['From'] = fromaddr 
        # storing the receivers email address  
        msg['To'] = toaddr 
        # storing the subject  
        msg['Subject'] = "Do not reply. Peer review assignments"
        # string to store the body of the mail 
        body = EmailBody
        # attach the body with the msg instance 
        msg.attach(MIMEText(body, 'plain')) 
       
        # open the file to be sent  
        FileList = ["TermPaperRubric_Quant_Draft.docx", 'Group_%d.docx'%(row['AssignedGroups'])]
        for file in FileList:
            part = MIMEBase('application', 'octet-stream')
            part.set_payload(open(file, 'rb').read())
            encoders.encode_base64(part)
            part.add_header('Content-Disposition', 'attachment; filename="%s"' % file)
            msg.attach(part)
        
        # creates SMTP session 
        s = smtplib.SMTP('smtp.gmail.com', 587) 
        # start TLS for security 
        s.starttls() 
        # Authentication 
        s.login(fromaddr, PASSWORD) 
        # Converts the Multipart msg into a string 
        text = msg.as_string() 
        # sending the mail 
        s.sendmail(fromaddr, toaddr, text) 
        # terminating the session 
        s.quit() 
        
    else:
         print('Already sent this one: %s'%(row['Email']))
    count += 1
```
## Peer-review 
The students receive their assigned papers via email and are asked to perform their review. The commented paper and the rubric are submitted as an assignment via Brightspace. The next step is to return all of these marked up files to the original groups.

## Download the peer-review assignments
Go to Brightspace and download all of the files as a zip file. Then unzip it. This will be one folder containing one folder for each student that turned in their assignment. Within the folder name is the student’s name. This is key for taking the files they turned it and sending them back to the original writers. 

## Return the Files
Sending the edited and commented files back to their original writers uses the following bit of code. This next step takes all files from your 90ish students and sends them back to each member of the original groups. Each student of each group gets 3 emails. Therefore, this next step sends over 270 emails. 

```python
import os
import shutil
import smtplib 
from email.mime.multipart import MIMEMultipart 
from email.mime.text import MIMEText 
from email.mime.base import MIMEBase 
from email import encoders 
import pandas as pd
# NOTE WHAT YOU TYPE HERE WILL NOT BE HIDDEN!
PASSWORD = input('Type in your password: ')

# This is a folder containing one folder per submission
# BaseDir = '/Users/jason/Dropbox/steffenercolumbia/UOttawa/Teaching/HSS3101/Fall2019'
BaseDir = '/Users/jasonsteffener/Dropbox/steffenercolumbia/UOttawa/Teaching/HSS3101/Fall2019'
# What is the section?
Section = 'SectionB'
InFolder = os.path.join(BaseDir, Section, 'PeerReviewInFolder')
# WHat is the file name containing the names and group numbers
DataFileName = 'HSS3101BPeerReviewAssignment.xlsx'
DataFile = os.path.join(BaseDir, Section, DataFileName)

# Read the data file
Data = pd.read_excel(DataFile)
# CHeck the file
Data.head()

# Get a list of all the submitted assignments. This is the folder downloaded from Brightspace
SubmissionFolders = next(os.walk(InFolder))[1]

count = 0
# There will be one submission folder per student
# Cycle over each folder
for dirpath in SubmissionFolders:
    # Use this if statement for testing and limits
    if count < 3000:
        # Find the directory name
        dirName = os.path.split(dirpath)[-1]
        # What files are in this folder?
        # SOme students submit a single file and others multiple files
        FilesFound = os.listdir(os.path.join(InFolder, dirpath))
        # Make a list of these files
        FileList = []
        for ff in FilesFound:
            FileList.append(os.path.join(InFolder, dirpath, ff))
        # Find out who these files are from
        # SPlit the folder name which contains the student names
        ExtractName = dirName.split(' - ')
        # Find first and last names
        LastName = dirName.split(' - ')[1].split(' ')[-1]# This changes between class A and B
        FirstName = dirName.split(' - ')[1].split(' ')[0]# This changes between class A and B
        # Use the first and last name to find this person in the spreadsheet
        FindexLN = Data.index[Data['LastName'] == LastName].tolist()
        FindexFN = Data.index[Data['FirstName'] == FirstName].tolist()
        IndexOfThisPerson = set(FindexLN) & set(FindexFN)
        # Make sure only one person was found
        AssignedGroup = -99
        # if the program cannot figure out who to send the files to it asks the user.
        # This process gets messed up when people have more than one first or last name
        # Or if there is an accent in the person's name.
        # In my spreadsheet I remove second first names and the first of two last names
        if len(IndexOfThisPerson) != 1:
            print('\n=================================\n')
            print('Error with %s %s'%(FirstName, LastName))
            print(dirName)
            print('FN index')
            print(FindexFN)
            print('LN index')        
            print(FindexLN)
            AssignedGroup = int(input('Type in this persons assigned group: '))
            
        if AssignedGroup == -99:
            # Then the group did not need to be manually assigned
            AssignedGroup = Data.iloc[list(IndexOfThisPerson)[0]].AssignedGroups
        # What is the index of the students in this group    
        OrigIndex = Data.index[Data['Groups'] == AssignedGroup].tolist()
        # Cycle over all students in the group
        for k in OrigIndex:
            fromaddr = "steffener@gmail.com"
            # Use this line for testing purposes
            # toaddr = "steffener@gmail.com"
            toaddr = Data.iloc[k].Email
            
            # Create an email for each person in the group
            EmailBody = "Dear %s %s,\n \
                Please find peer-review files attached. If the rubric is not filled out or \
                the review is incomplete, that means the peer-review assignment was not completed. \
                Please accept the feedback that you did receive and address it as you see fit.\n\
                Prof. S\n\n\
                P.S. I had to modify some of your names to get this all to work. Sorry!" % \
                (Data.iloc[k].FirstName,Data.iloc[k].LastName)
            # instance of MIMEMultipart 
            msg = MIMEMultipart() 
            # storing the senders email address   
            msg['From'] = fromaddr 
            # storing the receivers email address  
            msg['To'] = toaddr 
            # storing the subject  
            msg['Subject'] = "Peer Feedback"
            # string to store the body of the mail 
            body = EmailBody
            # attach the body with the msg instance 
            msg.attach(MIMEText(body, 'plain'))     
            # Attach the files
            FileCount = 1
            for file in FileList:
                Str = '\nSending file: %s\n\tto\n%s, %s at email: %s'%(file, Data.iloc[k].FirstName, Data.iloc[k].LastName, toaddr)
                print(Str)
                EXT = os.path.splitext(file)[-1]
                FileName = 'File%02d%s'%(FileCount, EXT)
                FileCount += 1
                part = MIMEBase('application', 'octet-stream')
                part.set_payload(open(file, 'rb').read())
                encoders.encode_base64(part)
                part.add_header('Content-Disposition', 'attachment; filename="%s"' % FileName)
                msg.attach(part)
            # creates SMTP session 
            s = smtplib.SMTP('smtp.gmail.com', 587) 
            # start TLS for security 
            s.starttls() 
            # Authentication 
            s.login(fromaddr, PASSWORD) 
            # Converts the Multipart msg into a string 
            text = msg.as_string() 
            # sending the mail 
            s.sendmail(fromaddr, toaddr, text) 
            # terminating the session 
            s.quit() 
            count += 1
```
	



