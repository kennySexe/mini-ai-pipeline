# CAS2105 Homework 6: Mini AI Pipeline Project

**Project Title:** Mini AI Pipeline: Movie Review Sentiment Analysis


**Your Name:** [PIAO YONGLIN]


**Student ID:** [2021147515]


**https://ko.overleaf.com/read/tjkxcxdjzhkc#f72a3b** (PDF)
---

## 1. Introduction

This project serves as a gentle introduction to designing simple AI pipelines, focusing on the core workflow: problem definition, pipeline design, evaluation, and reflection. 

I chose the task of **binary text classification** for movie reviews. The project is lightweight and designed to run quickly and comfortably on a standard CPU.

---

## 2. Task Definition

**Task description**
The objective is to perform binary text classification on movie reviews. Specifically, the system classifies an input text into one of two sentiment categories: Positive or Negative.

**Motivation**
Automated sentiment analysis is highly useful for businesses to process large volumes of user feedback instantly, understanding audience reception at scale.

**Input / Output**

* **Input:**  A string of text containing a user’s movie review
* **Output:** A binary label: 1 (Positive) or 0 (Negative)

**Success criteria**
The primary success criterion is Accuracy (percentage of correct predictions) on the test dataset. A successful system is one that significantly outperforms both a random guess (50%) and the simple na¨ıve baseline.

---

## 3. Methods

### 3.1 Naïve Baseline

**Method description**
I implemented a keyword counting heuristic algorithm. Two word lists were defined: a positive list (e.g., good, great, love, best) and a negative list (e.g., bad, terrible, boring, worst). I counted the frequency of these words in the original data. If the count of positive words count(positive-words) greater or equal the count of negative words  count(negative-words), the prediction is positive; otherwise, it is negative.

**Why naïve**
This approach is considered na¨ıve because it treats the text as a ”bag of words” without understanding syntax or context. It completely ignores word order and grammar.

**Likely failure modes**
This program may produce incorrect results due to ambiguous ex- pressions. (e.g., ”not bad” is counted as negative due to the word ”bad”). Failure may also occur if keywords cannot be identified.

---

### 3.2 AI Pipeline

**Models used**
I used the DistilBERT model, specifically the distilbert-base-uncased-finetuned-sst-2-english checkpoint from Hugging Face.

**Pipeline stages**

1. **Preprocessing:** The input text is tokenized using the DistilBERT tokenizer (512 tokens).
2. **Inference:** The input is passed through the pre-trained Transformer model to obtain logits.
3. **Decision:** A softmax function is applied, and the label with the highest probability score is selected as the prediction.

**Design choices and justification**
I chose DistilBERT to my model, it is lightweight and fast, making it suitable for a mini-project on limited compute. I selected the sst-2 finetuned version because SST-2 is a sentiment analysis dataset similar to IMDB, allowing for high
performance without training.

---

## 4. Experiments and Results

### 4.1 Dataset

* **Source:** I used the IMDB dataset which is movie review dataset. Conclude Negative commend and positive commend, witch loaded via the Hugging Face datasets library.
* **Total examples:** I used a random subset of 200 examples to keep the pipeline lightweight.
* **Train/Test split:** Since I used a pre-trained model , I treated all 200 examples from the official test split as my evaluation set to compare the baseline and the AI pipeline.

**Preprocessing steps**

* **Baseline:** Text was converted to lowercase to match the keyword lists.
* **AI pipeline:** Text was tokenized, truncated to a maximum length of 512 tokens, and padded to match the model’s input requirements.

---

### 4.2 Metrics

I used **Accuracy** as the primary quantitative metric.

---

### 4.3 Results

The AI pipeline significantly outperformed the baseline. The keyword baseline struggled with mixed sentiments, while the AI model effectively captured the overall tone.

| Method      | Accuracy |
| ----------- | -------- |
| Baseline    | 0.605    |
| AI Pipeline | 0.790    |

**Qualitative Examples**

**Success (Both)**

* **Text:** *"I absolutely loved this movie, it was fantastic..."* (Data 176)
* **True Label:** Positive (1)
* **Baseline Prediction:** Positive (1)
* **AI Prediction:** Positive (1)
* **Both methods predicted Positive. The baseline found words like ”loved” and ”fantastic.”

**Failure (AI only)**

* **Text:** *"The plot was not bad at all..."* (Data 79)
* **True Label:** Positive (1)
* **Baseline Prediction:** Negative (0) — fails due to the keyword "bad"
* **AI Prediction:** Positive (1) — understood ”not bad” context

**Hard Case (AI Failure)**

* **Text:** *"It tries so hard to be clever that it ends up being exhausting..."* (Data 112)
* **True Label:** Negative (0)
* **Baseline Prediction:** Positive (1) — found "clever"
* **AI Prediction:** Positive (1) — missed the nuance of ”exhausting”

---

## 5. Reflection and Limitations

The AI pipeline worked better than I expected, achieving over **75% accuracy** with only a few lines of code using the `transformers` library. The most difficult part was selecting a fair naïve baseline: it could not be completely random, but it also needed to remain simple enough to clearly highlight the performance gap.

Accuracy was chosen as the evaluation metric because the dataset was balanced. However, by examining individual prediction differences between the baseline and the AI pipeline, I observed that even the AI model struggles with subtle sarcasm and implicit negativity.

One limitation of this experiment is the small dataset size (only 200 examples), which may introduce high variance in the results. In future work, I would use a larger evaluation set and compute additional metrics such as F1-score for a more comprehensive comparison.

With more time and computational resources, I would fine-tune DistilBERT on the IMDB training set. This could allow the model to better capture the specific linguistic style of movie reviews compared to the generic SST-2 sentiment dataset.
