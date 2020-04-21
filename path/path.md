# path
首先看一下显示文件路径的几种方式。

``` javascript
console.log(__dirname); // /Users/zzh/test/my-node
console.log(__filename); // /Users/zzh/test/my-node/path.js
console.log(process.cwd()); // /Users/zzh/test/my-node
```

`__dirname` 显示文件所在文件夹的位置
`__dirname` 显示文件所在位置
`process.cwd()` 运行node命令所在文件夹的位置。假如我在上一层执行这个命令，上面的例子输出则变为 `/Users/zzh/test` 。

## dirname

> path.dirname(path)

`path.dirname()` 方法返回path的目录名。

``` javascript
const testPath = '/Hello/World/node.js';
const str = path.dirname(testPath);
console.log(str); // /Hello/World
```

## basename

> path.basename(path[, ext])

`path.basename()` 方法返回path的最后一部分。

``` javascript
const testPath = '/Hello/World/node.js';
const file = path.basename(testPath);
const file2 = path.basename(testPath, '.js');
console.log(file); // node.js
console.log(file2); // node
```

## extname

> path.extname(path)

`path.extname()` 方法返回path的扩展名，从最后一次出现 `.` （句点）字符到path最后一部分的字符串结束。参考几种情况
``` javascript
path.extname('index.html'); // '.html'

path.extname('index.coffee.md'); // '.md'

path.extname('index.'); // '.'

path.extname('index'); // ''

path.extname('.index'); // ''

path.extname('.index.md'); // '.md'
```
如果path以`.`为结尾，将返回`.`，如果无扩展名又不以`.`结尾，将返回空字符串。

## format
> path.format(pathObject)

`path.format()`方法从对象返回路径字符串。与 `path.parse()`相反。
``` javascript
const pathObj = {
    root: "/",
    dir: "/Hello/World/Node",
    base: "node.js",
    ext: ".js",
    name: "node",
};
const res = path.format(pathObj);
console.log(res); // /Hello/World/Node/node.js
```

题外：让我想起了之前学的`querystring`的`parse`和`stringify`。

## parse
> path.parse(path)

`path.parse()`方法返回一个对象，其属性表示 path 的重要元素。
返回的对象有以下属性。
* dir <string> 文件所在文件夹
* root <string> 
* base <string> 文件名+扩展名
* name <string> 文件名
* ext <string> 文件扩展名
``` javascript
const testPath = '/Hello/World/Node/node.js';
const obj = path.parse(testPath);
console.log(obj);
```
``` javascript
{ root: '/',
  dir: '/Hello/World/Node',
  base: 'node.js',
  ext: '.js',
  name: 'node' }
```

## normalize
> path.normalize(path)

path.normalize() 方法规范化给定的 path，解析 '..' 和 '.' 片段。
``` javascript
const testPath = '/Hello/World/Node/..';
const testPath2 = '//Hello/./World/Node/../Node2/node.js';

const res = path.normalize(testPath);
const res2 = path.normalize(testPath2);
console.log(res); // /Hello/World
console.log(res2); // /Hello/World/Node2/node.js
```

## join
> path.join([...paths])

`path.join()`方法使用平台特定的分隔符作为定界符将所有给定的 path 片段连接在一起，然后规范化生成的路径。

``` javascript
const res = path.join('/Hello/World', './Node', '../Node2', 'node.js');
const res2 = path.normalize('/Hello/World/./Node/../Node2/node.js');
console.log(res); // /Hello/World/Node2/node.js
console.log(res2); // /Hello/World/Node2/node.js
```
可以看到，上面两个结果是一样的。就是说join会对接收到的字符串序列进行拼接，然后再用`normalize`规范化。

## resolve
> path.resolve([...paths])

`path.resolve()`方法将路径或路径片段的序列解析为绝对路径。

``` javascript
const res = path.resolve('/Hello/World', '/Hello2/World2', '../World22', 'node.js');
console.log(res); // /Hello2/World22/node.js
```
我们可以将`resolve`理解为我们平时的cd操作。以上步骤可解释为：
1. 进入/Hello/World
2. 切换成了/Hello2/World2
3. 从World2返回上一级后进入World22
4. 找到World22下的node.js文件

## relative
> path.relative(from, to)  

`path.relative()`方法根据当前工作目录返回from到to的<b>相对路径</b>
``` javascript
const testPath = '/Hello/World/node.js';
const testPath2 = '/Hello/World2/Node/es.js';

const relativePath = path.relative(testPath, testPath2);
console.log(relativePath); // ../../World2/Node/es.js
```

更多详情，看官方文档啦。


