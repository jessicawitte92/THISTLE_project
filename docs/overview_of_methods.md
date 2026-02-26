# Methods 

As is common in pilot studies, the methodological direction of the project evolved over the course of the fellowship in response to emergent constraints and findings. In addition to the planned development, several exploratory approaches were considered, which are detailed in chronological order here.

The original research question centred on determining whether LLMs could be used for post-OCR error correction in historical newspaper data. I was also interested in comparing the accuracy of OCR transcription performed by the commonly used engines Tesseract and Transkribus. To calculate OCR accruacy, I planned to use the character error rate (CER) and word error rate (WER) formulas, which are calculated as follows:

CER = (S + D + I) / N  
WER = (S + D + I) / N  

where 
\(S\) = number of substitutions, \(D\) = number of deletions, \(I\) = number of insertions, and \(N\) = total number of characters (for CER) or words (for WER) in the reference (ground truth) text.  

However, an inital test case of both base LLM post-OCR error correction and fine-tuned LLM post-OCR error correction revealed that CER and WER were still well above the 5-10% threshold rates (see `/docs/results.md`). To improve the pipeline, a number of additional methods that incorporated the image data into the OCR correction pipeline were explored. They are explained below.

---

## Exploration of Layout Segmentation

Following discussion with the DISKAH PIs and research software engineer, an alternative methodological direction was explored which would involve adding layout segmentation as a preprocessing step to improve OCR quality. Doing so, we hoped, would both improve OCR extraction before the correction stage and potentially reduce structural errors caused by multi-column layouts.

The image data in *The Scotsman* that I had been working with up until this point in the project was already segemented. However, segmented articles occasionally spanned multiple columns that caused for OCR text extraction across multiple lines. In other cases, it appeared that the layout recognition engine incorrectly identified multiple articles as comprising a single article, as in the example below:

![layout segmentation error example](/imgs/layout_error.png)

Here, the article title has been cut off and rendered as 'No Title' in the metadata because the title that *does* appear--'OIL AND COAL GAS.'--is actually from the subsequent article on the page. As a result, the OCR text not only contains errors but has also spliced together text from several articles. 

Unfortunately, after several conversations considering how to best incorporate layout segmentation into the pipeline, *The Scotsman* dataset was deemed unsuitable for this approach. This was primarily due to the poor quality and low resolution of the full-page scans. Here is an example:

![full page of the scotsman](/imgs/scotsman-page.png)

The text itself is not only very small and often blurry, but there appears to be significant noise and ink bleeding or smudging, particularly around the newspaper's logo. At this stage of the project, I again attempted to get in touch with ProQuest to find out a) whether any alternative higher-quality resolution images existed and b) how (and when) these scans were conducted, but I did not hear back. Unfortunately, these roadblocks made it difficult to further pursue layout segmentation in the final workflow.

---

## Consideration of Alternative Datasets

At multiple points in the project, the possibility of switching to a higher-quality dataset was explored in order to facilitate segmentation or benchmarking. However, additional issues were encountered here. 

First, OCR accuracy evaluation requires reliable ground truth. There are very few datasets available that contain images of texts from historical archives alongside highly accurate transcriptions. One such dataset is the [*Old Bailey Online*](https://www.oldbaileyonline.org) project. However, the items in this collection are typically a single page and single column, and I was unable to identify a suitable training dataset for layout segmentation that would be mappable to *The Scotsman* dataset. Creating ground truth for other datasets is very time intensive and I had already performed this step for a sample of articles from *The Scotsman*.

Second, working with a cleaner dataset risked shifting the research focus away from the core problem, which was improving very messy OCR from historical collections. To retain alignment with the original research question, the decision was made to continue working with the *Scotsman* dataset.

---

## Baseline OCR Accuracy Estimation

Because CER and WER require comparison against manually transcribed ground truth, an additional exploratory approach was developed which used an NLP-based dictionary comparison method to estimate baseline OCR quality. This method works by comparing tokens in the dataset to the Oxford English Dictionary and flagging unmatched tokens against an English lexicon as a proxy indicator of likely OCR degradation. The aim was not to replace CER/WER benchmarking, but to get a general understanding for the quality of the OCR in the full archive of *The Scotsman* and, hopefully, to identify a subset of articles that were of better quality that could be used to train a layout segmentation engine or otherwise to improve the pipeline. This approach could also identify sections of the collection that required cleaning and those that were ready for research, which could contribute to the goal of making the data more accessible for University of Edinburgh researchers even without applying additional post-OCR cleanup.

I applied the method detailed in the [dictionary-check repository](https://github.com/emack463/dictionary-checker) developed by Ed Mackenzie. The results were somewhat comparable to the estimation compelted with CER/WER calculatsions (see `/docs/results.md`) but also raised new questions. For instance, many of the unmatched tokens referenced local place names, people, or Scots words and idioms. This approach could support future development on the pipeline by automating the correction of some of these misrecognised characters and words, but would benefit from adding an additional Scots lexicon and list of Scottish places and people from the time period.

---
