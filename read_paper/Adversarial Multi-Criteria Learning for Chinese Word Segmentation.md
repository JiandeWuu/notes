# Adversarial Multi-Criteria Learning for Chinese Word Segmentation

對抗性多標準學習的中文分詞

## Abstract

Different linguistic perspectives causes many diverse segmentation criteria for Chinese word segmentation(CWS).

因為不一樣的語言觀點所以有多樣的分詞標準在中文分詞上。

- linguistic（語言學）
- perspectives（觀點）
- diverse（多樣）

Most existing methods focus on improve the performance for each single criterion.

更多現有的方法專注在提高單一分詞標準的性能。

- existing（現有）

However, it is interesting to exploit these different criteria and mining their common underlying knowledge.

然而利用不同的標準去挖掘他們共同的底層知識是很有趣的。

- exploit（利用）
- mining（挖掘）
- common（共同）
- underlying（底層的）

In this paper, we propose adversarial multi-criteria learning for CWS by integrating shared knowledge from multiple heterogeneous segmentation criteria.

在這篇論文中我們提出了對抗性多標準學習的中文分詞來整合多個不一樣分詞標準的公開的知識。

- integrating（整合）
- heterogeneous（異質）

Experiments on eight corpora with heterogeneous segmentation criteria show that the performance of each corpus obtains a significant improvement, compared to single-criterion learning.

在實驗了8個不一樣分詞標準的語料後顯示了，比單一分詞標準的performance來說有著很大改善。

- experiments（實驗）
- significant（重大）
- improvement（改善）
- compared（比較）

## Introduction

Chinese word segmentation (CWS) is a preliminary and important task for Chinese natural language processing (NLP).

中文分詞是中文自然語意處理最初也重要的任務。

- preliminary （初步）

Currently, the state-of-the-art methods are based on statistical supervised learning algorithms, and rely on a large-scale annotated corpus whose cost is extremely expensive.

目前最先進的方法是基於是統計監督學習算法，依靠大量的帶註譯的語料他的成本是非常昂貴的。

- currently（目前）
- state-of-the-art（最先進的）
- statistical（統計）
- rely（依靠）
- large-scale（大規模）
- annotated（帶註譯）
- extremely（非常）
- expensive（昂貴）

Although there have been great achievements in building CWS corpora, they are somewhat incompatible due to different segmentation criteria.

雖然他在中文分詞語料庫有著傑出的成就，由於在不一樣分詞規則會有一些不相容。

- Although（雖然）
- achievements（成就）
- incompatible（不相容）
- due to（由於）

As shown in Table 1, given a sentence "姚明进入总决赛(YaoMing reaches the final)", the two commonly-used corpora, PKU's People's Daily(PKU)(**Yu et al., 2001**) and Penn Chinese Treebank(CTB)(**Fei, 2000**), use different segmentation criteria.

表1，給字串"姚明进入总决赛"在兩個常用的語料庫（PKU）（CTB）使用不同的分詞標準的狀況。

- commonly-used（常用）
- Daily（日常）

In a sense, it is a waste of resources if we fail to fully exploit these corpora.

在某種意義上，如果它沒有充分利用他的語料庫會浪費資料。

- In a sense（在某種意義上）
- resources（資源）
- exploit（利用）

Recently, some efforts have been made to exploit heterogeneous annotation data for Chinese word segmentation or part-of-speech tagging(**Jiang et al., 2009; Sun and Wan, 2012; Qiu et al., 2013; Li et al., 2015, 2016**).

最近一些努力在使用不一樣的帶註譯的資料在做中文分詞或是詞性標注的論文。

- Recently（最近）
- efforts（努力）
- part-of-speech（詞性）

These methods adopted stacking or multi-task architectures and showed that heterogeneous corpora can help each other.

這些方法被採用堆疊或是多任務結構，並且展現了不一樣的語料庫使可以互相幫助。

- adopted（被採用）
- architectures（建築）

However, most of these model adopt the shallow linear classifier with discrete features, which makes if difficult to design the shared feature spaces, usually resulting in a complex model.

然而大多數的模型都採用有離散特徵的淺線性類別分類器，這使得難以設計共享特徵空間，通常會導致模型複雜。

- shallow（淺層）
- discrete（離散）

Fortunately, recent deep neural models provide a convenient way to share information among multiple task(**Collobert and Weston, 2008; Luong et al., 2015; Chen et al., 2016**).

幸好最近深度神經網路模型提供了在多任務之間共享信息的方便方法。

- Fortunately（幸好）
- convenient（方便）

In this paper, we propose an adversarial multi-criteria learning for CWS by integrating shared knowledge from multiple segmentation criteria.

在這篇論文中，我們通過整合了多個分詞標準的共享知識，提出了一個給中文分詞的對抗性多標準學習。

Specifically, we regard each segmentation criterion as a single task and propose three different shared-private models under the framework of multi-task learning(**Caruana, 1997; B3n-David and Schuller, 2003**), where a shared layer is used to extract the criteria-invariant features, and a private layer is used to extract the criteria-specific features.

