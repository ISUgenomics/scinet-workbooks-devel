---
title: Project setup on SCINet’s supercomputers 
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
  - Understand the key components of setting up a bioinformatics project on an HPC environment.
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
  - Start with a clear project structure: separate raw data, processed results, scripts, and logs.
  - Use README.md, environment files, and changelogs to document workflows and project decisions.
  - Understand the purpose and limitations of different storage tiers (e.g., scratch vs. home vs. archive) and clean up intermediate files regularly.
  - Monitor your quota usage and be aware of retention or purge policies on shared resources.
  - Use version-controlled repositories (e.g., GitHub) to manage pipeline templates, analysis scripts, and documentation.
  - Document software versions and module configurations to ensure reproducibility.
  - Always test pipelines with small data subset before scaling up.

overview: [objectives, applications, terminology]


---


<div class="highlighted highlighted--basic"><div class="highlighted__body" markdown="1"> 
This tutorial assumes basic familiarity with **HPC environment**, such as OOD/SSH login and navigation in the CLI. 
No prior experience with bioinformatics pipeline development or workflow languages is required. 
Familiarity with the [command line](/computing-skills/) and basic Unix file systems is expected.
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



## SCINet file system

<div class="highlighted highlighted--basic"><div class="highlighted__body" markdown="1"> 
A **file system** is the structure an operating system uses to organize, manage, and access data on storage devices. 
It defines how key directories are arranged under the system's root, how files are stored within those directories, and how they are accessed by users and programs.
</div></div>

Effective use of an HPC system starts with understanding how its file system is structured. 
**Atlas and Ceres supercomputers provide separate storage locations optimized for different tasks**, such as managing private project data, 
running analyses on large datasets, and accessing software containers or shared reference databases. 
Knowing where these locations are helps prevent slowdowns, quota issues, and data loss.

Each location listed in a table is linked to detailed documentation, where you can find information on usage guidelines, 
[storage quotas](https://scinet.usda.gov/guides/data/storage#quotas), [access permissions](https://scinet.usda.gov/guides/data/storage#changing-file-permissions), and retention rules specific to that space.

{% include table no-row-labels=true caption="Key file system locations on SCINet supercomputers and their intended use" content="| path | name | description | permsissions |
|------|------|-------------|--------------|
| `/`  | root | base of the file system hierarchy | read-only |
| `/home/<user>` | [home dir](https://scinet.usda.gov/guides/data/storage#home-directories) | user's configurations and login files; 30GB quota | user-private |
| `/project/<research_project>` | [project dir](https://scinet.usda.gov/guides/data/storage#project-directories) | main storage for research projects; 1TB quota | group-private |
| `/90daydata/<research_project>` | [project 90day](https://scinet.usda.gov/guides/data/storage#large-short-term-storage) | large short-term storage and coputation workspace; no quota limit | group-private |
| `/90daydata/shared/` | [90day shared](https://scinet.usda.gov/guides/data/storage#large-short-term-storage) | cross-group sharing and collaboration | open to all |
| `/reference/data/` | datasets | pre-downloaded databases | open to all |
| `/reference/containers/` | [containers](https://scinet.usda.gov/guides/software/singularity#ceres-container-repository) | pre-downloaded software containers | open to all |
| `/reference/SCINetPolicies` | [SCINet&nbsp;Policies](https://scinet.usda.gov/about/policies) | policies and rules for SCINet supercomputers, CLI-searchable | open to all |" %}

{% include alert class="tip" content="Understanding where to store your data or access shared data and software ensures that your work complies with [SCINet policies](https://scinet.usda.gov/about/policies) and makes efficient use of available resources." %}


## Getting started

It is useful to take a quick tour of the **key file system locations** available on SCINet supercomputers. 
First, you will log in through **Open OnDemand (OOD)** and use the shell to explore the main directories.

<div class="usa-accordion" style="margin-top: 1em;">

{% include accordion title="<h3 style='margin: 0; border-bottom: none; font-size: 1.1em; display: inline;'>Access the shell via SCINet OOD</h3><span style='font-weight: 300;'>(required) used for accessing supercomputers</span>" class="primary" controls="access-scinet" icon=false %}
<div id="access-scinet" class="accordion_content" markdown='1' hidden> 

{% include setup/scinet_access %}
{% include setup/scinet_login %}
{% include setup/ood_shell %}

</div>
</div>

The goal is to understand what each location is meant for and how it fits into the workflow of building and running bioinformatics pipelines. 
Knowing these spaces will help you find reference datasets or software and decide where to place different types of files when setting up a new bioinformatics project. Once you are familiar with these locations, you will be ready to set up your own [project workspace](#).

<div class="process-list ul" markdown="1">

### Home directory

{% include setup/home_dir %}

### Project directory

{% include setup/project_dir %}

### 90daydata/shared

{% include setup/90day_shared %}

### Reference/data

{% include setup/ref_data %}

For details on the bioinformatics databases included, see [Databases on SCINet](/bioinformatics/resources/databases#databases-on-scinet) reference material.  

### Reference/containers

{% include term term="Container" %}
<hr>

{% include setup/ref_containers %}

</div>




