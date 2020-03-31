# Git 多用户配置

可能会面临这样一种场景：

我们已经有个人的 git 账户和对应的公私钥，同时在工作电脑上又用公司的账户设置了另一个 git 账户和一对公私钥。

如果需要针对不同的项目去连接不同的远端仓库，比如用个人的账户连接自己的 github 等平台；用公司的账户连接公司的 git 仓库，这就涉及到多用户配置了。

其实就算不配置，只要添加了 ssh-key 同样也是能连接的，只是由于账户不匹配，提交历史就无法被正常记录到自己的账户下。

话不多说，下面是配置过程：

首先，生成另一组账户使用的 ssh-key，同样的命令 `ssh-keygen -t rsa -C <对应账户的邮箱>`，注意在命名时区分一下，比如下面这样：

```
id_rsa
id_rsa.pub
id_rsa_github
id_rsa_github.pub
```

然后在 `~/.ssh` 目录下新建 `config` 文件，内容如下：

```
Host git.work.com
HostName git.work.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa

Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_github
```

就是配置好需要连接的各个 git host 和对应需要的 ssh-key。

通常如果没有设置多用户，在开始配置 git 的时候都会设置全局的 `user.name` 和 `user.email`，这个时候需要删除全局的设置。

```bash
git config --unset --global user.name
git config --unset --global user.email
```

接下来在各个具体的项目中配置项目使用的 `user.name` 和 `user.email`。

```bash
git config user.name xxx
git config user.email xxx
```

这样就实现了不同项目使用不同的 ssh-key 和账户。唯一麻烦一点就是没有了全局的设置，需要针对每个项目都设置一遍 user 信息。

## 不同项目对应不同 github 账户

假设现在有两个 github 账户 A 和 B，需要分别用不同的 ssh-key 去连接对应的仓库，其实也可以通过配置 config 实现。

```
Host git.a
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_a

Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_b
```

以上配置， HostName 保持一致，但是 Host 可以修改用于匹配，Host 的具体修改规则可以参考[这份文档](http://man.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man5/ssh_config.5?query=ssh_config%26arch=i386#PATTERNS)。

另外修改了 Host 之后，在项目的 remote url 设置中也需要设置 Host 部分和 config 一致以便成功匹配。 如 `git remote set-url origin git.a@xxxx`。
