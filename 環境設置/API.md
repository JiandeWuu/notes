請執行以下步驟（在Ubuntu 18.04服務器上工作）：
1- sudo add-apt-repository multiverse && sudo apt update
2- sudo apt install virtualbox
不要在服務器中對其進行測試，因為該服務器沒有“ X顯示”。然後，在安裝“ virtualbox”後，運行docker命令：
3- docker-machine create -d virtualbox 'your-virtualbox-name'

## 修改 linux 系統併發數

sudo vim /etc/security/limits.conf

```cmd
*       soft    nofile  65534
*       hard    nofile  65534
root    sorft   nofile  65534
root    hard    nofile  65534
```

sudo vim /etc/sysctl.conf

```cmd
session required pam_limits.so
```

sudo vim /etc/profile

```cmd
ulimit -SHn 65534
```
