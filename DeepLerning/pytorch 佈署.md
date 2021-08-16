# pytorch

## install lib

cpu
```cmd
wget https://download.pytorch.org/libtorch/cpu/libtorch-shared-with-deps-1.5.1%2Bcpu.zip
```

apt install make

## error

```cmd
Segmentation fault (core dumped)
```

這是因為python版本的問題，python 3.7會出此問題，3.6沒問題

```python
backpointers: List[List[int]] = []
```

必須先定義型態不然在轉c++會出錯
