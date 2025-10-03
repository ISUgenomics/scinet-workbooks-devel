---
title: Workspace setup for bioinformatics
svg: /genomics.svg
author: Aleksandra Badaczewska

header:
  overlay_image: 07-wrangling/assets/img/07_data_acquisition_banner.png
index: 
order: 5
wg: Bioinformatics
description: ""
type: interactive tutorial

objectives: 
  - Understand the key components of setting up a bioinformatics pipeline on an HPC environment.
  - Learn best practices for organizing project files, managing storage, and documenting workflows.
  - Recognize HPC-specific storage structures, usage policies, and quota limitations.
  - Become familiar with effective strategies for collaboration and data sharing in HPC settings.

applications:
  - Structuring a project directory for different types of bioinformatics analyses (e.g., RNA-seq).
  - Managing data and metadata across home, scratch, long-term storage, and shared workspaces.
  - Configuring and documenting software environments using module systems or Conda.
  - Using reproducible project templates for consistent pipeline setup and configuration.
  - Integrating GitHub repositories for version control, collaboration, and workflow transparency.

terms: [HPC, File System, Quota, Scratch Space, Project Directory, Metadata, Module System, Container, Environment File, Conda, Git Repository, Reproducibility]

takeaways: 
  - "Start with a clear directory tree structure: separate raw data, processed results, scripts, and logs."
  - "Use README.md, environment files, and changelogs to document workflows and project decisions."
  - "Understand the purpose and limitations of different storage tiers (e.g., scratch vs. home vs. archive) and clean up intermediate files regularly."
  - "Monitor your quota usage and be aware of retention or purge policies on shared resources."
  - "Use version-controlled repositories (e.g., GitHub) to manage pipeline templates, analysis scripts, and documentation."
  - "Document software versions and module configurations to ensure reproducibility."
  - "Always test pipelines with small data subset before scaling up."
  - "Preserve critical results in backed-up long-term storage on Juno."

overview: [objectives, applications, terminology]


---


<div class="highlighted highlighted--basic"><div class="highlighted__body" markdown="1"> 
An active SCINet account with access to shared and project directories is required to complete this tutorial. 
Basic familiarity with the [SCINet File System](/computing-skills/scinet/file_system) and [Shell interface](/computing-skills/scinet/interfaces) for [navigating directories](/computing-skills/command-line/#navigating-the-unix-file-system) and [managing files](/computing-skills/command-line/#file-management-in-the-shell) is also expected. No prior experience with bioinformatics pipeline development is required. 
</div></div>


## Overview

SCINet HPC environments provide the resources and computing power needed for large-scale bioinformatics analyses. 
To use these systems effectively, projects should be set up in a way that is well-organized, easy to maintain, 
efficient in storage usage, and aligned with best practices for shared multi-user workspace.

This tutorial will guide you through practical strategies for structuring bioinformatics projects on HPC systems. 
You’ll learn how to manage files across different storage locations, document your work clearly, and apply reusable templates that support reproducibility and collaboration.

{% include overviews %}


### Reproducibility, compliance, and collaboration needs

A well-structured project setup is more than a matter of convenience on a shared supercomputer:
- It ensures **reproducibility**, by keeping data, code, and environments organized and documented in a way that others (and your future self) can follow.  
- It supports **compliance**, by aligning with storage policies, quotas, and security rules that govern shared HPC resources.  
- It enables **collaboration**, by making projects easier to share, extend, and maintain within research groups.  

### Why project structure matters in bioinformatics?

Project structure matters in bioinformatics because it directly affects efficiency, reproducibility, and resource usage. Placing inputs and outputs in the right storage tier can speed up calculations and prevent quota or purge issues, while organizing scripts and results makes workflows easier to share and repeat. Many datasets (e.g., reference genomes) and tools are pre-downloaded on HPC systems, so knowing where to find them can save hours of unnecessary setup.

<div class="highlighted highlighted--tip"><div class="highlighted__body" markdown="1">
**Structure your project, and your research will structure itself!**
</div></div>

## Getting started

Before starting, make sure you are familiar with:  
- [Accessing interfaces on SCINet](/computing-skills/scinet/interfaces)
- [File system on Atlas and Ceres supercomputers](/computing-skills/scinet/file_system)  
- [Creating Project Structure](/computing-skills/scinet/project_setup#manage-project-structure)  
- [Managing Storage](/computing-skills/scinet/project_setup#manage-storage)  

First, you will log in through **Open OnDemand (OOD)** and use the shell to access SCINet file system locations.

<div class="usa-accordion" style="margin-top: 1em;">

{% include accordion title="<h3 style='margin: 0; border-bottom: none; font-size: 1.1em; display: inline;'>Access the shell via SCINet OOD</h3><span style='font-weight: 300;'>(required) used for accessing supercomputers</span>" class="primary" controls="access-scinet" icon=false %}
<div id="access-scinet" class="accordion_content" markdown='1' hidden> 
{% include setup/scinet_access %}
{% include setup/scinet_login %}
{% include setup/ood_shell %}
</div>
</div>

The goal is to help you set up and organize a well-structured **workspace for bioinformatics workflows** by creating a clear directory structure, using the appropriate storage tiers for inputs, temporary files, and results, and documenting your pipeline for reproducibility and collaboration.

<div class="process-list" markdown="1">

### Create a workspace

*Organize your analysis with a clear directory structure for inputs, intermediate files, and results.*



### Build a reproducible environment

*Use modules, containers, or package managers to ensure your tools and dependencies can be recreated anywhere.*



### Document from the start

*Keep notes and README files alongside your work so that others (and your future self) can follow every step.*



### Use version control daily

*Track code and scripts with Git to capture changes, collaborate safely, and roll back when needed.*



### Manage data storage dynamically

*Place files in the right storage tier, migrate data as needed for a task, clean up temporary files, and compress or archive results to stay within quotas.*



### Test pipelines with small data

*Validate workflows quickly with small datasets before scaling up to full research data.*



</div>

## Practical guidance

<div class="usa-accordion ul" style="margin-top: 1em;">
### Templates for Bioinformatics Pipelines

</div>

{% if page.takeaways %}
## Best Practices

<ul>
  {% for takeaway in page.takeaways %}
    <li>{{ takeaway }}</li>
  {% endfor %}
</ul>
{% endif %}


