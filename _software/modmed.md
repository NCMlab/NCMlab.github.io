---
short_name: mediation
layout: authors
imagename: ../images/LabAims.png
name: Moderated-Mediation for Neuroimaging
shortDescr: A MatLab based package for moderated-mediation analyses with brain imaging data
---
This package implements "process" modeling for functional and structural neuroimaging data and is based off the works by Andrew F. Hayes: http://www.afhayes.com/

One key feature of the current work is that it is written in MatLab for use on a computing cluster without the use of the distributed computing toolbox. The code uses its own map/reduce approach to split the data into small chunks. Bash scripts are written for calling MatLab to operate on each chunk and are then submitted to a cluster using qsub. This approach avoids the requirement for MatLab's distributed toolbox which can be quite expensive. 

Without the use of a computing cluster these models may not be able to be estimated due to the intense computational time. The large computational time comes mainly from the fact that a large number of voxels (~100,000) are estimated in a typical brain and significance is tested using bootstrapping.

Curremt and future development on this project will translate it into python and will take advantage of larger computer clusters and local GPUs.

Find the code [here.](https://github.com/NCMlab/ProcessModelsNeuroImage)
