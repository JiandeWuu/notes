# NLP

主軸在理解（語意）。每個人的定義都不太一樣。

## Category

### 輸入一段文字輸出類別

大部分分以下兩種
輸入一段文字輸出一個類別
輸入一段文字輸出每個 token 的類別

### 輸入一段文字輸出一段文字

seq2seq
+copy mechanism?
encoder, (attention), decoder

## Part-of-speech (POS) tagging

- Annotate each word in a sentence with a part-of-speech (e.g. Verb, Adjective, Noun)

- input: seq
- output: class for each token

## Word Segmentation (斷詞)

- for Chinese

- input: seq
- output: class for each token

## Parsing

Outlier

給一個句子產生一個樹狀結構

The results of parsing can be used in the downstream tasks.

## Coreference Resolution (指代消解)

看上下文給代名詞定義，把指同一個事物的詞句定義

## Summarization

- Extractive summarization
  摘要，從文章找出幾個句子當成摘要。

  - input: seq
  - output: class for each token

- Abstractive sumarization
  
  - input: seq (long)
  - output: seq (short)
  
  Pointer network: encouraging direct copy from input

## Machine Translation (翻譯)

Unsupervised machine translation is a critical research direction.

- input: seq
- output: seq

## Grammar Error Correction (改錯字)

- input: seq
- output: seq
(copy is encouraged)

## Sentiment Classification

ex: 判斷正負評

- input: seq
- output: class
  
## Stance Detection

立場偵測

Many systems use the Support, Denying, Querying, and Commenting(SDQC) labels for classifying replies.
Used in Veracity Prediction

## Veracity Prediction

事實偵測(判斷假新聞)

input: Post, Replies, Wikipedia
output: True/False

- input: several seq
- output: class

## Natural Language Inference(NLI)

邏輯推導，輸入一段敘述與一段推論判斷推論的成不成立

premise: A person on a horse jumps over a broken down airplane.

hypothesis: A person is at a diner.

output: contradiction, entailment, neutral

## Search Engine

- input: two seq
- output: a class

## Question Answering(QA)

- input: two seq(question, Knowledge source(search engine))
- output: seq(answer)

Reading comprehension

- Extractive QA: Answer in the document
  大部分是從文章中找答案

- input: several seq
- output: seq
(copy from input)

## chatting

cleverbot

不只是 seq2seq, 需要紀錄。

控制 chat box 的 Personality, Empathy, Knowledge.

## Task-oriented

任務導向的對話

seq2seq

### Natural Language Generation(NLG)

過去的對話, Ask >> NLG >> 回覆

### Policy & State Tracker

過去的對話 >> State Tracker >> State >> Policy >> Ask() >> NLG >> 回覆

State: What has happened in this dialogue.
保留重要的資訊（留下重點）存到 State 中，省略不重要的

Policy: 分類的問題

## Natural Language Understanding(NLU)

- initent Classification
- Slot Filling
  
- input: several seq, output: a seq

## Knowleadge Graph

entity: 東西（實體）

Step 1: Extract Entity
Step 2: Extract Relation
(warning: the following description oversimplifies the task)

### Name Entity Recognition(NER)

你關心的 Entity 就是你的 Name Entity, Name entity recognition: people, organizations, placess are usually name entity

- input: seq
- output: class for each token
(just like POS tagging, slot filling)

### Relation Extraction

relation: 關係

Classifiation Problem

---
NLP 的比賽

## General Language Understanding Evaluation(GLUE)

用一種 model 判斷以下9個任務，來分辨 model 理解語言的程度。

- Corpus of Linguistic Acceptability(CoLA)
- Stanford Sentiment Treebank(SST-2)
  
  input: 看一個句子
  output: 判斷類別

- Microsoft Research Paraphrase Corpus(MRPC)
- Quora Question Pairs(QQP)
- Semantic Textual Similarity Benchmark(STS-B)
  
  input: 兩個句子
  output: 像不像

- Multi-Genre Natural Language Inference(MNLI)
- Question-answering NLI(QNLI)
- Recognizing Textual Entailment(RTE)
- Winograd NLI(WNLI)
  
  NLI
  input: 前提, 假設
  output: 判斷

## Super GLUE

## DecaNLP
