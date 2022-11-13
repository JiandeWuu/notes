# mount

## setting

### fstab

`/etc/fstab`

`mount -a`

## nfs server

check nfs 權限

`showmount -e <nfs server ip>`

會顯示可以連的 path 與 ip

## disk is busy

check who is used:

`fuser -m <disk path>`
