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

我們假設我們有一個實體鏈接候選檢索方法，該方法返回輸入範圍的候選知識庫條目$C=(c_1,c_2\ldots)$。

$c_1$ is the highest scoring candidate.

$c_1$是最高分數的候選詞。

As a concrete example, consider a feature that represents the $score$ $of$ $the$ $top-1$ $candidate$.

舉一個具體的例子，考慮一個代表分數第一的候選詞特徵。

- concrete（具體）
- consider（考慮）

**Figure 1** shows an example of calculating this feature on a sentence in Kinyarwanda, one of the languages used in our experiments.

**Figure 1** 計算一個Kinyarwanda句子特徵的例子，我們在單一語言的實驗。

- calculating（計算）

The feature vector $f$ has an element corresponding to each named entity type in the KB (e.g., LOC, PER, and ORG).

特徵向量$f$具有一個與知識庫中每個命名實體類型相應的元素。

For this feature, the element corresponding to the entity type of the highest scoring candidate $c_1$ is updated with the score of the candidate.

對於該特徵，得分最高的候選詞$c_1$的實體類型相對應的元素被更新為候選者的分數。

That is,

$$
f_{type(c_1)}=score(c_1)
$$

This feature vector is applied to each word in the span, considering the position of the specific word in the span according to the BIO scheme; we use the "B-" vector elements for the first word in the span, "I-" otherwise.

該特徵向量將應用於範圍中的每個單詞，根據BIO方案考慮特定字詞的位置，範圍內的第一個字我們使用"B-"向量元素，範圍內其他的字"I-"。

- according（根據）

For a word $w_i$, we combine features from different spans by performing an element-wise addition over vectors of length $n$ that contain $w_i$.

對於單詞$w_i$，我們通過對於包含$w_i$的長度為$n$的向量執行逐元素加法來組合不同跨度的特徵。

- combine（結合）
- performing（表演）
- addition（增加）

The cumulative vector is then normalized by the number of spans of length $n$ that contain $w_i$, so that all values lie between 0 and 1.

然後，通過包含$w_i$的長度$n$的跨度數字累積的向量做標準化，以便所有值都在0到1之間。

- cumulative（累積的）

Finally, we concatenate the normalized vectors for each span length $n$ from 1 to $N$ ($N=3$ in this paper).

最終，我們將每個跨度長度$n$從1到$N$的標準化向量進行連接（本文中$N=3$）。

- concatenate（連接）

We experiment with different ways in which the candidate list can be used to produce feature vectors.

我們實驗了不同的方法使用候選列表生成特徵向量。

- produce（生產）

The complete feature set is:

1. **top-1 score**: This feature takes the score of the highest scoring candidate $c_1$ into account.
    $$
    f_{type(c_1)}=score(c_1)
    $$
    此特徵考慮了分數最高的候選詞$c_1$的分數。

2. **top-3 score**: Like the top-1 feature, we additionally create feature vectors for the second and third highest scoring candidates.
   像top-1功能一樣，我們幫分數第二第三高的候選詞另外建立特徵向量。
   - additionally（另外）

3. **top-3 count**: These features are type-wise counts of the top-3 candidates. Instead of adding the score to the appropriate feature element, we add 1.0 to the current value. For a candidate type $t$, such as LOC, PER or ORG,
    $$
    f_t=\sum_{c\in\{c_1,c_2,c_3\}}{1.0}\times 1_{type(c)=t}
    $$
    $1_{type(c)=t}$ is an indicator function that returns 1.0 when the candidate type is the same as the feature element being updated, 0.0 otherwise.

    這些特徵是前三名候選詞的類型計數。我們沒有將分數添加到適當的要素元素，而是將1.0添加到當前值。對於$t$候選的類型，像是LOC, PER or ORG,

    $1_{type(c)=t}$是一個指標函數，當候選類型與要更新的要素相同時返回1.0，否則返回0.0。

    - appropriate（適當）
    - current（當前）

4. **top-30 count**: This feature computes type-wise counts for the top-30 candidates.

   此特徵計算前30個候選詞的類型數量。

5. **margin**: The margin between the scores of consecutive candidates within the top-4. These features are not computed type-wise. For example the feature value for the margin between the top-2 candidates is,
    $$
    f_{c_1,c_2}=score(c_1)-score(c_2)
    $$

    前4名中的連續候選詞分數之間的差距。這些特徵不是照類別計算。例如，前2名候選詞之間的差距特徵值是，

    - margin（差距）

We experiment with different combinations of these features by concatenating their respective vectors.

我們通過串連它們各自的向量來實驗這些特徵的不同組合。

- concatenating（繫）
- respective（各自）

The concatenated vector is passed through a fully connected neural network layer with a $tanh$ non-linearity and then used in the NER model.

具有非線性$tanh$的全連接神經網路通過傳遞串連的向量，使用在NER模型。

## 4 Named Entity Recognition Model

As our base model, we use the neural CRF model of **Ma and Hovy (2016)**. We adopt the method from **Wu et al. (2018)** to incorporate linguistic features, which uses an autoencoder loss to help retain information from the hand-crafted features throughout the model (shown in **Figure 2**). We briefly discuss the model in this section, but encourage readers to refer to the original papers for a more detailed description.

作為我們的基本模型，我們使用 **Ma and Hovy (2016)** 裡的神經CRF模型。我們採用 **Wu et al. (2018)** 的方法整合語言特徵，它使用autoencoder loss來幫助保留模型中手工製作特徵的信息（如 **Figure 2** ）。我們在本節中簡要討論該模型，但是更多的細節描述鼓勵讀者去參考原始的論文。

