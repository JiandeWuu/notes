# Soft Gazetteers for Low-Resource Named Entity Recognition

## Abstract

Traditional named entity recognition models use gazetteers (lists of entities) as features to improve performance.

傳統的NER模型使用地名詞典進一步提升性能。

- Traditional（傳統的）
- gazetteers（地名詞典）

Although modern neural network models do not require such hand crafted features for strong performance, recent work(**Wu et al., 2018**) has demonstrated their utility for named entity recognition on English data.

雖然現代神經網路模型不需要這樣手工精心製作特徵就可以得到強大的性能，最近工作(**Wu et al., 2018**)能夠證明英文資料在NER的效用。

- crafted（精心製作）
- demonstrated（證明的）

However, designing such features for low-resource languages is challenging, because exhaustive entity gazetteers do not exist in these languages.

儘管如此，為低資源語言設計這樣功能是具有挑戰性的，因為這些語言沒有詳盡的實體地名詞典。

- low-resource（低資源）
- challenging（具有挑戰性的）
- exhaustive（詳盡的）

To address this problem, we propose a method of "soft gazetteers" that incorporates ubiquitously available information from English knowledge bases, such as Wikipedia, into neural named entity recognition models through cross-lingual entity linking.

為了解決這個問題，我們提出了一個方法"軟地名詞典"它結合了英文知識庫包含的無處不在的信息，像維基百科，通過跨語言的實體連結轉化成神經NER模型。

- soft（柔軟的）
- ubiquitously（無處不在）
- through（通過）
- cross-lingual（跨語言）

Our experiments on four low-resource languages show an average improvement of 4 points in F1 score.

我們實驗了4個低資源語言顯示F1分數平均改善了4個百分點。

## 1 Introduction

Before the widespread adoption of neural networks for natural language processing tasks, named entity recognition (NER) systems used linguistic features based on lexical and syntactic knowledge to improve performance (**Ratinov and Roth, 2009**).

在NLP廣泛採用神經網路之前，NER系統使用基於詞彙跟句法的語言特徵知識來提高性能(**Ratinov and Roth, 2009**)。

- widespread（廣泛）
- lexical（詞彙的）
- syntactic（句法）

With the introduction of the neural LSTM-CRF model (**Huang et al., 2015; Lample et al., 2016**), the need to develop hand-crafted features to train strong NER models diminished.

隨著介紹神經LSTM-CRF模型，開發手工製作的功能來訓練強大NER模型的需求減少了。

- diminished（減少了）

However, **Wu et al. (2018)** have recently demonstrated that integrating linguistic features based on part-of-speech tags, word shapes, and manually created lists of entities called $gazetteers$ into neural models leads to better NER on English data.

盡管如此，**Wu et al. (2018)** 最近證明，將基於詞性標籤，字詞形狀，和手動建立稱為$gazetteers$的實體序列的語言特徵集合到英文資料的神經模型中帶來更好的NER。

- part-of-speech（詞性）

Of particular interest to this paper are the gazetteer-based features - binary-valued features determined by whether or not an entity is present in the gazetteer.

本文特別興趣的是

- particular（特定）
- interest（興趣）
- determined（決心）
- present in（存在於）

Although neural NER models have been applied to low-resource settings (**Cotterell and Duh, 2017; Huang et al., 2019**), directly integrating gazetteer features into these models is difficult because gazetteers these languages are either limited in coverage or completely absent.

雖然神經NER模型已經應用在低資源的情況(**Cotterell and Duh, 2017; Huang et al., 2019**)，直接整合地名詞典功能在這些模型上是困難的因為這些語言的地名詞典要麻覆蓋率有限不然就是根本沒有詞庫。

- directly（直）
- coverage（覆蓋範圍）
- completely（完全地）
- absent（缺席）

Expanding them is time-consuming and expensive, due to the lack of available annotators for low-resource languages (**Strassel and Tracey, 2016**).

由於低資源的語言缺乏可用的註譯器，擴展這些是耗時的跟昂貴的(**Strassel and Tracey, 2016**)。

- Expanding（擴展中）
- time-consuming（耗時的）
- expensive（昂貴）
- due to（由於）
- annotators（註譯者）

As an alternative, we introduce "soft gazetteers", a method to create continuous-valued gazetteer features based on readily available data from high-resource languages and large English knowledge bases (e.g., Wikipedia).

作為備選，我們介紹"軟地名詞典"，一種基於高資源語言和大型英語知識庫的隨時可用數據創建連續值地名詞典特徵的方法。

- alternative（另類）
- continuous-valued（連續值）
- readily available（一應俱全）

More specifically, we use entity linking methods to extract information from these resources and integrate it into the commonly-used CNN-LSTM-CRF NER model (**Ma and Hovy, 2016**) using a carefully designed feature set.

更具體來說，我們使用實體鏈接方法從這些資源提取信息與用一個精心設計的功能整合他到常用的CNN-LSTM-CRF NER模型裡(**Ma and Hovy, 2016**)。

