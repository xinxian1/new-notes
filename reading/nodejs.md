# node.js模块化开发

开发规范：

- node.js规定一个**JavaScript文件**就是一个模块，模块**内部定义的变量和函数**默认情况下在**外部无法得到**。
- 模块内部可以使用**exports对象进行成员导出**，使用**require方法**导出其他模块。

```javascript
// a.js
// 在模块内部定义变量
let version = 1.0;
// 在模块内部定义方法
const sayHi = name => '你好, ${name}';
// 向模块外部导出数据
exports.version = version;
exports.sayHi = sayHi;
```

```javascript
// b.js 
// 在b.js模块中导入模块a
let a = require('./b.js');
// .js 可以省略
// 输出b模块中的version变量
console.log(a.version);
// 调用b模块中的sayHi方法 并输出其返回值
console.log(a.sayHi('黑马讲师'));
```

导入模块时后缀可以省略

模块成员导出的另外一种方式

```javas
module.exports.version = version;
module.exports.sayHi = sayHi;
```

**exports**是**module.exports**的别名**（地址引用关系），导出对象最终以module.exports为准**

exports                  ----------->  {

​														version:10,

module.exports   ---------->		  sayHi:sayHi

​												}

```javascript
exports.version = version;
module.exports.version = version;
```

```javascript
module.exports = {
    name: 'zhansan',
}
```

这时候module.exports 指向这个  以module.exports 为准。

# 系统模块

node运行环境提供的API。因为这些API都是以模块化的方式进行开发的，所以我们又称node运行环境提供的API为系统化模块。

fs 文件操作

f：file文件 ，s：system 系统 ， 文件操作系统。

```javascript
const fs = require('fs');
```

读取文件内容

```javascript
fs.readFile('文件路径/文件名称'[,'文件编码'],callback);
```

callback 回调函数

读取文件语法示例

```javascript
// 读取上一级css目录下中的base.css
fs.readFile('../css/base.css','utf-8', (err,doc) => {
    // 如果文件读取发生错误 参数err的值为错误对象否则err的值为null
    // doc参数为文件内容
    if (err == null) {
       console.log(doc);
    }
})
```

写入文件内容

```javascript
fs.writeFile('文件路径/文件名称','数据', callback);
```

```javascript
const content = '<h3>正在使用fs.writeFile写入文件内容</h3>';
fs.writeFile('../index.html', content, err => {
    if (err != null) {
        console.log(err);
        return;
    }
    console.log('文件写入成功');
});
```



系统模块path路径操作

为什么要进行路径拼接?

- 不同操作系统的路径分隔符不统一
- /public/uploads/avatar
- windows 是\ 
- linux 是 /

路径拼接语法

```javascript
path.join('路径','路径', ...);
```

```javascript
// 导入path模块
const path = require('path');
// 路径拼接
let finialPath = path.join('itcast', 'a', 'b', 'c.css');
// 输出结果为 itcast\a\b\c.css
console.log(finialPath);
```

相对路径VS绝对路径

- 大多数情况下使用绝对路径，因为相对路径有时候相对的是 命令行工具的当前工作目录
- 在读取文件或者设置文件路径时都会选择绝对路径
- 使用__dirname获取当前文件所在的绝对路径 



什么是第三方模块?

别人写好的、具有特定功能的、我们可以直接使用的模块即第三方，由于第三方模块通常都是由多个文件组成并且被放置在一个文件夹中，所以又名包。

第三方模块有两种存在形式：

- 以js文件的形式存在，提供实现项目具体功能的API接口。
- 以命令行工具形式存在，辅助项目开发

获取第三方模块

npmjs.com: 第三方模块的储存和分发仓库

npm（node package manager）：node的第三方，模块管理工具

- 下载：npm install 模块名称
- 卸载：npm uninstall package 模块名称

全局安装与本地安装

- 命令行工具：全局安装
- 库文件：本地安装

第三方模块 nodemon

nodemon是一个命令行工具，用以辅助项目开发。

在node.js中，每次修改文件都要在命令行工具中重新执行该文件，非常繁琐。

使用步骤：

1. 使用npm install nodemon -g 下载它
2. 在命令行工具中用nodemon命令代替node命令执行文件

g - 全局安装

第三方模块 nrm

nrm（npm registry manager）: npm下载地址切换工具

npm默认的下载地址在国外，国内下载速度慢

使用步骤

1. 使用npm install nrm -g 下载它
2. 查询可用下载地址列表 nrm ls
3. 切换npm下载地址 nrm use 下载地址名称

# Gulp

第三方模块 Gulp

