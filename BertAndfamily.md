# Bert and it's family

text without annotation >> Pre-train >> 針對問題調整 model

- ELMo(Embeddings from Language Models)
- BERT(Bidirectional Encoder Representations from Transformers)
- ERNIE(Enhanced Representation through Knowledge Integration)
- Grover(Generating )

## Outline

- What is pre-train model
- How to fine-tune
- How to pre-train

## Pre-train Model

以前的作法

    Represent each token by a embedding vector, Word2vec

    英文:

    FastText: 讀字母輸出分散式向量的 model

    中文:

    image >> CNN ??

Contextualized Word Embedding, 經由大量的文本資料輸出字詞的分散式向量

- input: 一個句子
- output: embedding for each token

- LSTM
- Self-attention layers
- Tree-based model(?)

## Smaller Model

## Network Architecture

- Transformer-XL: Segment-Level Recurrence With State Reuse
- Reformer
- Longformer
  
  reduce the complexity of self-attention

以上是目前的進展

---

## How to fine-tune

### NLP tasks

input:

- one sentence
- multiple sentences

outpu:

- one class
- class for each token
- copy from input
- general sequence
  
  seq2seq

pre-trained 不固定，一起 train 表現比較好

## Adaptor

在 pre-trained 加入一層 Apt, traing 只動 Apt 的參數。

## Weighted Features

把每一層的輸出 weighted sum 後給 Task Specific

## Why Pre-train Models
