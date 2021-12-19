# Homebrew 使用笔记

[Homebrew 官网](https://brew.sh/)

#### 安装

主要是备好梯子，以及终端代理和 git ssh 代理

安装脚本重写源地址，可另存为 ruby 脚本，通过 `ruby xxx.rb` 执行

```
BREW_REPO = "git://mirrors.ustc.edu.cn/brew.git".freeze

CORE_TAP_REPO = "git://mirrors.ustc.edu.cn/homebrew-core.git".freeze
```

homebrew-core clone 报错

```
// 执行下面这句命令，更换为中科院的镜像：
git clone git://mirrors.ustc.edu.cn/homebrew-core.git/ /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core --depth=1

// 把homebrew-core的镜像地址也设为中科院的国内镜像

cd "$(brew --repo)" 

git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core" 

git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git

// 更新
brew update
```