基于node平台开发的前端构建工具

将机械化操作编写完成任务，想要执行机械化操作时执行一个命令行命令任务就能自动执行了

用机器代替手工，提高开发效率。

Gulp能做什么？

- 项目上线，HTML、CSS、JS文件压缩合并
- 语法转换（es6、less...）
- 公共文件抽离
- 修改文件浏览器自动刷新

Gulp使用

1. 使用 npm install gulp 下载gulp库文件
2. 在项目根目录下建立gulpfile.js文件
3. 重构项目的文件夹结构 src 目录放置源代码文件 dist 目录放置构建后文件
4. 在gulpfile.js 文件中编写任务
5. 在命令行文件中执行gulp任务

Gulp中提供的方法

- gulp.src() : 获取任务要处理的文件
- gulp.dest() : 输出文件
- gulp.task() : 建立gulp任务
- gulp.watch() : 监控文件的变化

```javascript
const gulp = require('gulp');
// 使用gulp.task()方法建立任务
// 1.任务名称
// 2.任务的回调函数
gulp.task('first' , () => {
    // 获取要处理的文件
    gulp.src('./src/css/base.css')
    // 将处理后的文件输出到dist目录
    .pipe(gulp.dest('./dist/css'));
})
```

Gulp插件

- gulp-htmlmin：htm文件压缩
- gulp-css：压缩css
- gulp-babel：JavaScript语法转化
- gulp-less：less语法转化
- gulp-uglify：压缩混淆JavaScript
- gulp-file-include公共文件包含
- browsersync浏览器实时同步







# 路由

http://localhost:3000/index

http://localhost:3000/login

路由是指客户端请求地址与服务器端程序代码的对应关系。简单说，就是请求什么响应什么。

```javascript
// 当客户端发来请求的时候
app.on('request', (req,res) => {
    // 获取客户端的请求路径
    let {pathname} = url.parse(req.url);
    if(pathname == '/' || pathname == '/index') {
        res.end('欢迎来到首页');
    } else if (pathname == '/list') {
        res.end('欢迎来到列表页面');
    }else {
        res.end('抱歉，你访问的页面出错了');
    }
});
```

# 静态资源

服务器端不需要处理，可以直接响应给客户端的资源就是静态资源，例如CSS、JavaScript、image文件。

http://www.itcast.cn/images/logo.png

# 动态资源

相同的请求地址不同的响应资源，这种资源就是动态资源。

http://www.itcast.cn/article?id=1

http://www.itcast.cn/article?id=2

# 同步API，异步API

```javascript
// 路径拼接
const public = path.join(__dirname, 'public');
// 请求地址解析
const urlObj = url.parse(req.url);
// 读取文件
fs.readFile('./demo.text', 'utf8', (err, result) => {
    console.log(result);
})
```

同步API：只有当钱API执行完成后，才能继续执行下一个API

```javascript
console.log('before');
console.log('after');
```

异步API：当前API的执行不会阻塞后续代码的执行

```javascript
console.log('before');
setTimeout(
    () => { console.log('last');
    },2000);
console.log('after');
```

同步API，异步API的区别（获取返回值）

同步API可以从返回值中拿到API执行的结果，但是异步API是不可以的

```javascript
// 同步
function sum (n1, n2) {
    return n1 + n2;
}
const result = sum (10, 20);
```

```javascript
// 异步
function getMsg() {
	setTimeout(function () {
        return { msg: 'Hello Node.js' }
    }, 2000);
}
const msg = getMsg ();
```













































































































先装一下Typora和git

注册一个GitHub账号

新建一个 New repository  （new-notes）

本地创建一个公钥

在设置里面找到SSH and GPG keys

在终端新建一个key

命令：ssh-keygen -o

找到 id_rsa.pub 复制里面的ssh加到GitHub上的SSH keys

进到new-notes仓库 复制SSH git@github.com:xinxian1/new-notes.git

然后去终端 

git clone git@github.com:xinxian1/new-notes.git

cd new-notes

git status

然后创建需要的笔记

上传到远端 git status

增加所有刚刚新增的文件 git add .

git commit -m "add all"

然后可能会出现错误 把邮箱和名字存一下就可以了

Author identity unknown                 作者身份不明

*** Please tell me who you are.          请告诉我你是谁。

Run

  git config --global user.email "you@example.com" 
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: unable to auto-detect email address (got '0VJ.(none)')

出现这个就输入一下就行

git config --global user.email "123423213@qq.com"

git config --global user.name "xinxian1"

重新输入一次  git commit -m "add all"

git push

就传上去了