# path模块常用api

其实api这些看文档是最全的，不过path模块应该是非常常用，所以专门总结一下常用的几个，增加熟悉度。

```js
const path = require('path')

// 格式化
path.normalize('foo//bar//../bar')            // foo\bar

// 合并
path.join('/foo', '/baz/bar')                 // foo\baz\bar
path.join('foo/', 'baz/', '../bar')           // foo\bar

// 获取扩展名
// .开头的字符串返回空字符串，多个.取最后一个扩展
path.extname('/foo/bar/index')                // ''
path.extname('/foo/bar/index.js')             // js
path.extname('/foo/bar/index.config.js')      // js
path.extname('.js')                           // ''

// 获取文件名
path.basename('/foo/bar/baz/a.html')            // a.html
path.basename('/foo/bar/baz/a.html', '.html')   // a

// 获取目录路径，最后一级之前的路径，从返回的斜杠来看像是直接在字符串上做截取
path.dirname('/foo/bar/baz/a.html')             // /foo/bar/baz
path.dirname('/foo/bar/baz')                    // /foo/bar
path.dirname('\\foo\\bar\\baz')                 // \foo\bar

// 获取相对路径，第一个参数为起点，相对路径到第二个参数
path.relative('/foo/bar', '/foo/baz')           // ..\baz

// 获取绝对路径，可以传入多个参数，从右到左以取到的第一个绝对路径参数为止
// 如果全部都是相对路径，那么从根目录计算
path.resolve('foo/bar', '../baz')               // process.cwd() + \foo\baz
path.resolve('/foo/bar', '../baz')              // \foo\baz
path.resolve('foo/bar', '/bar/baz', 'a.html')   // \bar\baz\a.html

// process是一个全局访问变量，取得当前进程信息，调用cwd(current work directory)方法，获取当前执行脚本的工作目录（绝对路径）
process.cwd()
```

### 补充说明

- 通常返回的路径目录分隔符是平台的分隔符，比如 windows 是\，linux是 /，不过从前面代码来看，`path.dirname`有点不一样

- 只就路径而言，`path.resolve(from, ...to)`这个命令可以理解为从第一个绝对路径开始，不断执行`cd`最后查看当前路径

```bash
# path.resolve('foo/bar', '/bar/baz', 'a', 'b/c') 
cd /bar/baz
cd a
cd b/c
# windows
echo %cd% 
# linux
pwd
```
