# 自然語言 NLP

## 詞庫

將『相同意思的字詞（同義詞）』或『意思類似的字詞（相似詞）』分類在相同的群組。

另外有時會更仔細的定義字詞之間的『上層與下層』、『整體與部分』等更詳細的關聯性。

**WordNet** : 最有名的詞庫，由普林斯頓大學在1985年開始開發的傳統詞庫。

### 詞庫的問題

- 難以因應時代變遷
- 人工作業成本昂貴
- 無法表現字詞的細微語感差異

### 計數手法

以字詞為單位

```python
>>>text
'You say goodbye and I say hello.'
>>>words
['you', 'say', 'goodbye', 'and', 'i', 'say', 'hello', '.']
```

建立字詞的ID與字詞的對應表

```python
>>>id_to_word
{0: 'you', 1: 'say', 2: 'goodbye', 3: 'and', 4: 'i', 5: 'hello', 6: '.'}
>>>word_to_id
{'you': 0, 'say': 1, 'goodbye': 2, 'and': 3, 'i': 4, 'hello': 5, '.': 6}
```

### 分散式表示

將『詞意』以向量表現

### 分布假說

『詞意是由周圍的字詞形成的』

根據字詞的『上下文（context）』形成詞意

『視窗大小（window size）』表示上下文的大小

window size 為1的 **共生矩陣**（co-occurence matrix)

||you|say|goodbye|and|i|hello|.|
|---|---|---|---|---|---|---|---|
|you|0|1|0|0|0|0|0|
|say|1|0|1|0|1|1|0|
|goodbye|0|1|0|1|0|0|0|
|and|0|0|1|0|1|0|0|
|i|0|1|0|1|0|0|0|
|hello|0|1|0|0|0|0|1|
|.|0|0|0|0|0|1|0|

### 向量之間的相似度

測量向量相似度的方法有很多種。向量內積及歐式距離（Euclidean Distance）都是極具代表性的方法。要用向量表示字詞的相似度，最常用的方法是**餘弦相似度**（cosine similarity）。

#### 餘弦相似度

表示的是『兩個向量朝同方向的程度』

當有2個向量 $$x$$,$$y$$

$$x = (x1, x2, x3,...,xn)$$

$$y = (y1, y2, y3,...,yn)$$

$$similarity(x,y) = \frac{x\cdot y}{\left\|x\right\|\left\|y\right\|} = \frac{x_1y_1+\cdots+x_ny_n}{\sqrt{x^2_1+\cdots+x^2_n}\sqrt{y^2_1+\cdots+y^2_n}}$$

經過計算『you』和『i』的餘弦相似度是0.70...。餘弦相似度介於1到-1之間，這是比較**高**的數值（**具有相似度**）

### 改善計數手法

### 點間互資訊（Pointwise Mutual Information, PMI）

『**the**』與『car』的**關聯性** 遠遠 **小於 <<<** 『**drive**』與『car』的**關聯性**

但是

『**the**』與『car』的**共生次數**卻 **大於 >>>** 『**drive**』與『car』的**共生次數**

使用**點間互資訊**的指標可以解決這種問題。

$$PMI(x,y)=\log_2\frac{P(x,y)}{P(x)P(y)}$$

- $$P(x)$$代表$$x$$發生的機率
- $$P(y)$$代表$$y$$發生的機率
- $$P(x,y)$$是指$$x$$與$$y$$共生的機率
- PMI的值越高，代表關聯性越高

$$PMI(x,y)=\log_2\frac{P(x,y)}{P(x)P(y)}=\log_2\frac{\frac{C(x,y)}{N}}{\frac{C(x)}{N}\frac{C(y)}{N}}=\log_2\frac{C(x,y) \cdot N}{C(x)C(y)}$$

- 假設共生矩陣為$$C$$
- $$C(x, y)$$代表字詞$$x$$與$$y$$的共生次數
- $$C(x)$$、$$C(y)$$表示字詞$$x$$與$$y$$出現的次數
- 語料庫內含的字詞數量為$$N$$
- 