- More specifically（進一步來說）
- commonly-used（常用的）
- carefully（小心）

We use entity linking methods designed for low-resource languages, which require far fewer resources than traditional gazetteer features (**Upadhyay et al., 2018; Zhou et al., 2020**).

我們幫低資源語言設計了一個實體鏈接方法，比傳統的地名詞典特徵需要更少的資源。

- fewer（更少）

Our experiments demonstrate the effectiveness of our proposed soft gazetteer features, with an average improvement of 4 F1 points over the baseline, across four low-resource languages: Kinyarwanda, Oromo, Sinhala, and Tigrinya.

我們實驗證明提出的軟地名詞典功能是有效的，比標準提高了F1 4個百分點，跨越了四個低資源語言：Kinyarwanda, Oromo, Sinhala, and Tigrinya。

## 2 Background

**Named Entity Recognition** NER identifies named entity spans in an input sentence, and classifies them into predefined types (e.g., location, person, organization).

NER識別輸入句子中的命名實體範圍，並將它們分類成預定義的類別(e.g., location, person, organization)。

- identifies（確定）
- predefined（預定義的）

A commonly used method for doing so is the **BIO** tagging scheme, representing the **B**eginning, the **I**nside and the **O**utside of a text segment (**Ratinov and Roth, 2009**).

通常使用的方法是BIO標注方法，一個詞分割成B代表開始，I是中間，O是其他(**Ratinov and Roth, 2009**)。

- scheme（方案）
- representing（代表）
- segment（分割）

The first word of a named entity is tagged with a "B-", subsequent words in the entity are "I-", and non-entity words are "O".

命名實體（詞）的第一個字標注成"B-"，這個詞後續的字標注"I-"，不是命名實體的字標注"O"。

For example:

[Mark]B-PER [Watney]I-PER [visited]O [Mars]B-LOC

**Binary Gazetteer Features** Gazetteers are lists of named entities collected from various sources (e.g., nation-wide census, GeoNames, etc.).

**二進制地名詞典特徵** 地名詞典是一個由來自各種來源的命名實體集所組成的lists（像是人口普查，地名）。

- collected（集）
- various（各種）

They have been used to create features for NER models, typically binary features indicating whether the corresponding $n$-gram is present in the gazetteer.

它已經建立於NER模型，通常是二進制功能來指示地名詞典是否存在相應的$n$-gram。

- typically（通常）
- indicating（指示）
- corresponding（相應）

**Entity Linking** Entity linking(EL) is the task of associating a named entity mention with its corresponding entry in a structured knowledge base(KB) (**Hachey et al., 2013**).

**實體鏈接** 實體鏈接(EL)是在關聯命名實體提及的任務與它在結構化的知識庫(KB)中相應條目(**Hachey et al., 2013**)。

- associating（關聯）
- mention（提到）
- structured（結構化的）

For example, linking the entity mention "Mars" with its Wikipedia entry.

舉一個例子，將實體提及"Mars"與維基百科條目鏈接起來。

In most entity linking systems (**Hachey et al., 2013; Sil et al., 2018**), the first step is shortlisting candidate KB entries, which are further processed by an entity disambiguation algorithm.

多數時候的實體鏈接(**Hachey et al., 2013; Sil et al., 2018**)，第一步從知識庫中候選的短詞選擇最終要進入下一步的詞，由實體消岐算法做進一步處理。

- candidate（候選人）
- entries（參賽作品）

Candidate retrieval methods, in general, also score each candidate with respect to the input mention.

候選的檢索方法，一般情況，就是遵從輸入提到的給每個候選詞打分數。

- retrieval methods（檢索方法）

## 3 Soft Gazetteer Features

As briefly alluded to in the introduction, creating binary gazetteer features is challenging for low-resource languages.

如同介紹簡要的提到的那樣，幫低資源語言建立二進制地名詞典特徵是具有挑戰性的。

- briefly（簡要）
- alluded（提起）

The soft gazetteer features we propose instead take advantage of existing limited gazetteers and English knowledge bases using low-resource EL methods.

我們建議使用軟地名詞典功能，而不是利用現有的有限地名詞典和使用低資源EL方法的英語知識庫。

- instead（替代）
- advantage（優點）

In contrast to typical binary gazetteer features, the soft gazetteer feature values are continuous, lying between 0 and 1.

與典型的二進制地名詞典相比，軟地名詞典的值是連續的，介於0到1之間。

- contrast（對比）
- typical（典型）
- lying between（介於）

Given an input sentence, we calculate the soft gazetteer features for each span of $n$ words, $s=w_i,\ldots,w_{i+n-1}$, and the apply the features to each word in the span.

給定一個輸入句子，我們計算範圍內每個字$n$ words, $s=w_i,\ldots,w_{i+n-1}$的軟地名詞典特徵，並幫範圍內的每個字加上特徵。

- calculate（計算）

We assume that we have an EL candidate retrieval method that returns candidate KB entries $C=(c_1,c_2\ldots)$ for the input span.
