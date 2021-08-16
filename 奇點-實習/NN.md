---
# NN
---

## 感知機

AND, NAND, OR, XOR

-
### AND Gate
-
![](https://github.com/jzxc56788/images/blob/master/AND_Gate.png?raw=true)

$$x_1$$|$$x_2$$|$$y$$
---|---|---|---
0|0|0
1|0|0
0|1|0
1|1|1
-
### NAND Gate
-
![](https://github.com/jzxc56788/images/blob/master/NAND_Gate.png?raw=true)

$$x_1$$|$$x_2$$|$$y$$
---|---|---|---
0|0|1
1|0|1
0|1|1
1|1|0
-
### OR Gate
-
![](https://github.com/jzxc56788/images/blob/master/OR_Gate.png?raw=true)

$$x_1$$|$$x_2$$|$$y$$
---|---|---|---
0|0|0
1|0|1
0|1|1
1|1|1
-
### XOR Gate
-
![](https://github.com/jzxc56788/images/blob/master/XOR_Gate.png?raw=true)

XOR 無法用一條直線劃分，所以是**非線性**的

$$x_1$$|$$x_2$$|NAND|OR|$$y$$
---|---|---|---|---|---
0|0|1|0|0|0
1|0|1|1|1
0|1|1|1|1
1|1|0|1|0

下面是圖示表示XOR 感知器內部

![](https://github.com/jzxc56788/images/blob/master/XOR_perceptron.png?raw=true)

所以我們可以發現用多個線性感知器組合可以解決非線性問題

---

## 活化函數
>	將輸入訊號的總和轉換成輸出訊號的函數，具有決定如何發火的功能。

感知機：將訊號分類（變簡單）的函數。

神經網路：將Aiffine層輸出的結果（線性），帶來『非線性』的效果。（將數個活化後的結果再次運算。）
>	感知機的活化函數與神經網路的活化函數之間的差別。<br>
>	感知機：階梯函數。<br>
>	神經網路：other。ex : sigmoid

### sigmoid函數與階梯函數的差別
![](https://github.com/jzxc56788/images/blob/master/sigmoid_vs_stepfunction.png?raw=true)

橘色：階梯函數。藍色：sigmoid。

> 	首先是『平滑度』的差異。sigmoid函數是平滑曲線，針對輸入產生連續性的輸出；然而，階梯函數是以0為界線來改變輸出。學習神經網路時，sigmoid函數的平滑度具有重要意義。
>
>	階梯函數只能回傳0或1其中一個值，而sigmoid函數可以回傳0到1之間的實數。

正因為『平滑度』使得輸出可以在0到1之間遊走，達成學習的效果。

---
## NN概念

>	類神經網路只是一個『函數』。丟某個輸入轉換後給出某個輸出。

利用感知機的概念，將多個線性結果的組合達到非線性的答案。
![](https://github.com/jzxc56788/images/blob/master/QpM15R6.png?raw=true)
>	以權重來說是雙層，以結構的來說是三層。

$$(h_1,h_2,h_3,h_4)=(x_1,x_2)\begin{pmatrix}w_{11}&w_{12}&w_{13}&w_{14}\\w_{21}&w_{22}&w_{23}&w_{24}\end{pmatrix}+(b_1,b_2,b_3,b_4)$$
$$h_1 = x_1w_{11}+x_2w_{21}+b_1$$

>	計算矩陣：x(1,2)，w(2,4)，h(1,4)
>	x(N,I)，w(I,H)，h(N,H)
>	Ｎ：一次小批次樣本的數量
>	Ｉ：一個樣本的參數數量
>	Ｈ：中間層的神經元數量

---
## 分層概念
![](https://github.com/jzxc56788/images/blob/master/NN_Layer.png?raw=true)
兩層式的分層式神經網路

### 計算、Affine 層

-	計算與乘法
-	可以設定神經元數量，每個神經元都有自己的權重跟偏權值（bias）。

神經元包含Affine與活化函數

### 活化層、sigmoid 層
-	將 Affine 層的結果進行活化

$$\sigma (x) = \frac{1}{1+\exp(-x)}$$

### 輸出層、softmax層
softmax函數：
$$y_k=\frac{\exp(a_k)}{\sum^n_{i=1}\exp(a_i)}$$

-	$$y_k$$：是$$k$$類別的輸出機率。
- 	$$a_k$$：是$$k$$類別的計算後的結果。
-	可以算出每個輸出類別的比例（機率），正規化。

### 損失函數
>	類神經網路的學習中，需要有知道學習狀態的『指標』。一般而言，這項指標稱作損失（loss）。
>	損失指的是在學習階段，顯示類神經網路效能的指標。
>	多類別分類的類神經網路，通常會使用交叉熵誤差（Cross Entropy Error）。
>	交叉熵誤差是由類神經網路輸出各類別的『機率』及『訓練標籤』計算出來。

交叉熵誤差：
$$E = -\sum_k{t_k\log{y_k}}$$

-	$$t_k$$：是$$k$$類別的正確結果

t|0|0|1|0|0|0|0|0|0|0
---|---|---|---|---|---|---|---|---|---|---|---|---
y1|0.1|0.05|0.6|0|0.05|0|0|0.1|0|0
y2|0.1|0.05|0.1|0|0.05|0|0|0.6|0|0

-	y1：0.5108...
- 	y2：2.3025...
- 	**越低越好**

## 微分與梯度
### 反向傳播

### 更新權重
-	Step-1（小批次）
	從訓練資料中，隨機挑選多筆資料
-	Step-2（算出梯度）
	利用反向傳播法，求出與各個權重參數相關的損失函數之梯度
-	Step-3（更新參數）
	使用梯度，更新權重參數
-	Step-4（重複）
	根據需要，重複執行多次
	
**SGD**（Stochastic Gradient Descent：隨機梯度下降法）隨機是指針對隨機挑選出來的資料（小批次）使用梯度的意思。

$$W\gets W- \eta \frac{\partial L}{\partial W} $$

>	假設要更新的權重參數為$$W$$，與$$W$$有關的損失函數之梯度為$$\frac{\partial L}{\partial W}$$。$$\eta$$代表學習係數，實際上已經先決定使用ㄅㄢ0.01或0.001等數值。

