# NN
---
## 感知器
-	條件式
-	數學式
$$\Large{y=}\begin{cases}
  0, & b + w_1x_1 + w_2x_2 \leq 0\\
  1, & b + w_1x_1 + w_2x_2 > 0
\end{cases}$$

-	類別
	-	 AND
	-	 NAND
	-	 OR
	-	 XOR

---
$${(h_1,h_2,h_3,h_4) = }(x_1,x_2)\begin{pmatrix}
  w_{11} & w_{12} & w_{13} & w_{14} \\
  w_{21} & w_{22} & w_{23} & w_{24} 
\end{pmatrix}{+(b_1,b_2,b_3,b_4)}$$

![NN.png?width=1110&height=418](https://media.discordapp.net/attachments/525980572078833664/656887632407691296/NN.png?width=1110&height=418)

---
### Affine

```python
	np.dot(x,w)
```

[![](https://media.discordapp.net/attachments/525980572078833664/657080701212164097/Affine.png)](https://cdn.discordapp.com/attachments/525980572078833664/657080701212164097/Affine.png)

```python
class Affine:
    def __init__(self, W, b):
        self.params = [W, b]
        self.grads = [np.zeros_like(W), np.zeros_like(b)]
        self.x = None

    def forward(self, x):
        W, b = self.params
        out = np.dot(x, W) + b
        self.x = x
        return out

    def backward(self, dout):
        W, b = self.params
        dx = np.dot(dout, W.T)
        dW = np.dot(self.x.T, dout)
        db = np.sum(dout, axis=0)

        self.grads[0][...] = dW
        self.grads[1][...] = db
        return dx
```

---
### Sigmoid 活化函數
-	非線性
-	增加NN的表現力

![06632156.png](:storage\d6661dc5-72e2-4999-a48d-f336c5633934\06632156.png)
$$\Large{\sigma(x)=}\displaystyle{\frac{1}{1+exp(-x)}}$$

[![](https://media.discordapp.net/attachments/525980572078833664/657081997314752552/sigmoid.png)](https://cdn.discordapp.com/attachments/525980572078833664/657081997314752552/sigmoid.png)

```python
class Sigmoid:
    def __init__(self):
        self.params, self.grads = [], []
        self.out = None

    def forward(self, x):
        out = 1 / (1 + np.exp(-x))
        self.out = out
        return out

    def backward(self, dout):
        dx = dout * (1.0 - self.out) * self.out
        return dx
```

---
### Softmax
將輸入值正規化輸出

$$\huge y_k = \frac{exp(a_k)}{\sum^n_{i=1}exp(ai)}$$

輸出值總和1
`[0.1,0.05,0.1,0.0,0.05,0.1,0.0,0.6,0.0,0.0]`

```python
def softmax(x):
    if x.ndim == 2:
        x = x - x.max(axis=1, keepdims=True)
        x = np.exp(x)
        x /= x.sum(axis=1, keepdims=True)
    elif x.ndim == 1:
        x = x - np.max(x)
        x = np.exp(x) / np.sum(np.exp(x))

    return x

class Softmax:
    def __init__(self):
        self.params, self.grads = [], []
        self.out = None

    def forward(self, x):
        self.out = softmax(x)
        return self.out

    def backward(self, dout):
        dx = self.out * dout
        sumdx = np.sum(dx, axis=1, keepdims=True)
        dx -= self.out * sumdx
        return dx
```

---

### 損失函數
$$\Large y_k$$ : NN的輸出
$$\Large t_k$$ : 訓練資料
$$\Large k$$ : 資料的維度

-	均方誤差

$$\Large E = \frac {1}{2}\sum_k{(y_k - t_k)}$$

```python
	def mean_squared_error(y, t):
		return 0.5 * np.sum((y - t) ** 2)
```

-	交叉熵誤差
	-	只看正確答案的機率	

$$\Large E = - \sum_k{t_k \log y_k}$$

```python
	def cross_entropy_error(y, t):
		delta = 1e-7
		return -np.sum(t * np.log(y + delta))
```

**加上微小值 delta 防止出現負無限大**
**小批次**
$$\Large L = - \frac {1}{N}\sum_n\sum_k{t_{nk} \log y_{nk}}$$
$$\Large t_{nk}$$ : 第 $$n$$ 個資料第 $$k$$ 維的值
$$\Large y_{nk}$$ : NN的輸出

```python
class SoftmaxWithLoss:
    def __init__(self):
        self.params, self.grads = [], []
        self.y = None  # softmax的輸出
        self.t = None  # 訓練標籤

    def forward(self, x, t):
        self.t = t
        self.y = softmax(x)

        # 訓練標籤為one-hot向量時，轉換成正解標籤的索引值        if self.t.size == self.y.size:
            self.t = self.t.argmax(axis=1)

        loss = cross_entropy_error(self.y, self.t)
        return loss

    def backward(self, dout=1):
        batch_size = self.t.shape[0]

        dx = self.y.copy()
        dx[np.arange(batch_size), self.t] -= 1
        dx *= dout
        dx = dx / batch_size

        return dx
```

----

### 節點
#### Repeat
```python
D, N = 8, 7
x = np.random.randn(1, D)  				#輸入
y = np.repeat(x, N, axis=0)				#forward

dy = np.random.randn(N, D)				#假設的梯度
dx = np.sum(dy, axis=0, keepdims=True)	#backward
```
-	forward 是複製
-	backward 是加總

#### Sum
```python
D, N = 8, 7
x = np.random.randn(1, D)  					#輸入
y = np.sum(x, N, axis=0)					#forward

dy = np.random.randn(N, D)					#假設的梯度
dx = np.repeat(dy, axis=0, keepdims=True)	#backward
```
-	forward 是加總
-	backward 是複製

#### MatMul
![MatMul.png](https://cdn.discordapp.com/attachments/525980572078833664/656888350740971543/MatMul.png)
$$\huge{\frac{\partial L}{\partial x_i} = \sum_j\frac{\partial L}{\partial y_j}\frac{\partial y_j}{\partial x_i} = \sum_j\frac{\partial L}{\partial y_j}W_{ij}}$$

$$\huge{\frac{\partial L}{\partial x} = \frac{\partial L}{\partial y_j}W_{ij}}$$

```python
class MatMul:
    def __init__(self, W):
        self.params = [W]
        self.grads = [np.zeros_like(W)]
        self.x = None

    def forward(self, x):
        W, = self.params
        out = np.dot(x, W)
        self.x = x
        return out

    def backward(self, dout):
        W, = self.params
        dx = np.dot(dout, W.T)
        dW = np.dot(self.x.T, dout)
        self.grads[0][...] = dW
        return dx
```

#### 


---
#### 附錄
1.	關於 grads\[0]\[...] 的三個點是【深拷貝】，變數的記憶體位址並不會改變，改變的是值