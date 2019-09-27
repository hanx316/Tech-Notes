# Git 语言设置

最近我通过 Homebrew 升级了 Git 到 2.23.0 版本，然后语言就突然变成中文了，看着翻译的内容还挺有意思的，也能更好地理解 Git 中的一些内容。

不过，所谓一顿键盘操作猛如虎，然后 Terminal 显示了一堆中文，多少感觉有点那啥。根据经验，Git 应该有语言的相关配置。

本文源于此番折腾。

## 为什么变成了中文

`brew info git` 可以看到 git 安装的目录。

在 `../git/2.23.0/share/locale` 目录下有 i18n 相关的内容文件。中文相关的内容目测应该就是来自 zh_CN 目录。

zh_CN 目录下只有一个 LC_MESSAGES，看来 git 只配置了 message 相关的文本。

这个 locale 目录大概是随着某个版本升级弄进去的。但是也有朋友同样通过 Homebrew 升级之后并没有显示中文，不知道是不是跟我之前有运行过 `git gui` 命令有关，这点就不继续深入探索了。

## 怎么改回英文

知道了怎么来，要改回去就有思路了，无非是方式优雅不优雅而已。

最简单，删掉相应的语言文本目录，或者重命名目录，Git 就会显示回默认的英语。

当然我更倾向于 Git 本身能够配置自己的语言，但是查了文档所有 locale / language / config 等等内容，都没有发现相应的配置。

最后只能通过 [stackoverflow](https://stackoverflow.com/questions/10633564/how-does-one-change-the-language-of-the-command-line-interface-of-git) 上提供的方式从 Terminal 入手。

由于 Git 其实只有改变 LC_MESSAGES，所以在 runtime configs 里设置 `alias git="LC_MESSAGES=C git"` 就行了，不用设置 LANG 或者 LANGUAGE 等变量。

关于 LANG / LANGUAGE / LC_MESSAGES / LC_ALL 等等变量的区别，搜索 `lang language lc_all` 可以查到一篇烂大街的文章，[locale的设定及LANG、LC_CTYPE、LC_ALL环境变量](https://www.cnblogs.com/xlmeng1988/archive/2013/01/16/locale.html) 简单了解一下。

更详细考究的内容可以参考 [GNU gettext utilities](http://www.gnu.org/software/gettext/manual/gettext.html#Locale-Environment-Variables)

如果升级了但是又没有显示中文，看一下安装目录确认有语言文件的话，添加 `alias git="LC_MESSAGES=zh_CN git"` 应该就会显示中文吧。
