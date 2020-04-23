# RNN（遞歸神經網路）

## 5 \ RNN
### 5.1 \ 機率與語言模型

#### 5.1.1 \ 從機率的觀點檢視 word2vec

-  CBOW 模型是利用前後文 $w_{t-1}$ 與 $w_{t+1}$ 推測目標對象 $w_{t}$ 。
- 	得到的機率算式 （5.1），或是限制左邊視窗當上下文 $w_{t-2}, w_{t-1}$ 算式（5.2）
$$
\begin{equation}
P(w_t | w_{t-1}, w_{t+1})
\end{equation} \tag{5.1}
$$

$$
\begin{equation}P(w_t | w_{t-2}, w_{t-1})\end{equation} \tag{5.2}
$$

#### 5.1.2 \ 語言模型
​	**語言模型**（Language Model）：針對字詞排列給予機率。用機率評估字詞排列發生的程度（排列的自詞有多自然）。	「you say goodbye」高機率、「you say good die」 低機率。

​	例如：機器翻譯、語音辨識...。

​	假設有 $w_1,...,w_m$ 等 $m$ 個字詞形成的文章。用 $P(w_1,...,w_m)$ 形容字詞按照 $w_1,...w_m$ 的順序出現的機率。

​	我們可以把聯合機率 $P(w_1,...,w_m)$ 分解成這樣
$$
\begin{split}
P(w_1,...,w_m) &= &P(w_m|w_1,...,w_{m-1})P(w_{m-1}|w_1,...,w_{m-2})\\
& &...P(w_3|w_1,w_2)P(w_2|w_1)\\
&= &\prod_{t=1}^{m} P(w_m|w_1,...,w_{m-1})
\end{split}\tag{5.4}
$$

​	聯合機率：同時產生多種現象的機率。

#### 5.1.3 \ 將 CBOW 模型變成語言模型

​	CBOW 跟語言模型的差異：

  - 雖然 CBOW 可以設定任意程度，卻必須「固定」長度。
  - 會忽略上下文的字詞排列。
  - 如果想要考慮排列的話參數權重會與上下文的大小比例增加。

### 5.2 \ RNN 是什麼？

​	**RNN** 的特色是有迴圈路徑。


#### 輸入
序列型的資料：$xs$，$xs：(x_1, x_2, ..., x_t, ...)$

$x_t$：某種向量。如果是處理文章，就是各個字詞的分散式標示（詞向量）。

-	corpus：語料庫，大量的文本。
- 	vocab_size：輸入長度（不同的單詞數量）
-  	wordvec_size：向量長度

**時刻**：時間序列資料的索引值（如：時刻 $t$ 的輸入資料 $x_t$ ）。

各個時刻的 RNN 層會接收對該層的輸入及前一個 RNN 層的輸出。

#### 輸出

隱藏狀態、隱藏狀態向量： $hs$，$hs：(h_1, h_2, ..., h_t, ...)$

---

$$
h_t = \tanh(h_{t-1} W_h + x_t W_x + b) \tag{5.9}
$$

---

#### tanh 函數


$$
y = \tanh(x) = \frac{e^x-e^{-x}}{e^x+e^{-x}}\tag{A.5}
$$

-	微分公式：
$$
\begin{Bmatrix}
\frac{f(x)}{g(x)}
\end{Bmatrix}' = \frac{f'(x)g(x)-f(x)g'(x)}{g(x)^2} \tag{A.6}
$$
-	皮爾常數（e）的微分：
$$
\begin{eqnarray*}
\frac{\partial e^x}{\partial x} &=& e^x \tag{A.7} \\
\frac{\partial e^{-x}}{\partial x} &=& -e^{-x} \tag{A.8}
\end{eqnarray*}
$$
-	tanh 函數的微分：
$$
\begin{split}
\frac{\partial \tanh(x)}{\partial \ x} &= \frac{(e^x+e^{-x})(e^x+e^{-x})-(e^x-e^{-x})(e^x-e^{-x})}{(e^x+e^{-x})^2}\\
&= 1 - \frac{(e^x-e^{-x})(e^x-e^{-x})}{(e^x+e^{-x})^2}\\
&= 1 - \begin{Bmatrix}\frac{(e^x-e^{-x})}{(e^x+e^{-x})}\end{Bmatrix}^2\\
&= 1 - \tanh(x)^2\\
&= 1 - y^2
\end{split}\tag{A.9}
$$