具體來說，我們將每個分詞標準都當作單一任務，並在多任務學習的框架下提出三個不一樣的共享 私人模型，共享層是用來提取標準不變的特徵，私人層是為了提取特定標準的特徵。

- regard（看待）
- extract（提取）
- invariant（不變的）

Inspired by the success of adversarial strategy on domain adaption(**Ajakan et al., 2014; Ganin et al., 2016; Bousmalis et al., 2016**), we further utilize adversarial strategy to make sure the shared layer can extract the common underlying and criteria-invariant features, which are suitable for all the criteria.

受到已經在領域適應上取得成功的對抗性策略的啟發，我們更進一步利用對抗性策略來確保分享層可以提取適合所有標準的共同基礎跟不變標準的特徵。

- Inspired（啟發）
- strategy（策略）
- adaption（適應性）
- utilize（利用）
- suitable（適當）

Finally, we exploit the eight segmentation criteria on the five simplified Chinese and three traditional Chinese corpora.

最後我們將八個分詞標準使用在五個簡體中文跟三個繁體中文語料庫。

- simplified Chinese（簡體中文）
- traditional Chinese（繁體中文）

Experiments show that our models are effective to improve the performance for CWS.

實驗顯示我們的模型可有效提高中文分詞的性能。

We also observe that traditional Chinese could benefit from incorporating knowledge from simplified Chinese.

我們也觀察到繁體中文可以受益從簡體中文中吸收知識。

- observe（觀察）
- benefit（效益）
- incorporating（合併）

The contributions of this paper could summarized as follows.

這篇論文的貢獻可以總結如下。

- contributions（貢獻）
- summarized（總結）

1. Multi-criteria learning is first introduced for CWS, in which we propose three shared-private models to integrate multiple segmentation criteria.
  首先為中文分詞引進多標準學習，其中我提出三個共享-私人模型來整合多分詞標準。

- introduced（引進）
- integrate（整合）

2. An adversarial strategy is used to force the shared layer to learn criteria-invariant features,in which an new objective function is also proposed instead of the original cross-entropy loss.

對抗性策略是強制共享層去學習標準不變特徵，其中還提出新的目標函數替代原本的cross-entropy loss。

- force（強制）
- objective（目的）
- instead（代替）
  
3. We conduct extensive experiments on eight CWS corpora with different segmentation criteria, which is by far the largest number of datasets used simultaneously.

我們進行大規模實驗在八個中文分詞語料庫於不一樣的分詞標準，這是現在為止同時使用的最大datasets。

- conduct（進行）
- extensive（廣泛、大規模）
- largest（最大的）
- simultaneously（同時）

## General Neural Model for Chinese Word Segmentation

Chinese word segmentation task is usually regarded as a character based sequence labeling problem.

中文分詞任務通常被視為基於字符的序列標注問題。

- regarded（被視為）
- character（字符）

Specifically, each character in a sentence is labeled as one of $L = {B, M, E, S}$, indicating the begin, middle, end of a word, or a word with single character.

具體來說，句子中每個字符都有一個標注（B, M, E, S)，表示在一個詞的開始，中間，結束，或者詞就是單一的字符。

- indicating（指示）

There are lots of prevalent methods to solve sequence labeling problem such as maximum entropy Markov model(MEMM), conditional random fields(CRF), etc.

解決序列標籤問題的方法很多例如馬可夫練模型，CRF等等。

- prevalent（流行）
- solve（解決）
- conditional（有條件的）

Recently, neural networks are widely applied to Chinese word segmentation task for their ability to minimize the effort in feature engineering(**Zheng et al., 2013; Pei et al., 2014; Chen et al., 2015a,b**).

最近神經網路因為能夠最大程度地減少特徵工程工作量的能力而被廣泛應用於中文分詞任務。

- widely（廣泛）
- applied（應用於）
- ability（能力）

Specifically, given a sequence with $n$ characters $X = \{ x_1,\ldots,x_n \}$, the aim of CWS task is to figure out the ground truth of labels $Y^* = \{ y^*_1, \ldots, y^*_n\}$ :


- 具體來說給定一個有$n$個字符的序列$X = \{ x_1,\ldots,x_n \}$，中文分詞的目的就是弄清楚標籤的基本含意$Y^* = \{ y^*_1, \ldots, y^*_n\}$。

- figure out（弄清楚）
- ground truth（基本事實）

$$Y^*=\argmax p(Y|X), \tag{1}$$

where $\mathcal L = \{ B,M,E,S \}$.

The general architecture of neural CWS could be characterized by three component: (1) a character embedding layer; (2) feature layers consisting of several classical neural networks and (3) a tag inference layer.

神經網路的中文分詞一般分為三個部份組成:(1)字符embedding層 (2)一些神經網路所組成的特徵層跟(3)一個標籤推理層。

- characterized（特色的）
- component（組成）
- consisting（組成）
- several（一些）
- inference（推理）

The role of feature layers is to extract features, which could be either convolution neural network or recurrent neural network.

特徵層是提取特徵，可以是卷積神經網路或者是遞歸神經網路其中一個。

