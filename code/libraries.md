# Libraries and Dependencies
This document lists all Python libraries and external software required to run the notebooks in the `/code` directory, along with installation instructions and a brief explanation of their role in the THISTLE pipeline.

## Python Libraries
Most Python dependencies can be installed using `pip` in the command line or your IDE if working locally. If you are working in Colab, the first cell of each notebook will guide you through the installation process.

### Core Data Processing
**pandas**: Data manipulation and CSV handling.
Used for loading OCR outputs and ground truth datasets. 
 
**datasets**:
HuggingFace dataset management library.
Used in fine_tuning.ipynb for preparing training and validation data. 
 
**scikit-learn**:
Used for dataset splitting (train_test_split).

### OCR and Image Processing
**pillow**:
Image processing library used for preprocessing scanned newspaper images.
 
**pytesseract**:
Python wrapper for Tesseract OCR.
Used in img_ocr.ipynb for text extraction. 
 
**matplotlib**:
Used to visualise preprocessing results and compare image outputs. 
 
### LLMs
**ollama**: 
Python interface for running local LLMs via Ollama. 
Used in
`img_ocr.ipynb` and
`post_ocr.ipynb`.

**huggingface**:
Used for fine-tuning and deployment in
`fine_tuning.ipynb` and
`post_ocr_ft.ipynb`.
 
### Fine tuning
**transformers**: 
Model loading, tokenisation, and inference.
 
**torch**:
PyTorch deep learning framework (GPU support required for fine-tuning).
 
**peft**:
Parameter-Efficient Fine-Tuning (LoRA adapters).
 
**trl**:
Training utilities for supervised fine-tuning (SFTTrainer).
 
**accelerate**:
Device management and distributed training support. 
 
**bitsandbytes**:
Enables 4-bit quantisation (QLoRA) to reduce GPU memory usage.
 
**huggingface_hub**:
Authentication and model download from HuggingFace.
 
### Evaluation
**jiwer**: Computes
Word Error Rate (WER) and
Character Error Rate (CER).
Used in
`accuracy_and_results.ipynb` and
`fine_tuning.ipynb` (validation stage)
