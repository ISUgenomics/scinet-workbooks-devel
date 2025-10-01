---
title: "Getting started with SCINet"
description: "Orientation for newcomers and best-practice setups for SCINet supercomputer users"
type: introduction
author: Aleksandra Badaczewska
index: 1
order: 1

tags: [unix, command line]

objectives:
- Introduce the SCINet Initiative and its mission in advancing scientific computing.
- Provide guidance for new users to access the SCINet supercomputers.
- "Highlight resources: SCINet website, user guides, and workbooks with practical tutorials."

overview: [objectives, terms]


---


{% include overviews %}

### What is SCINet?

The SCINet initiative is an effort by the USDA [Agricultural Research Service (ARS)](https://www.ars.usda.gov/) to grow USDA’s research capacity by providing scientists with access to high-performance computing clusters, high-speed networking for data transfer, and training in scientific computing.

- [SCINet Website](https://scinet.usda.gov/)
- [SCINet Workbooks]()
- [SCINet User Guides](https://scinet.usda.gov/guides)
- [SCINet Forum](https://forum.scinet.usda.gov/)


## Accessing SCINet

<div class="process-list" markdown="1">
{% include setup/scinet_access %}

### Start onboarding

Users who are new to the HPC environment may benefit from the [SCINet/Ceres onboarding video](https://www.youtube.com/watch?v=d7oKSL4aitw). 

{% include table caption="HPC Clusters on SCINet" content="| Cluster Name	| Location	| Login Nodes	| Transfer Nodes |
|---------------|-----------|-------------|----------------|
| [Ceres](https://scinet.usda.gov/guides/resources/#scinet-ceres) | Ames, IA | ceres.scinet.usda.gov | ceres-dtn.scinet.usda.gov |
| [Atlas](https://scinet.usda.gov/guides/resources/atlas)	        | Starkville, MS | atlas-login.hpc.msstate.edu | atlas-dtn.hpc.msstate.edu |" %}

User Guide: [Differences between Ceres and Atlas](https://scinet.usda.gov/guides/resources/CeresAtlasDifferences)

{% include setup/scinet_login keep="true" %}


## Next steps

### Explore interfaces and file system

As a new user, begin by exploring the SCINet interfaces and navigating the core areas of the file system. 
These steps provide concise, practical guidance and will greatly facilitate your work on the system.

* **(selected)** [Accessing interfaces on SCINet](/computing-skills/scinet/interfaces) *(Shell, RStudio, JupyterLab, VS Code)*
* **(required)** [File system on Atlas and Ceres supercomputers](http://127.0.0.1:4500/computing-skills/scinet/file_system)

### Set up user workspace

<div class="highlighted highlighted--basic"><div class="highlighted__body" markdown="1"> 
SCINet account provides you 30 GB of storage space on Ceres and Atlas in a home directory `/home/<username>/`. 
You can also use collaborative space in `/90daydata/shared`.
</div></div>

With your SCINet account activated, you can quickly configure a **personal workspace** to work through the tutorials provided in this workbook. 
* **(required)** [Setting up your workspace]()
* **(optional)** [Customizing your shell](/computing-skills/command-line/configuration/)

### Set up project workspace

<div class="highlighted highlighted--basic"><div class="highlighted__body" markdown="1"> 
Users are advised to [request a SCINet project](https://scinet.usda.gov/support/request#project-request) with additional storage space. Requests for new projects must be submitted by a full-time ARS employee. Other users gain access to a project once they are added as members of the project group. SCINet project storage allocations are located in project directories `/project/<project_name>/` on both supercomputers.
</div></div>

With your SCINet project approved, set up the **project workspace** to launch your research tasks.
* **(required)** [Project setup on SCINet’s supercomputers](http://127.0.0.1:4500/computing-skills/scinet/project_setup)

</div>

## Where to get help?

* [SCINet FAQs](https://scinet.usda.gov/support/faq#faqs)
* [SCINet user guides](https://scinet.usda.gov/guides/#scinet-guides-list)
* [Contact the VRSC](mailto:scinet_vrsc@iastate.edu)

You're now ready to join SCINet community! 