- role（角色）
- either（或）
- convolution（卷積）
- recurrent（反覆發作）

In this paper, we adopt the bi-directional long short-term memory neural networks followed by CRF as the tag inference layer.

在這篇論文中，我們採用LSTM加上CRF推測標籤層。

- directional（定向的）

### 2.1 Embedding layer

In neural models, the first step usually is to map discrete language symbols to distributed embedding vectors.

這神經網路，第一步通常是將離散語言符號映射到分散式embedding向量。

- symbols（符號）
- distributed（分散式）

Formally, we lookup embedding vector from embedding matrix for each character $x_i$ as $e_{x_i} \in \mathbb{R}^{d_e}$, where $d_e$ is a hyper-parameter indicating the size of character embedding.

形式上，我們從每個字符$x_i$的embedding矩陣中查找embedding向量，作為$e_{x_i}\in \mathbb{R}^{d_e}$，其中$d_e$是指字符embedding大小的超參數。

- Formally（正式地）

### 2.2 Feature layers

We adopt bi-directional long short-term memory (Bi-LSTM) as feature layers.

我們採用Bi-LSTM當特徵層。

While there are numerous LSTM variants, here we use the LSTM architecture used by(**Jozefowicz et al., 2015**), which is similar to the architecture of (**Graves, 2013**) but without peep-hole connections.

雖然LSTM有許多變體，我們使用這篇論文(**Jozefowicz et al., 2015**)的LSTM算法，與(**Graves, 2013**)類似的算法但是沒有窺視孔連接。

- numerous（眾多）
- variants（變體）
- similar（類似）
- peep-hole（窺孔）

**LSTM** introduces gate mechanism and memory cell to maintain long dependency information and avoid gradient vanishing.

LSTM引入門機制跟儲存單元幫助保持長期的依賴訊息跟避免梯度消失。

- introduces（介紹）
- mechanism（機制）
- maintain（保持）
- dependency（依賴）
- avoid（避免）
- vanishing（消失）

Formally, LSTM, with input gate **i**, output gate **o**, forget gate **f** and memory cell **c**, could be expressed as:

$$
\begin{bmatrix}
\textbf{i}_i \\
\textbf{o}_i \\
\textbf{f}_i \\
\widetilde{\textbf{c}}_i
\end{bmatrix}
=
\begin{bmatrix}
\sigma  \\
\sigma  \\
\sigma  \\
\phi
\end{bmatrix}
(\textbf{W}_g^\intercal\begin{bmatrix}
\textbf{e}_{x_i}  \\
\textbf{h}_{i-1}
\end{bmatrix} + b_g), \tag{2}
$$
$$
\textbf{c}_i = \textbf{c}_{i-1} \odot \textbf{f}_i + \widetilde{\textbf{c}}_i \odot \textbf{i}_i, \tag{3}
$$
$$
\textbf{h}_i = \textbf{o}_{i-1} \odot \phi(\textbf{c}_i), \tag{4}
$$

where $\textbf{W}_g \in \mathbb{R}^{(d-e+d_h) \times 4d_h}$ and $\textbf{b}_g \in \mathbb{R}^{4d_h}$ are trainable parameters.

- trainable（可訓練的）

$d_h$ is a hyper-parameter, indicating the hidden state size.

$d_h$是超參數，指示hidden狀態的大小。

Function $\phi(.)$ and $\odot(.)$ are sigmoid and tanh functions respectively.

$\phi(.)$ and $\odot(.)$分別是sigmoid and tanh。

- respectively（分別）

**Bi-LSTM** In order to incorporate information from both sides of sequence, we use bi-directional LSTM(Bi-LSTM) with forward and backward directions.

為了整合序列兩側的信息，我們使用有向前與向後的BiLSTM。

- incorporate（合併）
- directions（指示）

The update of each Bi-LSTM unit can be written precisely as follows:
$$
\mathbb{h}_i = \overrightarrow{\mathbb{h}}_i\oplus\overleftarrow{\mathbb{h}}_i, \tag{5}
$$
$$
= Bi-LSTM(\textbf{e}_{x_i}, \overrightarrow{\mathbb{h}}_{i-1}, \overleftarrow{\mathbb{h}}_{i+1},\theta), \tag{6}
$$

BiLSTM神經元的更新可以精確地寫成這樣：

- written precisely（精確地寫）

where $\overrightarrow{\mathbb{h}}_i$ and $\overleftarrow{\mathbb{h}}_i$ are the hidden states at position $i$ of the forward and backward LSTMs respectively; $\oplus$ is concatenation operation; $\theta$ denotes all parameters in Bi-LSTM model.

$\overrightarrow{\mathbb{h}}_i$ and $\overleftarrow{\mathbb{h}}_i$分別表示LSTMs在hidden位置$i$的正向與反向;$\oplus$是並列操作;$\theta$表示Bi-LSTM的所有參數。

- concatenation（並列）
- operation（操作）
- denotes（表示）

### 2.3 Inference Layer

After extracting features, we employ conditional random fields(CRF)(**Lafferty et al., 2001**) layer to inference tags.
