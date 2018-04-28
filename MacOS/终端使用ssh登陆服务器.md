# 终端使用ssh登陆服务器

macos已经默认为我们装好了ssh，可以直接使用

```bash
# 查看ssh可执行文件
whereis ssh
```

#### 基本方法

可以使用`ssh username@ip_address`来登陆远程服务器主机，当然前提是服务端的主机开启了ssh服务，输入该命令后会提示输入密码，正确输入后即可连接，首次连接时会提示host的相关信息，输入yes即可

一般情况下都会用root用户来登陆（可以不输入用户名，直接输入ip），也有出于安全考虑，而采用其他用户，或者修改root用户的权限。如果遇到输入密码正确，但仍然提示`Permission denied`，可以考虑是服务端没有开通对应用户的ssh权限

#### 偷懒一点的方法

以上方法已经可以访问到服务器了，但是显然每次访问都要输入一长串ip地址作为命令是非常麻烦的事

那么可以考虑将以上ssh连带ip地址的整个命令设置一个命令别名，今后使用时直接输入别名即可，这也不失为一种方法

还有一种方法就是下面要提到的，`vi ~/.ssh/config`，然后设置host的别名（按如下设置）

```bash
# 服务器1
Host 别名
  HostName IP地址
  Port 22
  User 用户名
# 服务器2
Host 别名
  HostName IP地址
  Port 22
  User 用户名
```

保存以后就可以直接通过`ssh host别名`来连接服务器了

如果一时忘了别名，也可以通过`cat ~/.ssh/config`或者`cat ~/.ssh/config | grep 'Host'`来查看

#### 密钥方式连接

首先将公钥写入服务器`~/.ssh/authorized_keys`，如果没有生成过ssh-key可以使用`ssh-keygen`命令生成

接下来打开`/etc/ssh/sshd_config`文件，确保开启认证，比如像下面那样默认注释是yes就不用管

```
#RSAAuthentication yes
#PubkeyAuthentication yes
```

接下来重启服务即可`service sshd restart`