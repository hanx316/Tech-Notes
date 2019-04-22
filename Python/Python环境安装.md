# Python 环境安装

对于 Windows／macOS，Python 语言环境安装非常简单。可以直接在 [Python 官网](https://www.python.org/)进入 **Downloads** 下载对应系统的一键安装包进行安装。语言版本选择 Python3.x。

另外，Python3.5 以上版本不支持 Windows Xp 系统。

Windows 系统安装时注意勾选上添加 Python 到环境变量，其他都按照默认值设定即可，确定 pip 选项已经勾上。

macOS 升级 Python 3.x 的小版本可以直接用新的安装包覆盖安装。

## Python Shell 与 IDLE

安装好 Python 以后可以通过 `python --version` 和 `python3 --version` 或者 `python -V` 和 `python3 -V` 来查看版本号。

可以通过在终端输入 `python` 进入 Python Shell 模式，可以在终端输入符合语法的代码直接查看运行结果。输入 `exit()` 退出回到终端。

macOS 由于已经自带了 Python2.x 版本，所以输入 `python` 会打开 2.x 版本的 Python Shell，可以通过 `python3` 打开 3.x 版本的。

Python 还自带了一个 Python Shell 交互工具 IDLE，可以直接打开使用。

## VS Code 配置 Python 开发环境

VS Code 可以直接写 Python 代码，但是缺乏检查和一些语言提示等支持。所以需要进行一些额外的配置。

可以参考 VS Code 官方的这份指南：[Getting Started with Python in VS Code](https://code.visualstudio.com/docs/python/python-tutorial)

1. 安装官方推荐的 Python 语言支持插件，这个现在编辑器应该会进行提示。

2. 选择 Python 语言的解释器，在 User Settings 里设置 Python Path 为 `python3` 即可。

  也可以通过 `command + shift + p` 然后输入 `python` 选择 `Select Interpreter`，主要在 macOS 上由于共存了两个版本的 Python，所以需要指定解释器。选择 3.x 版本的解释器即可，可能存在多个选项，可以任意一个。这种方式是设置当前 workspace 的解释器，会在目录下新建 .vscode 配置，其他目录并不能共享。

3. 配置语法检查 Pylint。 Python3 自带了 pip3，所以选择好解释器以后应该可以根据编辑器插件的提示进行安装，或者自己在终端手动安装 `pip3 install pylint`。
