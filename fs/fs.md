# fs

## 文件知识

### 权限位 mode
读 4
写 2
执行 1
没有权限 0

输入以下命令，查看文件信息。

```
ls -al
```
选取输出的一些例子
```
-rw-r--r--@  1 zzh  staff  524  5  1 10:52 readme.md
drwxr-xr-x   2 zzh  staff   64  4  6 14:01 stream
```
最重要的信息是开头的这些。
`d`开头表示文件夹，` -`开头表示文件，后面9位分别表示当前用户、用户所属组、其他用户，3位一组。`r`读，` w`写，` x`执行， `-`没有对应的权限。

### 标识符 flag
表示对文件的操作方式
r 读取
w 写入
s 同步
a 追加
[文件系统标识](http://nodejs.cn/api/fs.html#fs_file_system_flags)


### 文件描述符 fd
Node.js抽象了各操作系统之间差异，为每个打开的文件分配一个数值型符号，用于追踪指定的文件。



## 文件操作

### readFile
> fs.readFile(path[, options], callback)  

* path <string> | <Buffer> | <URL> | <integer> 文件名或文件描述符。
* options <Object> | <string>
  + encoding <string> | <null> 默认值: null。
  + flag <string> 参见文件系统 flag 的支持。默认值: 'r'。
* callback <Function>
  + err <Error>
  + data <string> | <Buffer>

异步读取文件。如果指定encoding，data是一个解析后的字符串，否则将会以Buffer形式表示的二进制数据。
``` javascript
const fs = require('fs');
const path = require('path');
const filename = path.resolve(__dirname, './hello.txt');
fs.readFile(filename,(err, data) => {
  if(err){
    console.error(err);
  }else{
    console.log(data);
  }
});
// <Buffer 68 65 6c 6c 6f 20 77 6f 72 6c 64>
```

### readFileSync
> fs.readFileSync(path[, options]) 

同步读取文件。参数值与上面的异步读取是一样的。如果指定了 encoding 选项，则此函数返回字符串。 否则，返回 buffer。
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");
try {
    const str = fs.readFileSync(filename, {
        encoding: "utf-8",
    });
    console.log(str); // hello world
} catch (e) {
    console.error(e);
}

```

### writeFile
> fs.writeFile(file, data[, options], callback)  

* file <string> | <Buffer> | <URL> | <integer> 文件名或文件描述符。
* data <string> | <Buffer> | <TypedArray> | <DataView>
* options <Object> | <string>
  + encoding <string> | <null> 默认值: 'utf8'。
  + mode <integer> 默认值: 0o666。
  + flag <string> 默认值: 'w'。
* callback <Function>
  + err <Error>

异步写文件。file参数会区分是一个文件名还是文件描述符，如果是文件名，异步写入数据到文件，若文件已存在，覆盖文件内容。
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");
fs.writeFile(filename, 'goodbye', (err)=> {
  if(err){
    console.error(err)
  }else{
    console.log('写入成功')
  }
})
```
### writeFileSync
> fs.writeFileSync(file, data[, options])  

同步写文件。参数与上面的异步写入一样。返回undefined。
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");
try{
  fs.writeFileSync(filename, 'nice');
}catch(e){
  console.error(e)
}

```

当我们设置`flag`为`a`时，可以添加内容，而不会一执行就覆盖旧内容。
``` javascript
try{
  fs.writeFileSync(filename, '123',{flag: 'a'});
}catch(e){
  console.error(e)
}
```
异步同理，不举例了。

### appendFile
> fs.appendFile(path, data[, options], callback)  

* path <string> | <Buffer> | <URL> | <number> 文件名或文件描述符。
* data <string> | <Buffer>
* options <Object> | <string>
  + encoding <string> | <null> 默认值: 'utf8'。
  + mode <integer> 默认值: 0o666。
  + flag <string> 默认值: 'a'。
* callback <Function>
  + err <Error>

异步追加文件内容。异步地将数据追加到文件，如果文件尚不存在则创建该文件。
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");
fs.appendFile(filename, 'love', (err) => {
  if(err){
    console.error(err)
  }else{
    console.log('添加成功')
  }
})
```

### appendFileSync
> fs.appendFileSync(path, data[, options])  

同步追加内容。参数与上面的异步追加一致。
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");
try{
  fs.appendFileSync(filename, ' you');
}catch(e){
  console.error(e);
}
```

### unlink
> fs.unlink(path, callback)  

* path <string> | <Buffer> | <URL>
* callback <Function>
  + err <Error>

异步删除文件。
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello1.txt");
fs.unlink(filename, (err) => {
    if (err) {
        console.error(err);
    } else {
        console.log("删除成功");
    }
});
```

### unlinkSync
> fs.unlinkSync(path)

同步删除文件。参数与上面的异步删除一致
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello1.txt");
try{
  fs.unlinkSync(filename);
}catch(e){
  console.error(e);
}
```

### open
> fs.open(path[, flags[, mode]], callback)  

* path <string> | <Buffer> | <URL>
* flags <string> | <number> 参见文件系统 flag 的支持。默认值: 'r'。
* mode <string> | <integer> 默认值: 0o666（可读写）。
* callback <Function>
  + err <Error>
  + fd <integer>

异步打开文件。这里我们终于看到之前提到的文件描述符fd了。通常fs.open和fs.write、fs.read结合使用。
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");
fs.open(filename, (err, fd) => {
    if (err) {
        console.error(err);
    } else {
        console.log(fd);
    }
});
```

### openSync
> fs.openSync(path[, flags, mode])  

同步打开文件。参数与上面的异步打开一致。返回文件描述符
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");
try{
  const fd = fs.openSync(filename);
  console.log(fd);
}catch(e){
  console.error(e);
}
```

### read
> fs.read(fd, buffer, offset, length, position, callback)  

* fd <integer> 文件描述符
* buffer <Buffer> | <TypedArray> | <DataView> 数据写入的缓冲区
* offset <integer> buffer中开始写入的偏移量
* length <integer> 要读取的字节数
* position <integer> 指定文件中开始读取的位置
* callback <Function>
  + err <Error>
  + bytesRead <integer>
  + buffer <Buffer>
也就是offset是控制写入buffer的位置，length和position是控制读取文件的内容

异步读取文件。

我们把hello.txt文件的内容改为`hello对不起`。
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");
fs.open(filename, "r", (err, fd) => {
    let buffer = Buffer.alloc(255);
    fs.read(fd, buffer, 0, 8, 0, (err, bytesRead, buffer) => {
        if (err) {
            console.error(err);
        } else {
            console.log(bytesRead);
            console.log(buffer.toString());
            fs.read(fd, buffer, 8, 6, 8, (err2, bytesRead2, buffer2) => {
                if (err2) {
                    console.error(err2);
                } else {
                    console.log(bytesRead2);
                    console.log(buffer2.toString());
                }
            });
        }
    });
});
// 8
// hello对
// 6
// hello对不起
```

### readSync
> fs.readSync(fd, buffer, offset, length, position)

同步读取文件。参数与上面的read一致，返回byteRead数量。
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");

try{
  const fd = fs.openSync(filename);
  try{
    let buffer = Buffer.alloc(255);
    const byteRead = fs.readSync(fd, buffer, 0, 8, 0);
    console.log(byteRead);
    console.log(buffer.toString());
    try{
      const byteRead2 = fs.readSync(fd, buffer, byteRead, 6, byteRead);
      console.log(byteRead2);
      console.log(buffer.toString());
    }catch(e){
      console.error(e);
    }
  }catch(e){
    console.error(e)
  }
}catch(e){
  console.error(e);
}

// 8
// hello对
// 6
// hello对不起
```

### write
> fs.write(fd, buffer[, offset[, length[, position]]], callback)  

* fd <integer>
* buffer <Buffer> | <TypedArray> | <DataView>
* offset <integer>
* length <integer>
* position <integer>
* callback <Function>
  + err <Error>
  + bytesWritten <integer>
  + buffer <Buffer> | <TypedArray> | <DataView>

异步写文件。将buffer写入到指定的文件。

``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");

fs.open(filename, 'w', (err, fd) => {
  let buffer = Buffer.from('98765');
  fs.write(fd, buffer, 0, 5, 0, (err, byteRead, buffer) => {
    if(err){
      console.error(err);
    }else{
      console.log(byteRead);
    }
  })
})
```
hello.txt的内容被改为98765

### writeSync
> fs.writeSync(fd, buffer[, offset[, length[, position]]])  

同步写文件。参数与上面的write一致，返回写入的字节数。
``` javascript
const fs = require("fs");
const path = require("path");
const filename = path.resolve(__dirname, "./hello.txt");

fs.open(filename, "w", (err, fd) => {
    try {
        let buffer = Buffer.from("23456");
        const written = fs.writeSync(fd, buffer, 0, 5, 0);
        console.log(written);
    } catch (e) {
        console.error(e);
    }
});

```

### close
> fs.close(fd, callback) 

* fd <integer>
* callback <Function>
  + err <Error>

异步关闭文件。与open对应的方法，打开了就要关闭。

### closeSync
> fs.closeSync(fd)

同步关闭文件。与上面的close参数一致。返回undefined。

### rename
> fs.rename(oldPath, newPath, callback)

* oldPath <string> | <Buffer> | <URL>
* newPath <string> | <Buffer> | <URL>
* callback <Function>
  + err <Error>

``` javascript
const fs = require("fs");

fs.rename('./hello.txt', './src/hello2.txt', (err) => {
  if(err){
    console.error(err)
  }
})
```
rename除了可以重命名文件名，还能够移动文件。

### renameSync
> fs.renameSync(oldPath, newPath)

参数与上面的rename一致。返回undefined。

## 文件夹操作

### mkdir

> fs.mkdir(path[, options], callback)  

* path <string> | <Buffer> | <URL>
* options <Object> | <integer>
  + recursive <boolean> 默认值: false。
  + mode <string> | <integer> Windows 上不支持。默认值: 0o777。
* callback <Function>
  + err <Error>

异步创建文件夹。

``` javascript
const fs = require("fs");
const path = require("path");
const dirname = path.resolve(__dirname, "output");
const dirname2 = path.resolve(__dirname, "output/ouput2/ouput3");
fs.mkdir(dirname2, {recursive: true}, (err) => {
    if (err) {
        console.error(err);
    }
});
```
options中`recursive`是什么意思呢。通俗点就是我们打算目录下再有一个目录，设置为true之后，即使目录下没有这些目录也不会报错,就是一个可以创建多级目录的方式。
如果相同路径下再创建一个同名目录，会报错。

### mkdirSync
> fs.mkdirSync(path[, options]) 

同步创建文件夹。参数与上面的mkdir一致。同步地创建目录。 返回 undefined，或者如果 recursive 为 true，则返回创建的第一个文件夹的路径。
``` javascript
const fs = require("fs");
const path = require("path");
const dirname = path.resolve(__dirname, "output");
const dirname2 = path.resolve(__dirname, "output/ouput2/ouput3");
try {
    let str = fs.mkdirSync(dirname2, { recursive: true });
    console.log(str);
} catch (e) {
    console.error(e);
}
```

### rmdir
> fs.rmdir(path[, options], callback) 

异步删除目录。同样options中有个参数recursive可以递归删除。
``` javascript
const fs = require("fs");
const path = require("path");
const dirname = path.resolve(__dirname, "output");
fs.rmdir(dirname, (err) => {
    if (err) {
        console.error(err);
    }
});
```

### rmdirSync
> fs.rmdirSync(path[, options])

同步删除文件夹。参数与上面的rmdir一致。

### readdir
> fs.readdir(path[, options], callback)

* path <string> | <Buffer> | <URL>
* options <string> | <Object>
  + encoding <string> 默认值: 'utf8'。
  + withFileTypes <boolean> 默认值: false。
* callback <Function>
  + err <Error>
  + files <string[]> | <Buffer[]> | <fs.Dirent[]>

异步读取文件夹内容。files 是目录中的文件名的数组（不包括 '.' 和 '..'）
``` javascript
const fs = require("fs");
const path = require("path");
const dirname = path.resolve(__dirname, "src");
fs.readdir(dirname, (err, files) => {
  if(err){
    console.error(err)
  }else{
    console.log(files);
  }
})
// [ 'app.js', 'utils' ]
```
我的src目录下有app.js和utils目录。

### readdirSync
> fs.readdirSync(path[, options])

同步读取文件夹内容。参数与上面的readdir一致。