此次輸出（$h_t$）是利用輸入加上上次的輸出（$h_{t-1}$），所以 RNN 層有記憶力。

#### Backpropagation Through Time

往時間方向展開類神經網路的反向傳播法（Backpropagation Through Time, **BPTT**）

**BPTT** 的問題：

- 時間越長，反向傳播的梯度會變不穩定
- 佔用的記憶體也會增加

#### Truncated BPTT

為了解決 BPTT 的問題，使用截斷的方式在適當的長度『**切斷**』**反向**時刻傳播（類似建立多個小 RNN），這種手法就稱為 **Truncated BPTT** 。

不論時間序列資料多長，都不切斷正向傳播。

在實作得時候可以在 `TimeRNN` 中設定 `stateful` 參數來決定呼叫 `TimeRNN.forward()` 正向時都會傳入「零矩陣」。

### 評估語言模型

**困惑度**（perplexity）：評估語言模型預測能力好壞的指標。「機率的倒數」
$$
L = - \frac{1}{N} \sum_{n} \sum_{k} t_{nk} \log{y_{nk}} \tag{5.12}
$$

$$
\text{perplexity} = e^L \tag{5.13}
$$

分歧數：接下來可以選擇的數量。

困惑度也稱「平均分岐數」

## 6 \ LSTM

\#含閘門的RNN

### 6.1 \ RNN 的問題

#### 梯度爆炸

梯度大小隨著時間呈指數性成長，最後會超出負載（NaN）。

梯度剪裁（gradients clipping）：

​	模擬演算法：
$$
\begin{split}
\text{if} \|\hat{g}\|& \ge threshold:\\
	&\hat{g} = \frac{threshold}{\|\hat{g}\|}\hat{g}
\end{split}
$$
$threshold$ ：臨界值

$\hat{g}$：將兩個（所有）輸入的權重整合

$\|\hat{g}\|$：所有權重參數的平方和

#### 梯度消失

梯度大小隨著時間呈指數性減少，一但梯度變小，就不會更新權重參數無法學習長期的依存關係。

如果$$W_h$$非純量矩陣，矩陣的『奇異值』變成指標。最大值大於1，預測指數性增加的可能性較高。奇異值小於1，可以判斷會成指數性減少。但是奇異值大於1未必會梯度爆炸

### 6.2 \ 梯度消失與 LSTM

**LSTM 這種架構不會（很難）引起梯度消失。**

RNN 與 LSTM 的介面差異，LSTM 含有路徑 $c$ 稱為**記憶單元**（memory cell 或直接叫 cell）。記憶單元只在本身（LSTM層內）傳遞，不會輸出給其他層。

#### 輸出閘門（output gate）

「下一個時刻的隱藏狀態有多重要」

$x_t$ ：輸入

$h_{t-1}$ ：上一個隱藏狀態

$\sigma()$：sigmoidc 函數

$\text{o}$：輸出閘門

$$
\text{o} = \sigma(x_t W_x^{(\text{o})} +h_{t-1} W_h^{(\text{o})} + b^{(\text{o})}) \tag{6.1}
$$

$\odot$：阿達瑪乘積（hadamard product）

$h_t$：LSTM 輸出

$$
h_t = o \odot \tanh(c_t) \tag{6.2}
$$

#### 遺忘閘門（forget gate）

「要忘記什麼」

$\text{f}$：忘記閘門
$$
\text{f} = \sigma(x_t W_x^{(\text{f})} +h_{t-1} W_h^{(\text{f})} + b^{(\text{f})}) \tag{6.3}
$$

#### 新的記憶單元

加入必須記住的新資訊
$\text{g}$：新的記憶單元
$$
\text{g} = \tanh(x_t W_x^{(\text{g})} +h_{t-1} W_h^{(\text{g})} + b^{(\text{g})}) \tag{6.4}
$$

#### 輸入閘門（input gate）

