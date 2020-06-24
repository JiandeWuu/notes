# Python 虛擬環境

## 建立虛擬環境

> python3：`$ python3 -m venv tutorial-env`
>
> virtualenv ：`$ virtualenv test_env01(环境名称，文件夹名称)`
>
> pyenv: `$ pyenv virtualenv {版號} {虛擬環境名稱}`

## 呼叫虛擬環境

`$ source 環境資料夾名稱/bin/activate (activate路径)`
pyenv: `$ pyenv activate {虛擬環境名稱}`

## 退出環境

`$ deactivate`
pyenv: `$ pyenv deactivate`

cupy
cudnn

列出所有已安裝的套件：
`
$ pip freeze
`

- 終端機
- venv
- pipenv

## VScode

settings.json
> "terminal.integrated.shell.osx": "/usr/local/bin/zsh",
> "terminal.external.osxExec": "iTerm.app",
> "terminal.integrated.fontFamily": "SauceCodePro Nerd Font"

## install python

### ubuntu

`sudo apt update`
`sudo apt install python3-pip`

## install pyenv

`curl https://pyenv.run | bash`

[https://github.com/pyenv/pyenv-installer](https://github.com/pyenv/pyenv-installer)
[https://www.twblogs.net/a/5d693d42bd9eee5327feee9b](https://www.twblogs.net/a/5d693d42bd9eee5327feee9b)

`vim ~/.bashrc`

```export PATH="~/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"```

激活
`source ~/.bashrc`
