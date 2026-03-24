# Code
This directory contains the Jupyter notebooks that implement the THISTLE OCR-processing pipeline. The notebooks implement the following stages:

- Image preprocessing
- OCR text extraction
- Prompt-based LLM correction (test case)
- Optional LoRA fine-tuning
- Deployment of fine-tuned model
- Quantitative evaluation
  

---

# Notebooks

| File name |  Description |
|---|---|
| `img_preprocessing.ipynb` |  Enhances scanned newspaper images and visualises the results compared to originals; extracts text with Tesseract OCR |
| `post_ocr.ipynb` |  Test case using a locally-run LLM via Ollama to correct OCR errors |
| `fine-tuning.ipynb` |  Fine-tunes an LLM downloaded from HuggingFace using a LoRA prompt-tuning approach; deploys the fine-tuned LLM for post-OCR error correction |
| `accuracy_and_results.ipynb` |  Compares OCR accuracy using CER and WER scores; evaluates the overall results |

## Test case notes

`post_ocr.ipynb` implements a prompt-based approach to post-OCR correction using locally-run LLMs. It is possible to run a similar test case using a chatbot (e.g. ChatGPT); however, there are several important differences:

1. The notebook takes a `.csv` file as input and processes an entire dataset in a single code block. This makes it possible to assess a model's performance across a larger sample rather than manually copy-pasting a full dataset into a chatbot.
2. Multiple models can be tested and compared by changing a single variable. Many LLMs, particularly open-access LLMs, are not available as chatbots at the time of writing.
3. Working outside a chatbot interface allows you to adjust model parameters, perhaps most importantly the 'temperature' parameter which controls how 'creative' the model is in its outputs. For post-OCR error correction in historical texts, lower temperatures are likely to be more appropriate.
4. This notebook is designed to answer the question: How well does [model] perform at OCR correction at baseline, without fine-tuning or further adjustments? The WER and CER scores produced here provide a quantitative answer to that question and are an essential step before deciding whether and how to fine-tune, but qualitative review is also very important...especially when diagnosing the types of (and potential reasons for) errors produced.



## How to use the notebooks

**NOTE:** all of the notebooks load files from (and save files to) a project folder where, it is assumed, you have cloned this repo. Before you get started, be sure to confirm the path to the repo (which may be on your local machine, in Google Drive, or on your HPC cluster) and that all paths in the notebooks are correct before you get started. You may need to create a project folder if one does not already exist; you will likely also need to manually update the paths or you will encounter an error. 

You can read more about paths and a computer's file structure [here](https://www.sdmfoundation.org/2023/11/22/understanding-file-pathing/) or [here](https://www.codecademy.com/resources/docs/general/file-paths).

### Baseline workflow
1. Run `img_preprocessing.ipynb` to enhance scanned images and extract OCR text
2. Run `post_ocr.ipynb` to evaluate prompt-based correction using local LLMs.
3. 3. Run `fine-tuning.ipynb` to explore the performance of a fine-tuned LLM.
4. Run `accuracy_and_results.ipynb` to compare OCR quality across results.

### Fine-tuning workflow (optional)
1. Run `post_ocr.ipynb` to establish baseline performance.
2. If improvement is required, use `fine_tuning.ipynb` with a manually transcribed dataset (`data/ground_truth.csv`).
3. Deploy the fine tuned model using `post_ocr_ft.ipynb`.
4. Re-run `accuracy_and_results.ipynb` to compare baseline and fine-tuned results.

All notebooks assume that the repository structure remains intact. The dataset used for fine-tuning and evaluation is located at:
`data/ground_truth.csv`. If you are working locally and have cloned the repository, the dataset will already be available on your computer.

## Prerequisites
Ideally, you will have intermediate experience with Python and be comfortable working in Jupyter notebooks. However, anyone who is comfortable running code blocks in a Python notebook can also use them as a demonstration of what is possible.

You should also have access to a computer capable of running Google Colab or Python via an integrated development environment (IDE). The instructions for opening notebooks in Colab and installing Python via Anaconda can be found in the main README for this repository. 

