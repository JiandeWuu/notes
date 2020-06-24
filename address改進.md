# 改

1. 邏輯
    `address-ner/saico_ner/data_process/pre_process.py`
    `parsing_txt function line 290`

    ```python
    # Do parse.
    for file in os.listdir(folder):
        if file in files:
    ```

可以做的：

- 亂數排序
- 挖空

填空：

- 預設必須欄位
- 一批至少有必須欄位的資料
- tarin (LSTM or BiLSTM)
- 拿 CRF 切完的資料進行 test
