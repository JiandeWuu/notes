# Address 訓練日記

## DATA

### {from}\_{date}\_{area}\_{rule}\_{data tag}

- from: 來源
- date: 取得日期
- area: 資料區域
- rule: 資料的規則（正規、順序、挖空）
- data tag: 附屬的tag（可能是同規則不一樣的方式已V1,V2，也可以是分檔F1,F2）

## 訓練

### 1

#### Data

- 正規
    ```
    B-City
    I-City
    B-Dist
    I-Dist
    B-Vil
    I-Vil
    B-Neighborhood
    I-Neighborhood
    B-IdLocation
    I-IdLocation
    B-Section
    I-Section
    B-Lane
    I-Lane
    B-Alley
    I-Alley
    B-AddressNumber
    I-AddressNumber
    B-Building
    I-Building
    B-Floor
    I-Floor
    B-Room
    I-Room
    O
    ```
- 順序
    從IdLation分開前後，調換前後順序
- 挖空
    隨機挖空一個已有的類別
  
#### Model

##### Bert + Softmax

##### Bilstm + CRF

###### 正規

epoch time: 2354.604201555252
all time: 11779.286279439926
