\documentclass[11pt, a4paper]{article}  % Set default font and page size


% Load packages and configure them via options
\usepackage[utf8]{inputenc}
\usepackage[left=22mm, right=22mm, top=20mm, bottom=20mm, headheight=40pt, headsep=10pt]{geometry}  % Set margin of page
\usepackage[parfill]{parskip}  % Set spacing between paragraphs
\usepackage{amsmath}  % For the equation environment
\usepackage{graphicx}  % Required for inserting images
\usepackage[dvipsnames]{xcolor}  % Access color codes
\usepackage[colorlinks=true]{hyperref}  % Enable hyperlink
\usepackage{cleveref}  % Enable easy references to sections, figures, and tables
\usepackage{minted}  % Enable code formatting in LaTeX
\usepackage{changepage}  % Required for \adjustwidth
\usepackage{caption}
\usepackage{enumitem}  % Requried for changing configurations for list
\usepackage{lmodern}  % For \texttt font style
\usepackage{booktabs}  % For table formatting
\usepackage{multirow}  % Required for \multirow in tables
\usepackage{emoji}  % Required for Hugging Face emoji
\usepackage[numbers, sort&compress]{natbib}  % For citations and references
\bibliographystyle{unsrtnat}
\usepackage[utf8]{inputenc}

% Define macros, including color codes and macros
\definecolor{YonseiBlue}{HTML}{183772}

% TODO: Replace "CAS2105 Homework 6: Mini AI Pipeline Project \emoji{hugging-face}" with your report title
\title{CAS2105 Homework 6: Mini AI Pipeline Project \emoji{hugging-face}}  % Title


% TODO: Replace "Your Name (Your Student ID)" with your name and student ID
\author{PIAO YONGLIN (2021147515)}


% Redefine title style
\makeatletter
  \def\@maketitle{%
  \newpage
  \null
  \vskip 2em%
  \begin{flushleft}%
  {\color{YonseiBlue}\rule{\textwidth}{2pt}}
  \vskip 5pt%
  \let \footnote \thanks
    {\Large \bf \@title \par}%
    \vskip 1.5em%
    {\normalsize \bf \lineskip .5em% 
        \@author
        \vskip 2pt%
        \par}
    {\color{YonseiBlue}\rule{\textwidth}{2pt}}
    \vskip 1.5em
  \end{flushleft}%
  }
\makeatother



\begin{document}
\maketitle


% TODO: Replace the following sections with your own report!


\section{Introduction}
\label{sec:introduction}

% --- TODO: Students fill this in ---
% Describe your chosen task and why it is interesting.
% Keep the explanation clear enough for a classmate to understand.

This project provides a gentle introduction to designing simple \textbf{AI pipelines}. Rather than training large models or reading extensive research literature, you will:

\begin{itemize}
    \item Choose a small, concrete problem solvable with an AI pipeline (e.g., text classification, retrieval, simple QA, image classification).
    \item Choose or collect a small dataset (e.g., from \texttt{datasets}).
    \item Implement a simple \emph{na\"ive baseline} (e.g., rule-based or heuristic).
    \item Build an improved pipeline using existing pre-trained models.
    \item Evaluate both approaches using appropriate metrics.
    \item Reflect on what worked, what failed, and what you learned.
\end{itemize}

The emphasis is on the \emph{process} of AI work: problem definition, pipeline design, evaluation, iteration, and writing. Your problem should be small enough to run comfortably on a single GPU (e.g., RTX~3090) or CPU.

\paragraph{Your tasks.} 
Please replace all the sections to complete your report, starting from adding your project title, your name, and student ID above! Feel free to reorganize the sections. Then, submit a \textbf{public} github repository link that includes your code (including Jupyter notebook with results) and report \textbf{in PDF}, via LearnUs. 

You can use tables and figures, and cite references using \verb|\citep| (\citep{wei2022finetuned}) or \verb|\citet| (\citet{wei2022finetuned}).


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Task Definition}
\label{sec:definition}
\begin{itemize}
    \item \textbf{Task description:} The objective is to perform binary text classification on movie reviews. Specifically, the system classifies an input text into one of two sentiment categories: Positive or Negative.
    \item \textbf{Motivation:} Automated sentiment analysis is highly useful for businesses to process large volumes of user feedback instantly, understanding audience reception at scale.
    \item \textbf{Input / Output:} Input: A string of text containing a user's movie review.
