---
title: Project setup on SCINet’s supercomputers
description: ""
type: interactive tutorial
author: Aleksandra Badaczewska
index: 
order: 3

header:
  overlay_image: 07-wrangling/assets/img/07_data_acquisition_banner.png
svg: /genomics.svg

objectives: 
  - Learn how to structure and organize a research project on SCINet using best practices.
  - Understand the role of project directory trees, pipeline workspaces, and working directories.
  - Learn how $TMPDIR works as scratch space on a compute node and when to use it.
  - Practice creating documentation (README files) for clarity, reproducibility, and collaboration. 

applications:
  - Creating a <b>/project</b> directory tree with clear separation of raw data, software and pipeline versions. 
  - Building pipeline workspaces for different analysis versions or variant that include nested subdirectories for analysis steps.
  - Defining a working directory for active job execution within the project directory, large short-term storage, or scratch space on compute node.
  - Using <b>$TMPDIR</b> for temporary scratch storage during runtime, while ensuring results are copied back to persistent storage.
  - Writing <b>README</b> files at the project and workspace levels to document workflows and decision points.
  - Ensuring reproducibility and collaboration by maintaining organized structures and consistent documentation.

terms: [Project Directory, Directory Tree, Pipeline Workspace, Working Directory, Scratch Space, Documentation, README]

takeaways: 
  - A well-structured directory tree (project setup) is the foundation of efficient and reproducible research projects. 
  - Pipeline workspaces with nested subfolders make it easier to manage multiple analysis versions or variants.
  - The working directory determines where jobs are executed and files saved. Choose <b>/project</b>, <b>/90daydata</b>, or <b>$TMPDIR</b> based on performance and retention needs.
  - <b>$TMPDIR</b> provides high-performance scratch storage but must be used carefully; copy outputs back to permanent storage after jobs finish. 
  - <b>README</b> files act as living documentation, helping both current and future collaborators understand the project layout, workflow choices, and data handling.

overview: [objectives, applications, terminology]

---


<div class="highlighted highlighted--basic"><div class="highlighted__body" markdown="1">
An active SCINet account with access to shared and project directories is required to complete this tutorial. 
Basic familiarity with the [SCINet File System](/computing-skills/scinet/file_system) and [Shell interface](/computing-skills/scinet/interfaces) for navigating directories and managing files is also expected.
</div></div>


## Overview

This tutorial introduces best practices for setting up research projects on SCINet supercomputers, covering directory structures, pipeline workspaces, working directories, scratch space usage, and documentation with README files to ensure clarity and reproducibility.

{% include overviews %}


## Getting started

First, you will log in through **Open OnDemand (OOD)** and use the shell to access SCINet file system locations.

<div class="usa-accordion" style="margin-top: 1em;">

{% include accordion title="<h3 style='margin: 0; border-bottom: none; font-size: 1.1em; display: inline;'>Access the shell via SCINet OOD</h3><span style='font-weight: 300;'>(required) used for accessing supercomputers</span>" class="primary" controls="access-scinet" icon=false %}
<div id="access-scinet" class="accordion_content" markdown='1' hidden> 
{% include setup/scinet_access %}
{% include setup/scinet_login %}
{% include setup/ood_shell %}
</div>
</div>

The goal is to help you manage research projects efficiently by choosing the right location for your pipeline workspace and structuring it wisely, making effective use of temporary storage within system limits, and maintaining clear documentation for reproducibility and collaboration.


## Manage project structure

