# Adversarial Multi-Criteria Learning for Chinese Word Segmentation

對抗性多標準學習的中文分詞

## Abstract

Different linguistic perspectives causes many diverse segmentation criteria for Chinnese word segmentation(CWS).

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

In this paper, we propose adversarial multi-criteria learning for CWS by integrating shared knowledge from multiple heterogeneous segmentationn criteria.

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

Although there have been great achievements in building CWS corpora, they are somewhat incompatible due to differennt segmentation criteria.

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

Recently, some efforts have been made to exploit heterogeneous annotation data for Chinese word segmentationn or part-of-speech tagging(**Jiang et al., 2009; Sun and Wan, 2012; Qiu et al., 2013; Li et al., 2015, 2016**).

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

Finally, we exploit the eight segmentation criteria on the five simplified Chinese and three traditionnal Chinese corpora.
