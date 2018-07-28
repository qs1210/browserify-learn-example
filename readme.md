# 一个简单的探索browserify实现 commonJs 的方式

## 首先安装一下npm包

```javascript
//browser 按照
npm install -g browserify

//browser-unpack 安装, 用来分析编辑后的文件
npm install -g browser-unpack
```

新建两个文件

1. main.js

```javascript
const foo = require('./foo.js');
foo('hi, browserify');
```

2. foo.js

```javascript
module.exports = function(x) {
    console.log(x);
}
```

执行命令

$ browserify main.js > compile.js
$ browser-unpack < compile.js >unpack.js

查看其中的unpack文件

```javascipt
[
{"id":1,"source":"module.exports = function(x) {\n    console.log(x);\n}","deps":{}}
,
{"id":2,"source":"const foo = require('./foo.js');\nfoo('hi');","deps":{"./foo.js":1},"entry":true}
]
```

不过我更关心的是前边的 commonjs 的实现方式, 在文件 (compile-beauti.js)[./compile-beauti.js] 中, 这个是我用
(jsbeautifier)[http://jsbeautifier.org/] 进行处理的结果.

通过页面的debug, 可以查看一下实现方式. 感觉很不错

管家你的地方就是, 按照数组中的依赖栈进行依次执行, require 的就是上一个依赖 module.exports 出去的. 这一点理解了很重要.


