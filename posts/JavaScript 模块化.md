JavaScript 模块化从早起刀耕火种的时代，匿名执行函数， 进化到了 Node.js 的 CommonJS  ，浏览器端的 AMD，和同时兼容的 AMD 和 CommonJS 的 UMD 规范，以及使用第三方加载器 Browserify
和 webpack 时代，最终到了 ES2015 的 import 和 export 方案，下面已实例讲解各个规范的具体示例代码


#### CommonJS

Node程序由许多个模块组成，每个模块就是一个文件。Node模块采用了CommonJS规范。 
CommonJS规定，每个文件的对外接口是 module.exports 对象。这个对象的所有属性和方法，都可以被其他文件导入。

exports 单个对象

hello.js

```
function greet(name) {
  console.log(hello + ', ' + name + '!'); 
}
module.exports = hello;
```

App.js

内置的require命令用于加载模块文件。 
require命令的基本功能是，读入并执行一个JavaScript文件，然后返回该模块的exports对象。如果没有发现指定模块，会报错。

```
var greet = require('./hello');
greet('CommonJS');

```

export 多个对象

hello.js

```
function greet(name) {
  console.log('hello' + ', ' + name + '!');
}
function hello() {
  console.log('Hello, world!');
}
module.exports = {
  hello: hello,
  greet: greet
};
```

app.js

```
var hello = require('./hello');
hello.greet('CommonJS');
hello.hello()
```


