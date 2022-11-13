# network

## 兩台 linux 網路線對接

- 先 check 燈號
- `ifconfig` 確認有那些網孔編號
- `ethtool <網孔編號>` 確認哪個網孔有連接（看有無 speed）
  - 與 `ifconfig` 確認那個網孔有連接但是沒有設定
- 設定靜態IP
  - `/etc/sysconfig/network-scripts/ifcfg-<網孔編號>`

```txt
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp0s3
UUID=fc289d60-31ad-4d3c-a668-14d7445efa41
DEVICE=enp0s3
ONBOOT=yes
IPADDR=192.168.56.10
GATEWAY=102.168.56.1
NETWORK=192.168.56.0
NETMASK=255.255.255.0
DNS1=8.8.8.8
DNS2=9.9.9.9
```

-  啟動網路介面
   - `ifup enp0s3`
- 停用網路介面
  - `ifdown enp0s3`
