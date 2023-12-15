# Comparison of extreme text summarization models on BCC news dataset

Part of the Text Mining and Search project | UniMiB

For the following analysis, the dataset used is the X-SUM database, a collection of online articles from the British BBC. Retrieval of the data was done by downloading the different articles limited to sport category, creating a final data frame containing about 50k purely sports articles about 60 different sports.

The text technique applied on the dataset is text summarization; there are two main families of methods to solve a text summarization problem: extraction-based summarization and abstraction-based summarization.

- Extraction-based summarization is a family of techniques that, starting from the dictionary of words of a specific document, try to determine which are the most relevant words and combine them in order to create a summary.

- Abstraction-based summarization, on the other hand, is based on more advanced deep learning techniques. This family tries to copy a typical human behavior, allowing the model to add and generate words that are considered relevant but are not in the vocabulary of a given document.

For the given analysis, the following models have been considered:

- Baseline

The baseline pipeline is used only as a benchmark. It consists of the selection of the first three sentences of each document. The idea that the most relevant information of a document is inside the first part is the document was one of the most popular in the early stages of text summarization technique, while it is used mainly only for comparison purposes.

- T5

T5 (Text-To-Text Transfer Transformer) is a transformer model that is trained in an end-to-end manner with text as input and modified text as output, in contrast to BERT-style models that can only output either a class label or a span of the input. This text-to-text formatting makes the T5 model fit for multiple NLP tasks like Question-Answering, Machine Translation and also Summarization tasks like the current one.

- BART

BART is a denoising autoencoder for pretraining sequence-to-sequence models. It is trained by corrupting text with an arbitrary noising function, and learning a model to reconstruct the original text. It uses a standard Transformer-based neural machine translation architecture.

- PEGASUS

PEGASUS (Pre-training with Extracted Gap-sentences for Abstractive SUmmarization Sequenceto-sequence) was specifically designed for abstractive summarization and is pre-trained with a self-supervised gap-sentence-generation objective. In this task, entire sentences are masked from the source document, concatenated, and used as the target “summary”.

Evaluation of the results

|              | ROUGE-1    | ROUGE-2    | ROUGE-L | ROUGE-L SUM |
| ------------ | --------- | --------- | --------- | ----------- |
| Baseline     | 0.168     | 0.020     | 0.107     | 0.107       |
| T5           | 0.171     | 0.023     | 0.117     | 0.166       |
| BART         | 0.203     | 0.041     | 0.135     | 0.166       |
| PEGASUS      | 0.472     | 0.269     | 0.412     | 0.414       |


At the state of the art, one of the most used alternatives of a human evaluation of text summarization results, clearly not a scalable method, is the Rouge statistic, that is the one used for the current work. ROUGE stands for Recall-Oriented Understudy for Gisting Evaluation. It includes measures to automatically determine the quality of a summary by comparing it to ideal summaries created by humans. The proposed models obtained the following results:


To have a better understanding of the differences between summarization techniques, the following table report the results obtained on a specific document from the test set.


From both a human evaluation and an analytical one, it is clear the below summary obtained from the Pegasus model is the best one, still keeping a considerable short length of the summary.

Fine tuning of the best model



In order to improve the results obtained on the best model, we have implemented a fine tuning strategy on the Pegasus model. Fine tuning allows us to adjust the weights estimated on a huge dataset in order to become more similar to a specific task. In this case, the goal of the strategy is to adapt the Pegasus model to sport documents, but still starting from the original weights. The training phase uses 8 elements for each training batch and 4 for each validation one. This step required 8 epochs and around 9 hours of training. After this step, the improvement on the validation set was very limited so we decided to not continue more.

|              | ROUGE-1    | ROUGE-2    | ROUGE-L | ROUGE-L SUM |
| ------------ | --------- | --------- | --------- | ----------- |
| PEGASUS      | 0.472     | 0.269     | 0.412     | 0.414       |
| PEGASUS FINE-TUNED     | 0.497     | 0.275     | 0.418     | 0.418       |

The table above shows that the fine tuning model proposed improves the performance compared to the original one. It means that the new version is more capable to summarize sport documents, that is the main goal of the analysis. In particular, the best improvement is observed on the ROUGE-1 metric, so we can interpret the results as an improvement in recognizing relevant unigrams in a starting document.