Output: A binary label: 1 (Positive) or 0 (Negative).
    \item \textbf{Success criteria:} The primary success criterion is Accuracy (percentage of correct predictions) on the test dataset. A successful system is one that significantly outperforms both a random guess (50\%) and the simple naïve baseline.
\end{itemize}

\section{Methods}
\label{sec:methods}

This section includes both the na\"ive baseline and the improved AI pipeline.

\subsection{Na\"ive Baseline}
\label{subsec:baseline}

Implement a simple method that does not rely on heavy models. Examples include:
\begin{itemize}
    \item Keyword-based text classification,
    \item Simple color/shape heuristics for image tasks,
    \item String-overlap–based retrieval.
\end{itemize}

In your report, explain:
\begin{itemize}
    \item How the baseline works,
    \item Why it is considered na\"ive,
    \item Expected failure cases.
\end{itemize}

\subsubsection*{Your Baseline (Keyword Counting)}
\begin{itemize}
    \item \textbf{Method description:} I implemented a keyword counting heuristic algorithm. Two word lists were defined: a positive list (e.g., good, great, love, best) and a negative list (e.g., bad, terrible, boring, worst). I counted the frequency of these words in the original data. If the count of positive words count(positive-words) greater or equal the count of negative words count(negative-words), the prediction is positive; otherwise, it is negative.
    \item \textbf{Why na\"ive:} This approach is considered naïve because it treats the text as a "bag of words" without understanding syntax or context. It completely ignores word order and grammar.
    \item \textbf{Likely failure modes:} This program may produce incorrect results due to ambiguous expressions. (e.g., "not bad" is counted as negative due to the word "bad"). Failure may also occur if keywords cannot be identified.
\end{itemize}


\subsection{AI Pipeline}
\label{subsec:pipeline}

Design a small pipeline using one or more pre-trained models. Examples include:

\begin{itemize}
    \item \textbf{Text:} embedding encoder + classifier, or a small transformer model,
    \item \textbf{Retrieval:} embedding model + nearest-neighbor search,
    \item \textbf{Vision:} pre-trained classifier (e.g., ViT-tiny).
\end{itemize}

A typical pipeline contains:
\begin{enumerate}
    \item Preprocessing,
    \item Embedding or representation,
    \item Decision/ranking component,
    \item Optional post-processing.
\end{enumerate}

Fine-tuning large models is not required; inference-only usage is sufficient.

\subsubsection*{Your Pipeline (DistilBERT)}
\begin{itemize}
    \item \textbf{Models used:} I used the DistilBERT model, specifically the distilbert-base-uncased-finetuned-sst-2-english checkpoint from Hugging Face.
    \item \textbf{Pipeline stages:} 1.Preprocessing: The input text is tokenized using the DistilBERT tokenizer (512 tokens). 2.Inference: The input is passed through the pre-trained Transformer model to obtain logits. 3.Decision: A softmax function is applied, and the label with the highest probability score is selected as the prediction.
    \item \textbf{Design choices and justification:} I chose DistilBERT to my model, it is lightweight and fast, making it suitable for a mini-project on limited compute. I selected the sst-2 finetuned version because SST-2 is a sentiment analysis dataset similar to IMDB, allowing for high performance without training.
\end{itemize}


%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Experiments}
\label{sec:experiments}


\subsection{Datasets}
\label{sec:experiments:datasets}

% --- TODO: Students fill this in ---
% Describe your dataset clearly.

You may use a small public dataset (e.g., from \texttt{datasets}) or construct your own. 
In this section, describe:

\begin{itemize}
    \item \textbf{Dataset source}: where it comes from.
    \item \textbf{Size}: number of examples used.
    \item \textbf{Splits}: how you divided train/validation/test.
    \item \textbf{Preprocessing}: e.g., tokenization, resizing, truncation, normalization.
\end{itemize}

\subsection*{Your Dataset Description (IMDB Dataset)}
\begin{itemize}
    \item \textbf{Source:} I used the IMDB dataset which is movie review dataset. Conclude Negative commend and positive commend, witch loaded via the Hugging Face datasets library. 
    \item \textbf{Total examples:} I used a random subset of 200 examples to keep the pipeline lightweight.
    \item \textbf{Train/Test split:} Since I used a pre-trained model , I treated all 200 examples from the official test split as my evaluation set to compare the baseline and the AI pipeline.
    \item \textbf{Preprocessing steps:} 1.Baseline: Text was converted to lowercase to match the keyword lists. 2.AI Pipeline: Text was tokenized, truncated to a maximum length of 512 tokens, and padded to match the model's input requirements.
\end{itemize}


\subsection{Metrics}
\label{sec:experiments:metrics}

Use at least one quantitative metric appropriate for your task:
\begin{itemize}
    \item \textbf{Classification:} accuracy, precision, recall, F1,
    \item \textbf{Retrieval:} precision@k, recall@k,
    \item \textbf{Simple generation:} exact match, ROUGE-1.
\end{itemize}

\textcolor{gray}{It’s worth considering how the metrics you select align with your tasks.}

\subsection{Results}
\label{sec:experiments:results}

Report:
\begin{itemize}
    \item Metric values for baseline vs.\ pipeline,
    \item A results table,
    \item At least three qualitative examples.
\end{itemize}


\begin{center}
\begin{tabular}{lcc}
\toprule
\textbf{Method} & \textbf{Metric 1}\\
\toprule
\midrule
Baseline & 0.6050\\
AI Pipeline & 0.7900\\
\bottomrule
\end{tabular}
\end{center}

\begin{itemize}
    \item Metric values: The AI pipeline significantly outperformed the baseline. The keyword baseline struggled with mixed sentiments, while the AI model effectively captured the overall tone.
    \item Qualitative Examples:

Success (Both): "I absolutely loved this movie, it was fantastic." (Data 176)

Both methods predicted Positive. The baseline found words like "loved" and "fantastic."

Failure (Baseline only): "The plot was not bad at all."

Baseline: Negative (detected "bad").

AI Pipeline: Positive (understood "not bad" context).

Failure (Both/Hard case): "It tries so hard to be clever that it ends up being exhausting." (Data 57)

Baseline: Positive (found "clever").

AI Pipeline: Positive (Incorrect, missed the nuance of "exhausting").

True Label: Negative.
\end{itemize}


If you want to visualize results with any other than tables, refer to below links
\begin{itemize}
    \item \href{https://matplotlib.org/stable/tutorials/pyplot.html}{Matplotlib official tutorial: Introduction to \texttt{pyplot}}
    \item \href{https://matplotlib.org/stable/gallery/index.html}{Matplotlib example gallery (many bar/line/scatter plots with source code)}
    \item \href{https://www.kaggle.com/code/prashant111/matplotlib-tutorial-for-beginners}{Kaggle notebook: Matplotlib tutorial for beginners (interactive code)}
\end{itemize}
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
\section{Reflection and Limitations}
\label{sec:reflection}

Write approximately 6--10 sentences reflecting on:
\begin{itemize}
    \item What worked better than expected,
    \item What failed or was difficult,
    \item How well your metric captured ``quality'',
    \item What you would try next with more time or compute.
\end{itemize}

\subsection*{Your Reflection (Success, failure, areas needing improvement)}
% Students write freely here.
The AI pipeline worked better than I expected, I was able to get over 75\% accuracy with just a few lines of code using the transformers library. The hardest part was choosing a fair starting point, i figure out that it couldn't be completely random, but it also had to stay simple enough to clearly show the performance difference. So i choose accuracy to measure this, since the dataset was balanced, but when i check the dataset, different about baseline and AI, as the person, I noticed that even the AI model struggles with subtle sarcasm. I think one of the problem was the small number of examples (I only choose 200 dataset), which could lead to high variation. So i think i can try a bit dataset next time, and get metric values for baseline and AI pipeline, If I had more time and computing resources, I would try making small changes to DistilBERT using the IMDB training set. This would help me see whether it could learn the specific style of movie reviews better than the generic SST-2 model.


\bibliography{references}

\end{document}