取捨要增加的資訊，
$\text{i}$：輸入閘門
$$
\text{i} = \sigma(x_t W_x^{(\text{i})} +h_{t-1} W_h^{(\text{i})} + b^{(\text{i})}) \tag{6.4}
$$

#### LSTM 傳遞梯度

LSTM 的反向傳播中，不是計算「矩陣乘積」，而是『各個元素的乘積」。每個時刻、不同的閘門值計算各個元素的乘積。這樣不會（很難）發生梯度消失。

### 6.3 \ 執行 LSTM

LSTM 進行的運算：
$$
\begin{split}
\text{f} &= \sigma(x_t W_x^{(\text{f})} +h_{t-1} W_h^{(\text{f})} + b^{(\text{f})}) \\
\text{g} &= \tanh(x_t W_x^{(\text{g})} +h_{t-1} W_h^{(\text{g})} + b^{(\text{g})}) \\
\text{i} &= \sigma(x_t W_x^{(\text{i})} +h_{t-1} W_h^{(\text{i})} + b^{(\text{i})}) \\
\text{o} &= \sigma(x_t W_x^{(\text{o})} +h_{t-1} W_h^{(\text{o})} + b^{(\text{o})})
\end{split}\tag{6.6}
$$
$$
\text{c}_t = \text{f} \odot \text{c}_{t-1} + \text{g} \odot \text{i} \tag{6.7}
$$
$$
h_t = \text{o} \odot \tanh(\text{c}_t) \tag{6.8}
$$

可以將上述（6.6）的內部「仿射轉換」算式，統一成一個算式：
$$
\begin{split}
&x_t W_x^{(\text{f})} +h_{t-1}& W_h^{(\text{f})} + b^{(\text{f})} \\
&x_t W_x^{(\text{g})} +h_{t-1}& W_h^{(\text{g})} + b^{(\text{g})} \\
&x_t W_x^{(\text{i})} +h_{t-1}& W_h^{(\text{i})} + b^{(\text{i})} \\
&x_t W_x^{(\text{o})} +h_{t-1}& W_h^{(\text{o})} + b^{(\text{o})} \\
&&\bigg\downarrow \\
x_t [ W_x^{(\text{f})}\ W_x^{(\text{g})}\ W_x^{(\text{i})}\ W_x^{(\text{o})}]& + h_t [ W_h^{(\text{f})}\ W_h^{(\text{g})}&\ W_h^{(\text{i})}\ W_h^{(\text{o})}] + [ b^{(\text{f})}\ b^{(\text{g})}\ b^{(\text{i})}\ b^{(\text{o})}]\\
&&\bigg\downarrow \\
&\Large{x_t \ W_x + h_t}& \Large{ \ W_h + b}
\end{split}
$$

我們可以把四個權重（或偏權值）整合成一個，這樣只要進行一次運算就好。

乘完再分別進行活化（sigmoid、tanh），後做（6.7）（6.8）的運算，就可以得到輸出$c_t$ 、 $h_t$ 。

## 實作

為了達到 Truncated BPTT 的效果將每一層都製作由時間 $t$ 合併的 Time Layer （Time Embedding、Time RNN、Time LSTM、Time Affine ...）

簡單的模型架構：
$$
\LARGE{\text{Loss}} \\
\bigg\uparrow \\
\text{Time Softmax with Loss} \gets \LARGE{\text{ts}}\\
\bigg\uparrow \\
\text{Time Affine} \\
\bigg\uparrow \\
\text{Time LSTM} \\
\bigg\uparrow \\
\text{Time Embeding} \\
\bigg\uparrow \\
\LARGE{\text{ws}} \\
$$

$\LARGE{\text{ws}}$：輸入

$\LARGE{\text{ts}}$：訓練標籤

### 改善 LSTM

#### LSTM 層的多層化

重疊多層（LSTM），重疊多少層可以在超參數設定。以找出最合適的。

> ​	Google 翻譯重疊了八個 LSTM。



---

-	V：vocab_size，輸入長度（不同的單詞數量）
-	D：wordvec_size，向量維度
-	H：hidden_size，隱藏狀態的向量維度
-	N：批次大小
-	T：時間長度

---



我理解的活化函數的差異：