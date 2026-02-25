# Data

This document describes the data explored in the project, a subset of which is included in this repository. I discuss copyright and licensing issues regarding both this dataset and historical newspaper data more broadly.

---

## *The Scotsman* Newspaper

*The Scotsman* was founded in Edinburgh in 1817 and is one of Scotland's oldest newspapers. It has been published as a daily newspaper since 1855 and has documented over two centuries of Scottish and British political, cultural, economic and social news. At the time I am writing, all historical issues of *The Scotsman* are available digitised through the *ProQuest Historical Newspapers* database and *The British Newspaper Archive*. The University of Edinburgh's library is currently subscribed to this database; other UK and international academic institutions may also provide access to their users. To check for access, [visit ProQuest](https://www.proquest.com/hnpscotsman1/), [the British Newspaper Archive](https://www.britishnewspaperarchive.co.uk/help-faq/how-can-i-access-the-scotsman-digital-archive-resources) or contact your library for more information.


---

## Copyright, License and Historical Newspapers in the UK

### UK Copyright Law

In the United Kingdom, copyright law is governed primarily by the Copyright, Designs and Patents Act 1988 (CDPA). Under this legislation, literary works — including newspaper articles — are protected for 70 years from the end of the calendar year in which the author died, or, for works of unknown authorship, 70 years from the end of the year in which the work was first made available to the public.

For practical purposes, this means that the vast majority of newspaper content published before the early twentieth century is now in the public domain in the UK. *The Scotsman* issues published from 1817 through to approximately the 1940s are generally considered out of copyright, as their authors have been dead for more than 70 years. Content from the mid-twentieth century onwards remains under copyright and access is subject to licensing agreements.

It is important to note, however, that copyright is distinct from database and publisher licenses. For more information about the TDM exception, the UCL Library has published this [helpful guide](https://www.ucl.ac.uk/library/learning-teaching-support/ucl-copyright-advice/copyright-depth/text-and-data-mining-tdm-exception#tdm-and-publisher-licences) and CLARIN-EU has published this [summary of the policy in the UK and France](https://www.clarin.eu/content/text-and-data-mining-tdm-exceptions-uk-and-france).

---


### The Text and Data Mining (TDM) Exception

UK copyright law includes a specific exception for non-commercial text and data mining research. Under this exception, a lawful user of a work (e.g a researcher with access to a licensed database) may copy, download, and analyse that work computationally without requiring additional permission from the rights holder, provided the purpose is non-commercial research. In practice, however, the TDM exception does not override database subscriptions. For now, if you require access to a paywalled archive, you should start by consulting the digital research librarians at your institution.

---

### ProQuest and the ProQuest Historical Newspapers Database

ProQuest is an American company that licenses and distributes digitised primary materials from archives and libraries. The *ProQuest Historical Newspapers* database is a searchable commercial archive of digitised historical newspaper that provides full-text access to over 200 international publications. Based on UK copyright law, a proportion of the historical materials in this collection are considered to be in the public domain. However, databases such as ProQuest hold a view that the digitisation process is a significant initiative and investment, and the resulting database products are commercialised through institutional subscription licences. 

In other words, although the collections themselves are in the public domain and accessible in legal terms, access is often mediated by commercial databases typically only available to subscribers. In practice, this means that culturally significant historical records that ought to be publicly available are paywalled. This is not a criticism unique to ProQuest, but to all commercial databases; many digitised historical collections are held under similar arrangements. Many academics have, of course, criticised these practices. For the purposes of this project, I want to highlight the importance of open access initiatives for culturally significant historical materials, and how research ethics and legally accepted practices are not always equivalent.

Another challenge in working with digitised materials provided by subscription-based databases is that the availability of materials may change. For instance, a database could provide access for a set period of time; libraries could stop subscribing to databases or certain databases' products; collections may be added, changed, or removed; and so forth. Ensuring perpetual access to an archive only available in a subscription database requires purchasing a license.

---

### Licensing and Licensed Data

The University of Edinburgh has purchased a licence from ProQuest for a selection of historical newspaper collections. The licence grants access to the data for teaching and research purposes to all University of Edinburgh staff, students and registered visitors.

The licence covers the data as structured and delivered by ProQuest, which includes scanned PDF images of individual articles and pages and accompanying XML metadata files. Using the data is subject to the standard terms of the ProQuest licence agreement, which is held by the University Library. According to the terms of the license, access to the collections is not publicly available, even for items in the collections that are out of copyright.  

---


## About This Dataset

### The ProQuest Historical Newspapers Collection at the University of Edinburgh

The University of Edinburgh owns the licence for the following newspaper collections from the *ProQuest Historical Newspapers* database:

| Title | Date Range |
|---|---|
| *The Scotsman* | 25 January 1817 – 31 December 1950 |
| *The Washington Post* / *Times Herald* | 6 December 1877 – 31 December 2008 |
| *The Guardian* / *The Observer* / *The Manchester Guardian* | 5 May 1821 – 31 December 2003 |
| *The New York Times* | 14 September 1857 – 31 December 2021 |

This collection includes 27,316 files in 50 folders, or 3.24 TB of data. The data is currently stored on an internal server that only those with registered University of Edinburgh computing accounts can access. 399GB of this data is of *The Scotsman*.

No details about the digitisation process were included with the purchase and, unfortuantely, the license does not provide any documentation. Over the course of the project, I made several attempts to learn how *The Scotsman* was digitised, including reaching out to ProQuest, but my efforts were unsuccessful. I also could not find evidence of a quality check of the data itself or the OCR conducted before the license was purchased.

As those who are familiar with digitsed archives will know, the older items in the collections include OCR that is typically much less accurate than newer items. However, there is no information about the OCR quality provided in the dataset.

### Directory Structure

The top-level directory of the ProQuest collection contains four folders, each corresponding to one of the licensed titles:

| Title | Folder |
|---|---|
| *The Scotsman* | `1005678` |
| *The Washington Post* / *Times Herald* | `1006359` |
| *The Guardian* / *The Observer* / *The Manchester Guardian* | `1006051` |
| *The New York Times* | `1005685` |

Each top-level folder contains two subfolders: `PDF` and `XML`. These branch into additional nested subfolders, each titled as a number that corresponds to a subsection of the ProQuest titles. For instance, subfolder `55412` within `1006051` corresponds to issues of *The Observer* published from 4 December 1791 to 30 December 1900.

Scanned copies of individual articles are stored as PDF images; the metadata for each PDF is stored as a corresponding XML file and includes structured elements including OCR transcription, article title, publication date, page number, section and additional bibliographic information. For the purposes of this project, the OCR transcriptions were extracted along with the article title and type and date of publication.

### Data Quality
For those who are familiar with OCR-scanned historical archives, it is probably unsurprising that the quality of the collections varies from very poor to within a roughly acceptable error threshold (CER < 10%). A major challenge in the dataset is the quality of the images themselves. This is especially the case for the older articles in the collection, which tend to be poorer quality images that resulted in less accurate OCR. 

For example, some of the images are noisy and blurry to the extent that I myself found it difficult to read them:

![example of noisy image](data/readme_imgs/noise-example.png)

Other articles have complex layout features, such as tables, that caused the OCR engine to generate lots of errors:

![example of complex layout 1](data/readme_imgs/layout-example.png) 

![example of complex layout 2](data/readme_imgslayout-example-2.png)

Other challenging elements observed in the dataset include variations in typeface, faded column or table boundaries, footnotes, 
---

## About the Data in This Repository

Due to the licensing restrictions described above, the data included in this repository is by no means comprehensive or fully representative and is provided for demonstration purposes. I have included a small subset of 100 images publicly available to ProQuest subscribers along with publicly available metadata and the OCR transcriptions provided by ProQuest. 

This repository contains two data components:

### 1. Ground Truth Transcriptions (`ground_truth`)

The file `ground_truth` is a CSV file containing manually transcribed articles from early nineteenth-century issues of *The Scotsman* paired with their corresponding OCR text and metadata from ProQuest. Each row in the CSV represents a single article and contains the following fields:

| Column | Description |
|---|---|
| `proquest_url` | A direct link to the article in the ProQuest Historical Newspapers database (requires institutional access) |
| `original_ocr` | The raw OCR text as produced by ProQuest during the initial digitisation of the archive (method unspecified) |
| `year` | The year the article was published in *The Scotsman* |
| `manual_transcription` | A manual transcription of the article used as ground truth for evaluation |
| `doc_type` | The content category assigned to the article by ProQuest (e.g. `article`, `masthead`, `classified_ad`) |
| `png_img` | The filename of the corresponding `.png` image in `data/imgs/`, linking each row to its source scan |

The manual transcriptions were produced for use both as training data (in the fine-tuning stage of the pipeline) and as ground truth for evaluation (calculating character error rate (CER) and word error rate (WER) of OCR accuracy.

### 2. Article Images (`imgs/`)

The folder `imgs/` contains 100 `.png` images of individual articles from early nineteenth-century issues of *The Scotsman*. These are the preprocessed images used as input to the THISTLE pipeline. Each image corresponds to a single article scan and is named with a numeric identifier that matches the `id` field in the ground truth CSV.

The images were extracted from the original PDF scans in the ProQuest collection, converted to `.png` format and processed through the image preprocessing stage of the pipeline (see `code/img_preprocessing.ipynb`). 
