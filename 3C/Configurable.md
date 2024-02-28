---
name: Configurable
layout: page
---
# Configurable
The focus of this software platform is reusability. Therefore, a single version of the software to run each tasks exists. Any variations to the tasks are made via configuration files. This approach facilitates code updates and ensures that any updates are applied to all versions of the tasks.

Every task has some common components. These include a welcome screen, thank you screen, an option for a user to type in notes and feedback, and instructions. These common components only exist in one location in the software. The individual tasks each choose which of these common components to include within their own configuration files. The overall flow of a task is shown below.



<img src="/3C/assets/FlowChart.png" alt="FlowChart.png"/>