- retain（保留）
- throughout（始終）
- discuss（討論）
- encourage（鼓勵）
- refer（參考）
- description（描述）

**NER objective** Given an input sequence, we first calculate a vector representation for each word by concatenating the character representation from a CNN, the word embedding, and the soft gazetteer features. The word representations are then used as input to a bidirectional LSTM (BiLSTM). The hidden states from the BiLSTM and the soft gazetteer features are input to a Conditional Random Field (CRF), which predicts a sequence of NER labels. The training objective, $L_{CRF}$, is the negative log-likelihood of gold label sequence.

給定一個輸入句子，我們首先通過將來自CNN的字符表示，word embedding，軟地名詞庫特徵這些串連起來。然後，將單詞表示形式用作雙向LSTM（BiLSTM）的輸入。再將BiLSTM的hidden狀態跟軟地名詞典特徵輸入到CRF預測一個NER標籤的序列。訓練的標籤序列的目標函式$L_{CRF}$是negative log-likelihood。

**Autoencoder objective** **Wu et al. (2018)** demonstrate that adding an autoencoder to reconstruct the hand-crafted features leads to improvement in NER performance. The autoencoder takes the hidden states of the BiLSTM as input to a fully connected layer with a sigmoid activation function and reconstructs the features. This forces the BiLSTM to retain information from the features. The cross-entropy loss of the soft gazetteer feature reconstruction is the autoencoder objective, $L_{AE}$.

**Wu et al. (2018)**證明加上autoencoder重建手工製作特徵可以提高NER性能。autoencoder需要BiLSTM的hidden狀態輸入到全連接層再經過活化函式sigmoid並重建這些特徵。BiLSTM的作用是保留特徵的信息。autoencoder的目標函式是軟地名詞典重建的cross-entropy loss, $L_{AE}$。

- reconstruct（重建）

**Training and inference** The training objective is the joint loss: $L_{CRF}+L_{AE}$. The losses are given equal weight, as recommended in **Wu et al. (2018)**. During inference, we use Viterbi decoding to obtain the most likely label sequence.

訓練的目標函式是合併loss：$L_{CRF}+L_{AE}$。 **Wu et al. (2018)** 推薦loss給定一樣的權重。推論的過程中，我們使用Viterbi解碼去取得最有可能的標籤序列。

- recommended（推薦的）
- During inference（推論中）
- obtain（獲得）
- likely（可能）

## 5 Experiments

In this section, we discuss our experiments on four low-resource languages and attempt to answer the following research questions: 1) "Although gazetteer-based features have been proven useful for neural NER on English, is the same true in the low-resource setting?" 2) "Do the proposed soft-gazetteer features outperform the baseline?" 3) "What types of entity mentions benefit from soft gazetteers?" and 4) "Does the knowledge base coverage affect performance?".

在這節，我們討論了四種低資源語言的實驗並嘗試回答以下研究問題： 1)雖然基礎地名詞典特徵證明對英文的神經NER有用，但是一樣對低資源語言有用嗎？ 2)提出的軟地名詞典比標準好多少？ 3) 那些類別的實體是對軟地名詞典受益的？ 4) 知識庫的覆蓋範圍會影響性能嗎？

- attempt（嘗試）
- proven（證明）
- useful（有用）
- mentions（提及）
- benefit（好處）

### 5.1 Experimental setup

**NER Dataset** We experiment on four low-resource languages: Kinyarwanda (kin), Oromo (orm), Sinhala (sin), and Tigrinya (tir). We use the LORELEI dataset (**Strassel and Tracey, 2016**),which has text from various domains, including news and social media, annotated for the NER task.

我們實驗了四個低資源語言：Kinyarwanda (kin), Oromo (orm), Sinhala (sin), and Tigrinya (tir)。我們使用了LORELEI資料集(**Strassel and Tracey, 2016**)，其中的文字來自各種不一樣的地方，包含了新聞跟社群軟體，帶標注的NER任務。

**Table 1** shows the number of sentences annotated. The data is annotated with four named entity types: locations (LOC), persons (PER), organizations (ORG), and geopolitical entities (GPE). Following the CoNLL-2003 annotation standard, we merge the LOC and GPE types (**Tjong Kim Sang and De Meulder, 2003**). Note that these datasets are very low-resource, merely 4% to 13% the size of the CoNLL-2003 English dataset.

顯示帶標注句子的數量。資料中標注有四種類別：locations (LOC), persons (PER), organizations (ORG), and geopolitical entities (GPE)。照著CoNLL-2003的標注標準，我們合併了LOC跟GPE類別(**Tjong Kim Sang and De Meulder, 2003**)。注意這些資料集資源非常少，僅僅只佔CoNLL-2003英文資料集的4%到13%。

- merely（僅僅）

These sentences are also annotated with entity links to a knowledge base of 11 million entries, which we use $only$ to aid our analysis. Of particular interest are "NIL" entity mentions that do not have a corresponding entry in the knowledge base (**Blissett and Ji, 2019**). The fraction of mentions that are NIL is shown in **Table 1**.

這些句子還帶有指向11百萬的知識庫的實體鏈接的標注，我們使用$only$來輔助我們的分析。特別感興趣的是沒有"NIL"實體相應的條目在知識庫提及(**Blissett and Ji, 2019**)。**Table 1**中顯示了提及為NIL的部份。

- aid（援助）
- analysis（分析）
- fraction（分數）

**Gazetteer Data** We also compare our method with binary gazetteer features, using entity lists from Wikipedia, the sizes of which are in **Table 1**.


