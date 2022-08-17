# server

## 開機

### HPC

開機順序：head, cn1, cn2, cn3, cn4

### 222, 223

1. 需先將223完成開機，並`systemctl status nfs`確認服務有起來。
2. 再將222開機，正常開完應該會掛載，如果沒有，手動執行 `mount -t nfs 140.113.120.223:/data /data`

## 關機

`shutdown`

指定時間關機：

`shutdown {hh:mm}`

倒數關機：

`shutdown {+m}`

重開：

`shutdown -r`