---
name: Battery of Tasks
layout: page
---
# Battery of Tasks
Delivering a predefined battery of tasks requires setting up a config file for the battery. This battery is assigned a numeric ID which is used in the weblink URL. Therefore, the software reads the provided battery ID and loads the appropriate lists of tasks and questionnaires. 

Specification of the battery also includes specification of the parameters for each task. Therefore, the same battery of tasks may have different configurations for the individual tasks. This may include different languages of administration, instructions using different levels of language comprehension, or short and long versions of the same task.

Identification of batteries of tasks each using their own configurations facilitates code reusability. Every battery may contain different questionnaires and tasks; however, the code underlying the tasks does not change. Only the configuration files change. The result is that any software updates are immediately transferred to all batteries.

An example of a splash screen shown at the start of battery is shown below.

<img src="/3C/assets/3CPlatformBattery.png" alt="3CPlatformBattery.png"/>