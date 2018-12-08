# 升级OpenSSL

Mac 自带了 OpenSSL 库，不过版本比较旧，可使用 `openssl version` 可查看版本。曾经 OpenSSL 爆出过安全漏洞，如今 Mac 也改用了 LibreSSL，这些事情不扯远了，平时自己也就一般用用，不纠结。

```bash
# 确认当前命令的可执行文件地址，系统自带的是 /usr/bin/openssl
which openssl

# 通过 homebrew 安装最新的 openssl
brew update
# 未通过 homebrew 安装过
brew install openssl
# 已经安装过，直接升级
brew upgrade openssl

# 安装升级之后需要替换掉系统默认的，比较好的方式是不修改 ／usr/bin 下的自带版本，将 homebrew 安装的可执行文件软链到 /usr/local/bin 下
# homebrew 安装的包通常在 /usr/local/Cellar 下
brew list openssl
ln -s <openssl_bin_path> /usr/local/bin/openssl
```
