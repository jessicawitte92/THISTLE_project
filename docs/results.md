# Results

### ProQuest dataset
The evaluation dataset consisted of historical newspaper of *The Scotsman* from the early nineteenth century (1817-1825). In the ProQuest database, the articles in the collection are categorised by genre; the distribution of categories in the dataset is:

article: 33 
 
classified_ad: 29
 
other: 14
 
display_ad: 10
 
front_page: 7
 
masthead: 2
 
review: 2
 
letter_to_editor: 2
 
editorial_article: 2
 

The dataset is a .csv file containing metadata for each article, including the OCR output included in the original XML metadata file, along with a manual transcription of the full text.  

#### Baseline OCR accuracy
I calculated baseline OCR accuracy for each article using Character Error Rate (CER) and Word Error Rate (WER), two common metrics in OCR and automatic speech recognition research. Both metrics are based on the Levenshtein edit distance between the model output and the ground-truth transcription. Values range from 0 (perfect match) upward; scores exceeding 1.0 indicate that the number of insertions exceeds the total number of reference units. The average WER for the dataset is 65.61% with a per-sample minimum of 12.33% and maximum of 205.26%; the average CER is 38.44% with a per-sample minimum of 4.65% and maximum of 134.63%.

### Tesseract and Transkribus
### DeepSeek OCR
### Zero-shot prompting test case
At this stage, I compared three 'base' models of open-access LLMs in Ollama: Mistral (Mistral AI), Llama (Meta) and Gemma (Google). The aim was to evaluate whether LLMs could improve noisy OCR without task-specific fine-tuning, a zero-shot prompting approach was implemented; additionally, the LLMs' behaviour informed the approach for fine-tuning in the next stage of the pipeline. Prompts were structured to instruct the model to correct OCR errors without modernising or altering the historical language, but no additional information or examples were included.

In earlier iterations, a system message defined the model persona ("THISTLE") and constrained the model to preserve historical language and formatting. Variations of this instruction were tested, though the core methodology remained zero-shot correction without exposure to ground truth examples.

#### Quantitative Results

Overall descriptive statistics for LLM output compared to ground truth are shown in the summary table and plot below:
 
| Model   | Metric | Mean   | Median | Std    | Min    | Max     |
| ------- | ------ | ------ | ------ | ------ | ------ | ------- |
| llama   | WER    | 0.6243 | 0.6801 | 0.2017 | 0.1672 | 0.9945  |
| llama   | CER    | 0.3879 | 0.3602 | 0.2010 | 0.0540 | 0.8199  |
| gemma   | WER    | 0.6949 | 0.6734 | 0.2846 | 0.0000 | 0.8199  |
| gemma   | CER    | 0.4587 | 0.4575 | 0.2134 | 0.0000 | 1.3500  |
| mistral | WER    | 0.9389 | 0.6707 | 2.6364 | 0.1470 | 26.7500 |
| mistral | CER    | 0.7010 | 0.4427 | 2.5654 | 0.0538 | 25.8571 |
 

![plot of results 2](/imgs/ocr_plot_2.png)


Across models, zero-shot correction reduced some character-level errors, but overall error rates still exceeded the 5–10% range often considered acceptable for reliable downstream computational analysis. CER scores were generally lower than WER scores, which suggests that while models were sometimes able to correct individual tokens, sentence-level correction was more difficult. In several cases, CER and WER values exceed 1.0, indicating that the models *added* text not present in the original input. Llama's lower maximum CER and WER threshold suggests quantitatively that it was the least prone to generating extraneous text, but closer look revealed that these lower error rates could primarily be attributed to task refusal. Llama often noted concerns of copyright breach in its refusal, a finding which informed the construction of the fine tuning prompt to explicitly mention that the articles are in the public domain. 

These results indicate weak performance for zero-shot prompting in correcting highly degraded nineteenth-century OCR. At the time this evaluation was conducted, these models were some of the highest benchmarked in text-to-text generation open-access. However, I did not test closed-source models, and it is possible that may perform better in zero-shot OCR error correction; alternatively, models fine-tuned on more closely aligned historical or OCR-specific tasks would yield stronger zero-shot performance could have better results.

A central methodological insight from this test case was that task refusal and unsolicited text generation could impact the effectiveness of the OCR correction pipeline. In the next stage of development, I focused not only on improving error rates but also on constraining model behaviour.