<div class="highlighted highlighted--basic"><div class="highlighted__body" markdown="1"> 
A **workspace** is a [directory tree](#directory-tree) created by a user to organize input data, scripts, intermediate files, and results for a specific analysis, pipeline, or computational task. A **working directory** is the active location (usually within a workspace) where computations are executed and outputs are written during a run.

</div></div>

Once you have access to the specific [ARS Research Project](/computing-skills/scinet/file_system#project-directory) (i.e., `<project_name>`), you can create your own nested directories under the locations assigned to that project:
- **/project/**`<project_name>` on supercomputers (Atlas, Ceres)
- **/90daydata/**`<project_name>` on supercomputers (Atlas, Ceres) 
- **/LTS/project/**`<project_name>` on archive storage (Juno)

The number of nested workspaces is not limited, but it is important to structure them consistently to keep the project manageable and reproducible.

<div class="process-list ul" markdown="1">

### Directory Tree

A well-organized **directory tree** for your ARS research project, makes it easier to manage data, scripts, and results, while keeping personal and shared resources clearly separated. Below is an example structure you can adapt:
```bash
/project/<project_name>/
├── user1/                    # personal configs, installations, project-related notes
├── user2/
├── software/                 # group-shared tools or container recipes
├── scripts/                  # group-shared scripts or workflows
├── raw_data/                 # group-shared input data for analyses
├── research/                 # container for computations and analysis
│ ├── pipeline_X_v1_date/     # e.g. RNASEQ_deseq_05_25
│ ├── pipeline_X_v2_date/     # e.g. RNASEQ_svm_07_25
│ ├── analysis_Y_v2_date/     # e.g. compare_deseq_svm_09_25
│ ├── ...
│ └── README.md               # top-level description of the project structure and decision points
└── README.md                 # description of the project aims     
```
A good practice is to start with a **personal folder** for each user in the group, where they keep their own configurations, installations, or notes. 
Create separate **software/** and **scripts/** folders for tools, shared code, and configuration files that everyone in the group can use. 
For research activities:
  - maintain a **raw_data/** directory shared between group members,
  - create a top-level **research/** or **analysis/** directory as a parent for all computations, and
  - a separate [workspace](#pipeline-workspace) for each **pipeline iteration** or **analysis variant**. 

*This makes it easy to track changes, repeat analyses, and identify which results came from which run.*

<div class="highlighted highlighted--tip"><div class="highlighted__body" markdown="1"> 
<details markdown="1"><summary>A similar structure can also be mirrored within each user’s personal workspace.</summary> 

```bash
/project/<project_name>/
├── user1/
│ ├── software/              # user-installed tools
│   ├── envs/                # virtual environments (e.g., conda, venv)
│ ├── scripts/               # user scripts or workflows
│ ├── workdir-analysis-1     # e.g. RNASEQ_ref_genome_X
│ ├── workdir-tool-test      # e.g. diff_expression_deseq
│ └── README.md              # brief description of pipeline_v1 contents
...
```

This allows individual users to test tools or develop pipelines independently, while still following the same organizational pattern used by the group. Keeping personal workspaces consistent with the shared project structure makes it easier to move analyses between private and collaborative areas when needed. 
</details>
</div></div>

<div class="highlighted highlighted--highlighted"><div class="highlighted__body" markdown="1"> 
In the **/90daydata/** location, only create workspaces for selected pipelines or analyses that require large data loads or heavy computations with large intermediate files exceeding the project quota.   
- Scripts and tools can be called directly from their original location in **/project/** (with proper permissions). The working directory and output paths should be set to the **/90daydata/** location within your job submission script.  
- Once computations are completed, move the key outputs to **/project/** or archive them on Juno.
</div></div>

### Pipeline Workspace

A workspace should be created for each pipeline run, analysis variant, or other substantial computational task that involves managing files and commands in an organized way. Such a folder provides a structured layout to keep inputs, intermediate files, and results organized step by step. 

```bash
/project/<project_name>/
├── pipeline_X_v1_date/     # pipeline iteration, e.g., RNASEQ_deseq_05_25
│ ├── 01_step/              # e.g. 01_quality_control
│ ├── 02_step/              # e.g. 02_alignment
│ ├── 03_step/              # e.g. 03_quantification
│ ├── 04_step/              # e.g. 04_dge
│ ├── final_report/         # summary statistics, final tables, selected plots and figures, etc.
│ └── README.md             # order of steps and description of inputs and requirements
...
```

Each numbered subfolder corresponds to a sequential stage in the analysis, while parallel steps can be distinguished with letters. A [README.md](#readme) file at the pipeline root should record the order of steps, required inputs, special parameters, and the reasoning behind key decisions. This structure and documentation makes the analysis easier to reproduce and to share with collaborators.  

### README

Each layer *(analysis step)* in the [pipeline workspace](#pipeline-workspace) should ideally include a **README** file. These files act as lightweight documentation that explain the folder’s purpose, contents, and how it fits into the overall workflow. A clear and consistent `README` makes it easier for group members (and your future self) to understand what has been done, reproduce results, and continue work without confusion.  

Even short notes on inputs, outputs, and tool versions can prevent mistakes and save time. It is good practice to revisit and update the `README` each time you interact with the workspace (e.g., when checking the run status, troubleshooting issues, or reviewing and analyzing outputs), so that the documentation stays current and reflects what was actually done.  

<div class="highlighted highlighted--note"><div class="highlighted__body" markdown="1"> 
<details markdown="1"><summary>See a template for a well-structured README for a pipeline workspace</summary>
This format keeps each pipeline run transparent, reproducible, and easy to hand off to collaborators.
```markdown
# Pipeline Workspace: <pipeline_name> 
- version/variant: [...] 
- date: [...]
- location: e.g., Ceres:/project/<project_name>/path_to/<pipeline_workspace>

## Purpose
Briefly describe the analysis or pipeline variant this workspace represents.

## Data
- **Inputs:** list input files and their source (e.g., `/project/.../raw_data/sample_R1.fastq.gz`)
- **Outputs:** summarize expected outputs from this pipeline run
- **Intermediate files:** note which are safe to delete after completion

## Steps
1. **01_quality_control/**  
   Tool: FastQC v0.12.1  
   Parameters: default  
   Inputs: `sample_R1.fastq.gz`, `sample_R2.fastq.gz`  
   Outputs: `sample_R1_fastqc.html`, `sample_R2_fastqc.html`  

2. **02_alignment/**  
   Tool: BWA v0.7.17  
   Parameters: `-M`  
   Inputs: FastQC-checked FASTQs  
   Outputs: `aligned_reads.sam`  

3. **03_variant_calling/**  
   Tool: GATK v4.4.0.0  
   Parameters: `HaplotypeCaller` defaults  
   Inputs: aligned BAM  
   Outputs: VCF file with variants  

## Environment
- Modules loaded: `fastqc/0.12.1`, `bwa/0.7.17`, `gatk/4.4.0.0`
- Conda environment file: `/project/<project_name>/software/envs/pipeline_env.yml`

## Notes
- Special parameters or decisions (e.g., why a specific version was chosen)
- Known issues or limitations
- Suggestions for next iteration
```
</details>
</div></div>

<div class="highlighted highlighted--tip"><div class="highlighted__body" markdown="1"> 
<details markdown="1"><summary>A README can be created as either a plain text (.txt) or a Markdown (.md) file. </summary>
Both formats can be written and edited with simple command-line editors such as `nano` or `vim`, making them lightweight and easy to maintain on an HPC system. Using the `.md` extension allows you to take advantage of **Markdown syntax** (e.g., headings, bullet lists, code blocks). This is optional but highly recommended, because Markdown files are automatically rendered into a clean, human-readable format on GitHub and in modern editors with a graphical interface (such as Visual Studio Code). This makes the documentation easier to read and more accessible to collaborators. 
</details>
</div></div>

</div>

## Manage storage

<div class="process-list ul" markdown="1">
### Working Directory

A working directory is the active location where computations are executed and where intermediate or temporary files are written by default. In many cases, this is the same location as the [pipeline workspace](#pipeline-workspace) in the **/project/** location. You can run commands directly from the working directory or submit SLURM scripts while inside it.  

The best practice is to define the working directory variable (`WORKDIR`) inside the submission script and reference it in the code (`$WORKDIR`). Absolute paths will always point to the same location, no matter where the submission script is located or from where it is submitted. 

```bash
# define working directory (absolute path) and move into it
WORKDIR=/project/<project_name>/pipeline_v1_date/01_step
mkdir -p $WORKDIR
cd $WORKDIR
```

<div class="highlighted highlighted--note"><div class="highlighted__body" markdown="1"> 
<details markdown="1"><summary>See an example of a complete submission script with WORKDIR setup</summary>
```bash
#!/bin/bash
#SBATCH --job-name="blastp"   # name of this job
#SBATCH -p short              # name of the partition (queue) you are submitting to
#SBATCH -N 1                  # number of nodes in this job
#SBATCH -n 40                 # number of cores/tasks in this job, you get all 20 physical cores with 2 threads per core with hyper-threading
#SBATCH -t 01:00:00           # time allocated for this job hours:mins:seconds
#SBATCH --mail-user=emailAddress   # enter your email address to receive emails
#SBATCH --mail-type=BEGIN,END,FAIL # will receive an email when job starts, ends or fails
#SBATCH -A slurm_account_name # name of the slurm account - same as the SCINet project name
#SBATCH -o "stdout.%j.%N"     # standard output, %j adds job number to output file name and %N adds the node name
#SBATCH -e "stderr.%j.%N"     # optional, prints our standard error

# define an absolute path to working directory and move into it
WORKDIR=/project/<project_name>/pipeline_v1_date/01_step
mkdir -p $WORKDIR
cd $WORKDIR

# load required modules, if applicable
module load fastqc/0.12.1

# run the tool, writing outputs into the working directory
INPUTS=/project/<project_name>/raw_data   # absolute path to raw inputs
date                                      # optional, prints out timestamp at the start of the job in stdout file
fastqc ${INPUTS}/sample_R1.fastq.gz ${INPUTS}/sample_R2.fastq.gz \
       --outdir $WORKDIR
date                                      # optional, prints out timestamp when the job ends
```
</details>
</div></div>

For tasks that generate large data loads and heavy intermediate files exceeding the project quota, the **/90daydata/** location on SCINet supercomputers is optimized for this purpose. When submitting jobs, set the working directory and output paths to `/90daydata/` so that temporary files are handled efficiently. 
```bash
# define working directory (absolute path) and move into it
WORKDIR=/90daydata/<project_name>/pipeline_v1_date/01_step
mkdir -p $WORKDIR
cd $WORKDIR
```

<div class="highlighted highlighted--highlighted"><div class="highlighted__body" markdown="1"> 
Scripts, tools, and reference data should remain in their permanent locations under **/project/**, while only runtime files and outputs are written into the working directory.
</div></div>

### $TMPDIR

As an additional option for tasks with large storage requirements, each **compute node** (only!) provides temporary local space in `$TMPDIR` (~1.5TB) that can be used as a [working directory](#working-directory). This storage is useful for I/O-intensive workflows that extensively use disk space reading and writing multiple small files. It is usable only while the job runs in the interactive session or in batch-mode. Be sure to copy final results to permanent storage before the job ends, as `$TMPDIR` is automatically cleared when the job concludes. 

<div class="highlighted highlighted--note"><div class="highlighted__body" markdown="1"> 
<details markdown="1"><summary>See an example of a complete submission script with $TMPDIR setup</summary>
```bash
#!/bin/bash
#SBATCH --job-name="fastqc_tmp"   # name of this job
#SBATCH -p short                  # partition (queue)
#SBATCH -N 1                      # number of nodes
#SBATCH -n 4                      # number of cores (adjust for tool)
#SBATCH -t 01:00:00               # time limit
#SBATCH --mail-user=emailAddress
#SBATCH --mail-type=BEGIN,END,FAIL
#SBATCH -A slurm_account_name     # SCINet project name
#SBATCH -o "stdout.%j.%N"         # standard output
#SBATCH -e "stderr.%j.%N"         # standard error

# define aboslute paths for permanent storage locations
INPUTS=/project/<project_name>/raw_data
RESULTS=/project/<project_name>/pipeline_v1_date/01_step
mkdir -p $RESULTS

# load module
module load fastqc/0.12.1

date      # optional: print timestamp for job start
# OPTIONAL: copy inputs into $TMPDIR (local node storage, faster I/O)
cp ${INPUTS}/sample_R1.fastq.gz ${INPUTS}/sample_R2.fastq.gz $TMPDIR

# move into $TMPDIR and run tool there
cd $TMPDIR
fastqc sample_R1.fastq.gz sample_R2.fastq.gz --outdir $TMPDIR

# copy $TMPDIR/<final_results> back to permanent storage
cp $TMPDIR/*fastqc* $RESULTS
date      # optional: print timestamp for job end
```
</details>
[Temporary Local Node Storage](https://scinet.usda.gov/guides/data/storage#temporary-local-node-storage) (SCINet User Guide) provides detailed instructions on staging data in and out of `$TMPDIR`. 
</div></div>

</div>


{% if page.takeaways %}
## Best Practices

<ul>
  {% for takeaway in page.takeaways %}
    <li>{{ takeaway }}</li>
  {% endfor %}
</ul>
{% endif %}