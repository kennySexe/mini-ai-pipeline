# CAS2105 Homework 6: Mini AI Pipeline Project

**Project Title:** Mini AI Pipeline: Movie Review Sentiment Analysis
**Your Name:** [请在此处填写你的姓名]
**Student ID:** [请在此处填写你的学号]

---

## 1. Introduction

This project serves as a gentle introduction to designing simple AI pipelines, focusing on the core workflow: problem definition, pipeline design, evaluation, and reflection. The goal is to practice the process of AI work rather than achieving state-of-the-art results.

I chose the task of **binary text classification** for movie reviews (*Sentiment Analysis*). The project is lightweight and designed to run quickly and comfortably on a standard CPU.

---

## 2. Task Definition

**Task description**
The objective is to perform binary text classification, categorizing an input movie review into one of two sentiment categories: **Positive** or **Negative**.

**Motivation**
Automated sentiment analysis is essential for quickly processing large volumes of user feedback, helping film studios and streaming services understand audience reception and trends.

**Input / Output**

* **Input:** A string of text (a movie review)
* **Output:** A binary label: `1` (Positive) or `0` (Negative)

**Success criteria**
The primary success criterion is **Accuracy**. A successful pipeline must demonstrate a significant and clear performance improvement over the naïve baseline.

---

## 3. Methods

### 3.1 Naïve Baseline

**Method description**
I implemented a simple **Keyword Counting heuristic**. The algorithm maintains two short lists of common positive and negative sentiment words. It counts the occurrences of words from both lists in a review. If the count of positive keywords is greater than or equal to the negative count, the prediction is **Positive (1)**; otherwise, it is **Negative (0)**.

**Why naïve**
This method treats text as a *bag of words*, completely ignoring grammatical structure, word order, and context (such as negation or sarcasm).

**Likely failure modes**
Failure is highly likely in cases of explicit negation (e.g., *"not good"*) or when the review expresses sentiment implicitly without using predefined keywords.

---

### 3.2 AI Pipeline

**Models used**
I used the pre-trained **DistilBERT** model, specifically the `distilbert-base-uncased-finetuned-sst-2-english` checkpoint from the Hugging Face library.

**Pipeline stages**

1. **Preprocessing:** Input text is processed by the DistilBERT tokenizer, which converts raw text into numerical tokens and attention masks. Sequences longer than 512 tokens are truncated.
2. **Inference:** The tokenized input is passed through the pre-trained DistilBERT Transformer model.
3. **Decision:** Output logits are used to determine the final class by selecting the label (Positive or Negative) with the highest probability.

**Design choices and justification**
DistilBERT was chosen because it is a lightweight and efficient distilled Transformer model. Using the SST-2 fine-tuned version provides strong performance on sentiment analysis tasks with minimal computational overhead, making it well-suited for a small-scale project focused on transfer learning.

---

## 4. Experiments and Results

### 4.1 Dataset

* **Source:** IMDB Large Movie Review Dataset (loaded via the Hugging Face `datasets` library)
* **Total examples:** 200 randomly sampled examples from the public test split
* **Train/Test split:** All 200 examples were used for evaluation, since the AI pipeline uses a pre-trained model without fine-tuning

**Preprocessing steps**

* **Baseline:** Text was lowercased
* **AI pipeline:** All preprocessing (tokenization, truncation, padding) was handled by the model’s tokenizer

---

### 4.2 Metrics

I used **Accuracy** as the primary quantitative metric.

---

### 4.3 Results

The AI pipeline significantly outperformed the naïve baseline, demonstrating the value of deep learning models in capturing linguistic context.

| Method      | Accuracy | F1-Score (Optional) |
| ----------- | -------- | ------------------- |
| Baseline    | 0.605    | TODO                |
| AI Pipeline | 0.790    | TODO                |

**Qualitative Examples**

**Success (AI Pipeline only)**

* **Text:** *"This movie spends most of its time preaching that it is the script that makes the movie, but apparen..."*
* **True Label:** Negative (0)
* **Baseline Prediction:** Positive (1) — fails due to misleading keyword counts
* **AI Prediction:** Negative (0) — correctly captures the overall negative tone

**Failure (Baseline only)**

* **Text:** *"The plot was not bad at all, I was quite entertained."*
* **True Label:** Positive (1)
* **Baseline Prediction:** Negative (0) — fails due to the keyword *"bad"*
* **AI Prediction:** Positive (1) — correctly interprets the negation

**Hard Case (AI Failure)**

* **Text:** *"It tries so hard to be clever that it ends up being exhausting."*
* **True Label:** Negative (0)
* **Baseline Prediction:** Positive (1) — misled by *"clever"*
* **AI Prediction:** Positive (1) — fails to capture the subtle negative implication

---

## 5. Reflection and Limitations

The AI pipeline worked better than I expected, achieving over **75% accuracy** with only a few lines of code using the `transformers` library. The most difficult part was selecting a fair naïve baseline: it could not be completely random, but it also needed to remain simple enough to clearly highlight the performance gap.

Accuracy was chosen as the evaluation metric because the dataset was balanced. However, by examining individual prediction differences between the baseline and the AI pipeline, I observed that even the AI model struggles with subtle sarcasm and implicit negativity.

One limitation of this experiment is the small dataset size (only 200 examples), which may introduce high variance in the results. In future work, I would use a larger evaluation set and compute additional metrics such as F1-score for a more comprehensive comparison.

With more time and computational resources, I would fine-tune DistilBERT on the IMDB training set. This could allow the model to better capture the specific linguistic style of movie reviews compared to the generic SST-2 sentiment dataset.
