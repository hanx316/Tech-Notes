# Oh-My-Zsh 设置终端风格

unix 内核的操作系统（linux, osx），往往通过 shell 来和系统内核指令进行交互。macos 已经装好了 shell，终端默认使用的是叫做 `bash shell` 的解释器，使用 oh-my-zsh 需要将终端的 shell 解释器切换成 `zsh`

```bash
# 查看当前安装的 shell 解释器
cat /etc/shells

# 查看当前 zsh 的版本，oh-my-zsh 安装最好是5.0及以上版本的zsh
zsh --version

# 切换当前默认使用的 shell，这里切换成 zsh
chsh -s /bin/zsh
```

官网的安装命令老是提示超时，可以运行如下命令安装

```bash
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```

安装完成以后可以编辑配置文件来修改主题，找到 `ZSH_THEME="xxx"` ，试了几个，还是默认的主题好看

```bash
vi ~/.zshrc
```

然后终端修改偏好设置，将主题设置为`homebrew`

我不知道是不是我安装了 homebrew 所以有这个主题，就是那种亮绿色字体，黑色背景的黑客风格

简单这么设置一下，终端就好看多了～

#### Oh-My-Zsh 升级和卸载

打开终端分别运行 `upgrade_oh_my_zsh` 和 `uninstall_oh_my_zsh` 即可
