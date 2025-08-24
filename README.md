# Public Discourse Sentiment Analysis Pipeline

This repository presents an applied research and engineering effort in building a robust sentiment analysis system for Indonesian text. The project combines model benchmarking, architectural experimentation, scalable labeling strategies, and deployment-oriented design. The end goal is to deliver a task-specific AI agent that can classify sentiment in Indonesian comments with production-level reliability.

---

## Introduction

Public discourse in Indonesia is increasingly shaped by social media platforms, where millions of comments reflect public opinion on politics, economics, and social issues. Unlike formal writing, these comments often contain **slang, emojis, spelling variations, and informal structures**, making sentiment analysis a non-trivial task.

This project situates itself in a **real-world case study**: the controversy surrounding the **Indonesian House of Representatives (DPR) receiving a daily salary bonus of IDR 3 million**. The case generated intense public reaction online, producing a wide range of sentiments—from anger and criticism to sarcasm and occasional support. By analyzing such discussions, the system demonstrates its relevance in handling sentiment within politically charged and socially noisy contexts.

---

## Initial Step: Data Extraction and Cleaning

As a starting point, I collected user-generated comments by extracting **five YouTube videos** related to the DPR bonus salary issue using the YouTube API. This raw dataset mirrors the **authentic language of Indonesian netizens**, including non-standard expressions and heavy use of slang.

A **basic cleaning pipeline** was then applied to prepare the data for downstream experiments. This included:

* Removing duplicate comments.
* Standardizing whitespace and symbols.
* Lowercasing for consistency.
* Retaining emojis and certain slang terms, since they may carry critical sentiment signals.

This initial dataset serves as the foundation for evaluating transformer-based sentiment models, comparing their robustness on **raw versus cleaned text**, and guiding subsequent experiments in hybrid architectures and semi-automated labeling.

---

Here’s a revised version of your README section with the reasoning you want to add, plus the illustrative table:

---

## 1. Model Evaluation and Benchmarking

I compare transformer-based sentiment models on both raw and cleaned text data:

* **IndoBERT**: `mdhugol/indonesia-bert-sentiment-classification`
* **IndoRoBERTa**: `indobenchmark/indoroberta-base` (sentiment fine-tuned)

---

#### Key Results

**IndoBERT**

* Distribution nearly identical across raw vs clean inputs.
* Agreement between raw and clean labels: \~99%.
* Strong robustness against noise and inconsistent preprocessing.
* **Raw vs Clean for BERT differ by <0.2%. This shows that cleaning did not add value, and the model can already interpret messy text well.**
* **Because sentiment is often expressed through slang, emoji, and “raw” stylistics in Indonesian social media, I recommend using the raw dataset (`Comment`) as the primary input.**

**IndoRoBERTa**

* Clearer separation of Neutral class.
* Agreement rate: \~88%, showing higher sensitivity to preprocessing.
* Requires a stricter normalization pipeline.

Hence, IndoBERT is recommended for production deployments due to stability and robustness, while IndoRoBERTa may be used in controlled environments where preprocessing can be guaranteed.

---

#### Example of Raw vs Clean Differences

| Index | Comment                                           | clean\_comment                                    | label\_raw | label\_clean |
| ----- | ------------------------------------------------- | ------------------------------------------------- | ---------- | ------------ |
| 119   | @@semestaraya562oooh gitu ya                      | @ gitu ya                                         | LABEL\_0   | LABEL\_2     |
| 207   | @@RadioSantuyID Siap gaspool                      | @ siap gaspool                                    | LABEL\_1   | LABEL\_0     |
| 211   | @@RadioSantuyID untuk mereka yg menempatkan di... | @ untuk mereka yg menempatkan diri sebagai opo... | LABEL\_0   | LABEL\_2     |

This table illustrates how minimal cleaning can sometimes alter the contextual cues that influence label assignment, reinforcing the choice of **raw data** for IndoBERT.

---
