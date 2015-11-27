# Jean
This project is forked from the official Hansoft Jean project with some minor fixes and customizations to support the Schneider Electric SAFe kit.
To setup the Schneider Electric SAFe demo toolkit you need to assemble the following projects hosted on github:
1. [Hansoft-Jean-Jean](https://github.com/patrikha/Hansoft-Jean-Jean)
2. [Hansoft-Jean-Behavior](https://github.com/patrikha/Hansoft-Jean-Behavior)
3. [Hansoft-ObjectWrapper](https://github.com/patrikha/Hansoft-ObjectWrapper)
4. [Hansoft-Jean-DeriveBehavior](https://github.com/patrikha/Hansoft-Jean-DeriveBehavior)
5. [HansoftSAFeExtension](https://github.com/patrikha/HansoftSAFeExtension)
6. [Hansoft-Jean-AggregateMilestoneBehavior](https://github.com/patrikha/Hansoft-Jean-AggregateMilestoneBehavior)
7. [Hansoft-Jean-DependencyBehavior](https://github.com/patrikha/Hansoft-Jean-DependencyBehavior)
8. [Hansoft-Jean-TypeColorBehavior](https://github.com/patrikha/Hansoft-Jean-TypeColorBehavior)
The settings files are configured to run on the demo Hansoft server that is packaged on docker hub:
[patrikha/hansoftserver](https://hub.docker.com/r/patrikha/hansoftserver/)


## SAFe kit
The Schneider Electric SAFe implementation for Hansoft is based on a set of assumptions:
* All programs and teams are kept in separate Hansoft projects within the same database.
* The following naming convention is used:
  * Programs: “Program -  xyz”
  * Hardware projects, eg gantt scheduling projects: “Project - xyz”
  * All team projects regardless of process: “Team - xyz”
* The producer of some feature always expose a milestone for that feature. (Milestone carries estimated finish date, status and progress.)
* The consumer always links a backlog item to the producing milestone.
* There can be more that one producer to every feature. (multiple links)
* Mandatory custom columns for each project type are defined in the demo database hosted on docker hub. (See above.)

## Features:
* Program features gets updated with aggregated information from all linked team milestones.
* Project schedule tasks gets updated with information based on linked team milestones.
* Board views are colorized based on task type.
* External and internal program dependencies are updated based on milestone links.
* Team to team dependencies are updated based on milestone links.
* Aggregation milestones can be created to visualize program features that should drive gantt scheduling tasks in HW projects.
For a complete list of features please see the settings file:
[JeanSettings.xml](https://github.com/patrikha/Hansoft-Jean-Jean/blob/master/JeanSettings.xml)
