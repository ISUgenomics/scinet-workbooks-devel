---
title: File system on Atlas and Ceres supercomputers
description: ""
type: reference material
author: Aleksandra Badaczewska
index: 
order: 2

header:
  overlay_image: 07-wrangling/assets/img/07_data_acquisition_banner.png
svg: /genomics.svg

objectives: 
  - Understand the structure of the file system on SCINet and the role of each storage location.
  - Learn best practices for storing, organizing, and managing research data.
  - Recognize policies for shared storage, including purge rules and quotas.
  - Identify where to find reference datasets and container images to support reproducible research.

applications:
  - Using the <b>/home</b> directory for scripts, personal settings, and lightweight files.
  - Organizing large-scale datasets and results in the project directory for research groups.
  - Leveraging the <b>/90daydata/shared</b> for collaboration, with awareness of the 90-day purge policy.
  - Accessing curated <b>/reference/data</b> for common scientific datasets without duplicating storage.
  - Running analyses with standard <b>/reference/containers</b> for consistent software environments.

takeaways: 
  - The <b>/home</b> directory is for personal use and lightweight files; keep it uncluttered.
  - The <b>/project</b> directory is the main space for ARS research projects.
  - The <b>/90daydata/shared</b> directory enables collaboration but files are purged 90 days after their last timestamp.
  - The <b>/reference/data</b> provides publically available reference datasets to avoid redundant downloads.
  - The </b>/reference/containers</b> supply ready-to-use software environments, supporting reproducibility.
  - Understanding storage tiers and policies ensures smoother workflows and prevents data loss.
  - Clean up regularly and follow best practices to optimize both personal and shared system resources.

overview: [objectives, applications]



---


<div class="highlighted highlighted--basic"><div class="highlighted__body" markdown="1">
An active SCINet account with access to shared and project directories is required to complete this tutorial. 
Basic familiarity with the [Shell interface](/computing-skills/scinet/interfaces) for navigating directories and managing files is also expected.
</div></div>

## Overview

This tutorial introduces the file system on SCINet supercomputers, explaining the purpose of the home, project, and shared directories, as well as the pre-downloaded datasets and container images. It highlights storage policies, and provides guidance on organizing research data effectively.

{% include overviews %}
 

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
Knowing these spaces will help you find reference datasets or software and decide where to place different types of files when setting up a new bioinformatics project. Once you are familiar with these locations, you will be ready to set up your own workspace or working directory for the pipeline.

<div class="process-list ul" markdown="1">

### Home directory

{% include setup/home_dir %}

### Project directory

{% include setup/project_dir %}

### 90daydata/shared

{% include setup/90day_shared %}

### Reference/data

{% include setup/ref_data %}

### Reference/containers

{% include term term="Container" %}
<hr>

{% include setup/ref_containers %}

</div>


{% if page.takeaways %}
## Lesson summary

<ul>
  {% for takeaway in page.takeaways %}
    <li>{{ takeaway }}</li>
  {% endfor %}
</ul>

{% endif %}