# Technology for Handling and Improving Stubborn Text-Level Errors (THISTLE)

## About THISTLE
THISTLE is a prototype pipeline developed for improving the accuracy of machine-readable text extracted from digitised images using optical character recognition (OCR). Developed over the course of a one-year fellowship supported by the Digital Skills in Arts & Humanities (DISKAH) Network, THISTLE is provided as a research prototype that trials large language models (LLMs) for post-OCR error correction. Though it was designed for a specific dataset--in this case, a dataset of articles published in *The Scotsman* newspaper in the early nineteenth century--THISTLE could be adapted in full or in part for LLM-based OCR error correction of other datasets.

## What's next for THISTLE?

Future development will work to improve the accuracy of THISTLE's OCR correction by exploring alternate approaches to further improve the accuracy of the final product and to compensate for the LLMs' tendencies to generate extraneous information and modernise historical text and spellings. For instance, for *The Scotsman* data specifically, fine-tuning the models on historical English as used in Scotland--which includes variants of Scots alongside local idioms and terms--would further improve the accuracy of results. 

*This work has received support from UKRI (Grant No. APP4595) through the ‘Digital Skills in Arts and Humanities (DISKAH): Transforming Access to Digital Infrastructure and Skills’ project.*

![DISKAH logo](imgs/DISKAH.png)

## About the DISKAH Network and Fellowships
The Digital Skills in Arts and Humanities (DISKAH) Network aims to build capacity amongst Arts and Humanities (A&H) researchers in the use of state-of-the-art national Digital Research Infrastructure (DRI) to foster innovation and collaboration. DISKAH aims to introduce the wider field of A&H to the capabilities and opportunities afforded by computationally intensive data-driven methods and impact hundreds of A&H researchers through planned activities, training dissemination approaches and knowledge exchange events. 

Visit the [DISKAH website](https://culturedigitalskills.org/) to learn more about the network, current fellows and future opportunities.

## About this repository
This repository contains the code required for using THISTLE in addition to materials generated during the development process. It also includes documentation of the potentials and pitfalls of existing NLP-based approaches to OCR error correction, instructions on how to adapt code for working on high-performance computing (HPC) infrastructure and a discussion of the broad challenges in OCR-based digitisation--especially of historical archives. See the Table of Contents at the end of this README for a complete list.

Please note that THISTLE is provided as a research prototype / proof-of-concept. Some components remain in draft or experimental form. This repo reflects the state of the project as of 4 February 2026 and while it will be monitored occasionally, there is a possibility that bugs in the code will appear over time unnoticed. Please contact me if you encounter any difficulties or bugs.


# How to use THISTLE
THISTLE has been tested in both Google Colab and Anaconda. It can be run locally in Python IDEs such as PyCharm and Spyder or via Jupyter Notebooks. 
## Jupyter Notebooks via Google Colab
### Importing the repo to Colab
First, log in to your Google account. If you don't have a Google account, you will need to make one in order to use Colab.

Next, open Google Colab: https://colab.research.google.com

You should see a popup window. Click the 'GitHub' tab on the left side of the window. Here, you will paste the URL of this repository. 

To do this, go to the GitHub header and copy the link to this repo. Then, paste the link into the Colab window and select the notebook you would like to use. Press 'enter' and it should open.

### Using the notebook
Each notebook contains paragraphs of explanatory text interspersed with grey cells containing code blocks. To run a code block and see the result:
1. Place your cursor within the cell. 
2. Click the 'Run' button on the top menu. 
3. The results of running this code will appear below. 
4. If the results do not appear immediately, check the icon in the broswer tab. An egg timer icon indicates the code is processing.
5. It is best to follow the notebook from top to bottom as some code blocks will not run without the results from previous cells. 
6. You can edit code blocks yourself and run them to see the results of your changes.
7. To clear the results and run the code again, you can use the 'cell' menu on the top menu bar. Cell > Current outputs > clear will clear the results of the current cell, and Cell > All output > clear will clear all results.

## Using Python locally
### Installing Python via Anaconda
Python is great for general-purpose programming and is a popular language for scientific computing as well. Installing all of the packages required for this lessons individually can be a bit difficult, however, so we recommend the all-in-one installer Anaconda.

Regardless of how you choose to install it, please make sure you install Python version 3.x (preferably Python 3.11 or higher).

#### Windows - [Video tutorial](https://www.youtube.com/watch?v=xxQ0mzZ8UvA)

1. Open anaconda.com/download with your web browser. 

2. Download the Python 3 installer for Windows. 

3. Double-click the executable and install Python 3 using MOST of the default settings. The only exception is to check the Make Anaconda the default Python option. 

#### macOS - [Video tutorial](https://www.youtube.com/watch?v=TcSAln46u9U)

1. Open anaconda.com/download with your web browser. 

2. Download the Python 3 installer for macOS. 

3. Install Python 3 using all of the defaults for installation. 

#### Starting Python

1. To start Jupyter Notebooks, open the Anaconda Navigator and click on the Jupyter icon.

2. If you wish, you can create a Python virtual environment and install all dependencies for the course by navigating to the course folder in the terminal and running sh setup.sh. 

#### Upload the Notebook

1. Download the notebook on your machine. 
2. Select 'upload'.
3. Navigate to where you have downloaded your file in your directory.  
4. Select 'upload' again. 
4. Double-click on the uploaded file. 


# Table of Contents
This repository contains four directories, each of which includes its own README introducing the materials contained within. The directories are as follows:
## `/docs`
A description and chronological summary of the project is available in the `/docs` directory, including:
- Project description and research questions (`/project_description.md`)
- Research process and methods (`/overview_of_methods.md`)
- Experiments and results (`/results.md`)

## `/data`
A README describing *The Scotsman* dataset and copyright and licensing practices in the UK, along with samples of data used in the THISTLE pipeline: 
- `/ground_truth.csv` contains 100 manually transcribed articles from *The Scotsman* and their corresponding metadata 
- `/imgs` contains .png files of images for the articles in the .csv file as 

## `/code`
All code required to run the THISTLE pipeline formatted as Jupyter notebooks:
1. img_preprocessing.ipynb: enhances and prepares images for OCR  
2. img_ocr.ipynb: extracts text from processed images using Tesseract OCR 
3. post_ocr_llm.ipynb: prompts LLMs to correct the OCR output 
4. fine_tuning.ipynb: fine tunes LLMs for post-OCR error correction using an input dataset 
5. post_ocr_ft.ipynb: deploys fine-tuned LLMs to correct OCR output 
6. ocr_evaluation.ipynb: uses word error rate (WER) and character error rate (CER) to evaluate the accuracy of OCR output against 'ground truth' (in this case, a manual transcription)

## `/HPC`
Instructions on how to adapt the Jupyter notebooks for working on a high-performance computing (HPC) cluster, with a README why (and when) you might want to do so and a description of the role of the University of Edinburgh's Eddie HPC cluster in this project.

# License

This repository is licensed under the Creative Commons Attribution–NonCommercial 4.0 International License (CC BY-NC 4.0).

You are free to:
- Share — copy and redistribute the material
- Adapt — remix, transform, and build upon the material

Under the following terms:
- Attribution — you must give appropriate credit
- NonCommercial — you may not use the material for commercial purposes

See the LICENSE file for full terms.
