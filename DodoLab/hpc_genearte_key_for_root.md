# hpc root ssh key setting

如果想要在一個新的設備登入 hpc 控制節點，需要先產生 ssh key。

然後再從已經有權限的設備把新產生的 ssh key 貼到控制節點的 `/root/.ssh/authorized_keys` 中

並從新啟動 ssh 服務 指令 service sshd restart

ssh 登入：
`ssh -p 15322 root@140.113.120.220`

產生 ssh key 的方式：

- puttygen
- ssh-keygen

## Puttygen

建議 Windows 使用

[Puttygen for Windows](https://www.ssh.com/academy/ssh/putty/windows/puttygen#puttygen-download-and-install)

[Puttygen for Linux](https://www.ssh.com/academy/ssh/putty/linux/puttygen)

不管是哪一個都會產生一個 `keyfile.ppk` 檔案，在 putty 中使用這個檔案即可。

如果使用 Linux 版的話會需要將產出的 ssh key 導出到 Tectia SSH 或 OpenSSH。
我沒試過大概率是坑請小心。

## ssh-keygen

建議 Linux, Mac 使用

產生金鑰：
`ssh-keygen`

> 在產生金鑰的過程中，會詢問一些問題，對於一般的使用者而言，全部都使用預設值（直接按下 Enter 鍵）即可。
>
> ```cmd
> Generating public/private rsa key pair.
>Enter file in which to save the key (/home/seal/.ssh/id_rsa):
> ```
>
> 首先指定金鑰儲存的位置，使用預設值即可，直接按下 Enter 鍵。
>
> ```cmd
> Enter passphrase (empty for no passphrase):
> ```
>
> 指定金鑰保護密碼，如果有設定密碼的話，以後每次使用都要輸入密碼，除你需要非常高的安全性，否則就不用設定了，直接按下 Enter 鍵即可。
>
> ```cmd
> Enter same passphrase again:
> ```
>
> 再次輸入密碼，直接按下 Enter 鍵，接著就會產生金鑰了。

公鑰會在：`.ssh/id_rsa.pub`

私鑰會在：`.ssh/id_rsa`

[參考](https://blog.gtwang.org/linux/linux-ssh-public-key-authentication/)