Fine-tuning LLMs is more efficient with GPUs. The notebooks were tested in Google Colab using the T4 GPU and in an academic HPC environment with GPU allocation.

## Dependencies

### Python libraries
All Python dependencies are installed within the notebooks. Core libraries include:
- pandas
- datasets
- transformers
- torch
- peft
- trl
- accelerate
- bitsandbytes
- jiwer
- pillow
- pytesseract
- matplotlib
- scikit-learn
- huggingface_hub
  
A complete description of libraries and installation instructions is provided in `code/libraries.md`.

### Installing software dependencies locally
#### Tesseract OCR
Required for `img_preprocessing.ipynb`.

```bash
# macOS (via Homebrew)
brew install tesseract

# Ubuntu / Debian
sudo apt-get install tesseract-ocr

# Windows
# Download the installer from: https://github.com/UB-Mannheim/tesseract/wiki
# Then add the installation directory to your system PATH
```
#### Ollama
Required for `post_ocr.ipynb`. Ollama runs LLMs locally — no API key or internet connection is needed once models are downloaded.

Download and install from [ollama.com](https://ollama.com). After installation, start the Ollama application or run `ollama serve` in your terminal before opening the notebook.

### Large language models (LLMs)

In the notebooks, you will be introduced to two methods for working with LLMs. 
The first involves working with models locally on your computer using Ollama; this approach is used in `img_ocr.ipynb` and `post_ocr.ipynb`. 
The second invovles downloading models from HuggingFace, used in fine-tuning.ipynb and post_ocr_ft.ipynb.

Ollama and HuggingFace both offer a large selection of models. If you'd like to try a different model in one of the notebooks, you can browse the full selection for Ollama [here](https://ollama.com/search) and HuggingFace [here](https://huggingface.co/models).

Some HuggingFace models are gated, meaning that you must first make an account and request access before you can use them. You will then need to authenticate your identity with a passcode in the notebook. Instructions for this process are included in the fine tuning notebook.


### About LoRA fine-tuning

Full fine-tuning of LLMs requires updating billions of parameters and is computationally expensive. LoRA instead trains a small number of additional parameters (the 'low-rank adapters') while keeping the original model weights frozen. This dramatically reduces GPU memory requirements and training time while still producing meaningful improvements on domain-specific tasks. The LoRA adapters trained in `fine-tuning.ipynb` are saved separately from the base model and can be merged or loaded alongside it at inference time. 

Here are some resources with more details about this approach: 

Wang et al. (2023). 'LoRA ensembles for large language model fine-tuning' https://doi.org/10.48550/arXiv.2310.00035 

Zhao et al. (2024). 'LoRA Land: 310 Fine-tuned LLMs that Rival GPT-4, A Technical Report' https://arxiv.org/abs/2405.00732 

Gao et al. (2024). 'FashionGPT: LLM instruction fine-tuning with multiple LoRA-adapter fusion' https://doi.org/10.1016/j.knosys.2024.112043 

IBM, 'What is LoRA (low-rank adaption)?' https://www.ibm.com/think/topics/lora

---
## Dataset
The dataset used for training and evaluatino consists of 100 manually transcribed articles from early nineteenth-century issues of The Scotsman (1817–1825). You can find the dataset at `data/ground_truth.csv`. 

## Notes on fine-tuning
- Fine-tuning notebooks demonstrate supervised instruction tuning using LoRA adapters. Only LoRA adapter weights are saved (not the full base model).
- Inference requires loading the base model alongside the saved adapters.
- Fine-tuning parameters (epochs, rank, learning rate) can be adjusted depending on dataset size and GPU capacity.
- GPU hardware may affect processing time but will not affect methodology.
- Preprocessing parameters were tuned for *The Scotsman* dataset; different archives may require parameter adjustment or alternative fine tuning approaches.

## Reproducibility notes
- Calculating WER/CER requires accurate manual transcription.
- Even after fine-tuning, it is possible that model behaviour can be unstable in future iterations.
- Ollama model versions are periodically updated and may affect model behaviour.
- Fine-tuning effectiveness depends heavily on parameter selection, approach, dataset size and prompt structure.
