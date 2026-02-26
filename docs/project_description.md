# Project summary
In 2022, the University of Edinburgh purchased the licence for four digital newspaper collections from the *ProQuest Historical Newspapers* database: *The Scotsman, The Guardian, The Washington Post*, and *The New York Times*. In total, the collection contains over 3.2 terabytes of .PDF scans and .XML metadata from issues published from the eighteenth century to the present. Although the collections are digitised and licensed for research use by members of the University of Edinburgh community, they are not readily usable for large-scale computational analysis. Two structural limitations constrain their accessibility: inconsistent OCR quality, particularly in earlier issues; and complex file structure and storage format. 

This project proposes a pilot study designed to address these challenges by trialling a large language model (LLM)-augmented OCR error correction pipeline on a subset of early nineteenth century articles from *The Scotsman* illustrative of the types of complex OCR errors commonly encountered in historical archives. The pipeline is composed of three stages: image preprocessing, OCR scanning, and post-OCR error correction. If successful, this workflow could be expanded to improve the FAIRness (Findability, Accessibility, Interoperability, and Reusability) of the broader ProQuest dataset, increasing its usability for computational research within the University of Edinburgh community. The pipeline could also be adapted by other institutions facing similar challenges in large-scale digitised archives. 

High-performance computing (HPC) is central to this project, particularly for designing, fine-tuning, and deploying LLM-based post-OCR correction models. The implementation described here uses the University of Edinburgh’s HPC cluster, Eddie, though the workflow can be adapted to other academic HPC environments.

# Background and context
In digital cultural heritage, optical character recognition (OCR) has facilitated the mass digitisation of texts and archives. Though digitisation has made it easier for researchers to access collections, OCR errors that arise during the scanning process are common in digitised texts, especially in historical materials. OCR errors affect not only readability of a particular text, but also whether—and how—it might be computationally analysed. For instance, studies have shown that a character error rate (CER) of just 5% can affect information retrieval (de Olivera et al., 2023), while CER of 10-20% can impact data pre-processing (van Strien et al., 2020) and text analysis (Hamdi et al., 2020). 

The simplest OCR corrective methods are lexicon-based, where words in a document are compared to those in a dictionary or other comprehensive word list. However, lexicon-based methods are challenging to implement in archives that span centuries due to variances in language over time. Additionally, OCR errors often arise from layout misrecognition rather than character misrecognition; in such cases, more complex methods from machine learning have been explored, but they require significant technical expertise  and large datasets for training. As such, there is currently no accessible method for addressing complex OCR errors in historical archives.

# Research process
1. Identify a subset of articles from early issues of *The Scotsman* to serve as the dataset for the study.
2. Manually transcribe the articles to create 'ground truth' for estimating OCR accuracy.
3. Calculate the baseline OCR accuracy for the dataset by applying the CER and WER formulas.
4. Enhance the images in the dataset by removing noise, increasing contrast and sharpening.
5. Extract OCR from the articles in the dataset using OCR engines, and compare the results to the ProQuest OCR.
6. Conduct a test case of prompting LLMs to correct OCR in the dataset, and comparing the results to the OCR engines and ProQuest OCR.
7. Based on the findings from the test case, fine-tune an LLM for OCR error correction.
8. Compare the results of the fine-tuned and 'base' LLM for OCR error correction.
9. Review the findings.

![diagram of the pipeline](/imgs/pipeline.png)

# References
  
