# Table of Contents

- [What is an HPC?](#what-is-an-hpc)
- [HPC Tiers](#hpc-tiers)
- [Prerequisites for Working with HPC](#prerequisites-for-working-with-hpc)
- [General Tips](#general-tips-for-hpc)
- [Adapting Code for an HPC Cluster (SLURM)](#adapting-code-for-an-hpc-cluster-slurm)
- [THISTLE Code Examples](#thistle-code-examples)
- [Resources](#resources)

### What is an HPC? 
An HPC (high performance computing) cluster provides remote access to high-powered computers to run large-scale or resource-intensive computational tasks. It can be a bit of a learning curve to adapt workflows for HPC environments. You will first need to remotely connect to a system before you can submit your code, which will then be placed in queueing system for processing as a 'job'.

### HPC Tiers
In the UK, HPC infrastructure is organised into tiers. Tier 1 HPC systems include national supercomputers, Tier 2 systems are regional, and Tier 3 are institutional. THISTLE was developed using the University of Edinburgh's Eddie HPC cluster, which is a Tier 3 resource.

Some projects will require Tier 2 or Tier 1 HPC access, which may require first applying for access and/or funding. Tier 3 access is usually free for users who are members of the providing institution, but the option to 'skip the queue' for a fee may be available.

# Prerequisites for working with HPC
To work with an HPC, you will ideally have some familiarity with:
1. Unix/Linux command line basics — navigating directories, moving files, running scripts. The Software Carpentry shell tutorial [here](https://swcarpentry.github.io/shell-novice/01-intro.html)
2. SSH access and how to login to the remote server from a terminal (e.g. Terminal on MacOS or MobaXterm on Windows). For a University HPC, this is likely your institutional email and password, but check with your information services team if you are unsure.
3. If working remotely, access to the University's (or HPC provider's) VPN 
4. The difference between interactive and batch jobs
5. HPC concepts such as nodes and modules; CPUs/GPUs/memory/RAM; parallel processing 
6. File storage and management structures, both locally (on your computer) and remotely

# General tips for HPC

- Test before scaling up. Before submitting your job in full, run the script interactively on a small subset.
- Request realistic resources. Over-requesting memory or CPUs can push your job down the queue. For this pipeline, 4 CPUs and 16GB RAM is sufficient for most batch sizes; scale up if processing thousands of high-DPI scans.
- SLURM module names vary by cluster. Commands like `module load tesseract` may differ on your specific system. If modules are not available, contact your HPC support team; they can usually install commonly needed tools.
- Ollama is not suitable for HPC. The LLM post-processing notebook uses Ollama, which is designed for interactive local use and cannot easily be run as a SLURM batch job. Instructions for adapting this notebook for an HPC environment are provided in the sections below.

# Adapting code for an HPC Cluster (SLURM)
If you are working with a large dataset or LLM, running your code on a high performance computing (HPC) cluster will be significantly faster than running it on a laptop or in Colab. This section explains how to adapt the notebooks in this repository (`/code`) for a typical university HPC cluster running the SLURM job scheduler. 

HPCs can be slightly different but the overall process will be: convert the Jupyter notebook to a plain Python script, set up your Python environment on the cluster, and submit the script to the queueing system as a job.

# Setting up a Python environment on an HPC cluster 
## Step 1: Convert the notebook to a Python script
Jupyter notebooks (.ipynb files) cannot be submitted directly to SLURM. You can convert them using the following command (run locally or on the cluster login node):
```
jupyter nbconvert --to script ocr_preprocessing.ipynb
```
This produces `ocr_preprocessing.py.`

You can also download the notebooks from Colab or Jupyter as .py files. Go to 'File' > 'Download' > 'Download .py' to do so.

## Step 2: Set up your Python environment on the cluster
HPC clusters use a 'modules' system to manage software. Rather than installing packages system-wide (which you likely won't have permission to do), you will first load libraries and packages and then create a personal virtual environment.

First, log in (or 'ssh') in to the cluster. Then, follow these steps:

1. Check to see if Python is available, and which version(s)
   
``` 
module avail python
``` 
2. Load Python:

``` 
module load python/3.10
```

3. Check whether dependencies and requirements are available as modules...for instance:

``` 
module avail tesseract
module avail poppler
```

4. If available, load them:

``` 
module load tesseract
module load poppler
```

If dependencies are not available, you will need to install them in your virtual environment.

4. Create a virtual environment in your home or project directory:

``` 
python -m venv ~/envs/ocr_env
``` 
5. Activate the virtual environment and install Python libraries 

``` 
source ~/envs/ocr_env/bin/activate
pip install pdf2image pytesseract pillow matplotlib
``` 

Note: Many clusters have a dedicated project or 'scratch' storage area that is faster and has more quota than your home directory. Store your input images and output files there (e.g. /scratch/your_username/ or /project/your_project/), not in your home folder. 


## Step 3 — Handle file paths on the cluster
File paths on an HPC cluster are different from your local machine and need to be absolute. Update these variables at the top of ocr_preprocessing.py...and be sure to use absolute paths on the cluster rather than relative paths like '../data'
```
img_folder  = '/scratch/your_username/newspaper_scans/'   # input images
output_path = '/scratch/your_username/ocr_results/ocr_output.txt'  # OCR text output
fig_dir     = '/scratch/your_username/ocr_results/figures/'         # saved comparison plots
```
Create output directories if they don't exist:

```
import os
os.makedirs(os.path.dirname(output_path), exist_ok=True)
os.makedirs(fig_dir, exist_ok=True)
```

Before submitting your job, copy your input data to the cluster using scp or rsync from your local machine. Copy local scan folder to the cluster scratch space. The path to your 'scratch' or working directory will have a unique path depending on your cluster.

## Step 4 — Write a SLURM job submission script
Create a file called run_ocr.sh in the same directory as your Python script:

```
#!/bin/bash
#SBATCH --job-name=ocr_pipeline          # name of the job, which will be shown in the job queue
#SBATCH --output=logs/ocr_.out         # output log name
#SBATCH --error=logs/ocr_.err          # error log name
#SBATCH --time=04:00:00                  # maximum processing time (HH:MM:SS)
#SBATCH --mem=16G                        # memory per node 
#SBATCH --cpus-per-task=4               # number of CPU cores 

# Load required modules (update module names to match your cluster)
module load python/3.10
module load tesseract
module load poppler

# Activate your virtual environment
source ~/envs/ocr_env/bin/activate

# Create log directory if it doesn't exist
mkdir -p logs

# Run the script
python ocr_preprocessing.py

# Submit the job from the cluster login node:
sbatch run_ocr.sh

## Monitor its progress with:
squeue --me                  # view your jobs in the queue
tail -f logs/ocr_<jobid>.out # stream the output log in real time
```
# Resources
- [batchspawner for JupyterHub](https://github.com/jupyterhub/batchspawner)
- [Software Carpentry Shell tutorial](https://swcarpentry.github.io/shell-novice/01-intro.html)
- [Adapting Jupyter Notebook for academic HPC, video example](https://www.youtube.com/watch?v=TqkuyM5zFag)

