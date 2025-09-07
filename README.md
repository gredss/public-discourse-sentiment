# Public Discourse Sentiment Analysis

This repository presents an applied research and engineering effort in building a robust sentiment analysis system for Indonesian text. The project combines model benchmarking, architectural experimentation, scalable labeling strategies, and deployment-oriented design. The end goal is to deliver a task-specific AI agent that can classify sentiment in Indonesian comments with production-level reliability.

---

## Introduction

Public discourse in Indonesia is increasingly shaped by social media platforms, where millions of comments reflect public opinion on politics, economics, and social issues. Unlike formal writing, these comments often contain **slang, emojis, spelling variations, and informal structures**, making sentiment analysis a non-trivial task.

This project situates itself in a **real-world case study**: the controversy surrounding the **Indonesian House of Representatives (DPR) receiving a daily salary bonus of IDR 3 million**. The case generated intense public reaction online, producing a wide range of sentiments—from anger and criticism to sarcasm and occasional support. By analyzing such discussions, the system demonstrates its relevance in handling sentiment within politically charged and socially noisy contexts.

Moreover, presents a dataset of Indonesian YouTube comments collected in the context of the Jakarta protest on 28 August 2025, a major demonstration that drew nationwide attention. The dataset has been curated to capture how Indonesian citizens express sentiment online in politically and socially charged moments.

Therefore, this project focuses on creating and documenting a high-quality dataset that can serve as a foundation for future work in sentiment analysis, computational social science, and digital politics.

---

## Label Definitions

The sentiment classification task is designed around five categories that capture the expressive range of Indonesian social media comments, particularly in politically and socially charged discussions:

* **Anger** – Comments reflecting strong negative emotions.
* **Disgust** – Sentiment expressing moral revulsion or disdain.
* **Sadness** – Expressions of disappointment, grief, or hopelessness, often framed as collective suffering.
* **Sarcasm** – Indirect or humorous sentiment.
* **Neutral** – Comments unrelated to the DPR salary case or devoid of sentiment.

These categories ensure coverage of both **explicit emotional expression** and **implicit communicative forms** (e.g., sarcasm), reflecting the nuances of Indonesian digital discourse.

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
* **Because sentiment is often expressed through slang, emoji, and “raw” stylistics in Indonesian social media, I choose using the raw dataset (`Comment`) as the primary input.**

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

#### Purpose and Use

By making this dataset available, the goal is to support:

* **NLP research** on Indonesian sentiment classification.
* **Social science studies** of political discourse and protest mobilization.
* **Dataset benchmarking** for transformer-based language models.

Researchers may choose their own annotation strategies, preprocessing pipelines, or model architectures depending on their objectives.
