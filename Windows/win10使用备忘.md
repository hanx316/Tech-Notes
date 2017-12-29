# win10使用备忘

对操作系统的使用常常流于表面，有些功能或者需求偶尔用到了每次都要查也麻烦，就记录一下。

### 禁用升级服务

Win10不像之前Win7那样可以直接在系统升级选项里面禁用升级，得通过禁用服务来禁止系统自动升级

`win + r`打开运行，输入`services.msc`

然后找到Windows update服务，双击打开

先停用当前服务，然后选择禁用

### 查看系统激活的相关命令

`win + r`打开运行，输入以下命令

```bash
# 查看激活信息，包括：激活ID、安装ID、激活截止日期等信息
slmgr.vbs -dlv

# 查看系统版本、部分产品密钥、许可证状态等
slmgr.vbs -dli

# 查看是否永久激活
slmgr.vbs -xpr

# 查看系统内核版本，以及注册用户信息
winver
```

### 查看OFFICE2013激活情况

office2013在软件中只能看到是否激活，调用命令执行脚本可以查看具体的时长

```bash
cscript "C:/Program Files/Microsoft Office/Office15/ospp.vbs" /dstatus
```

安装路径替换为自己的