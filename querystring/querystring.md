# querystring
> querystring 模块提供用于解析和格式化 URL 查询字符串的实用工具  

querystring用于处理url中的查询字符串。  
查询字符串指：url字符串中，从问号`?`(不包括`?`)开始到锚点`#`或者到url字符串的结束的部分叫做查询字符串。  

## parse
> querystring.parse(str[, sep[, eq[, options]]])

* str 查询字符串  
* sep 分割键值对的字符，默认为'&'  
* sep 分割键和值的字符，默认为'='  
* options  
  + decodeURIComponent
  + maxKeys


``` javascript
querystring.parse('name=gary&age=18&gender=male');
```
再看一个自定义分割符的例子
``` javascript
querystring.parse('name+gary-age+18-gender+male', '-', '+');
```
这个跟上一个例子一样，都输出以下对象
``` javascript
{
  name: 'gary',
  age: '18',
  male: 'gender'
}
```

## stringify
> querystring.stringify(obj[, sep[, eq[, options]]])

* obj 要序列化为查询字符串的对象
* sep 分割键值对的字符，默认为'&'
* eq 分割键和值的字符，默认为'='
* options
  + encodeURIComponent

``` javascript
const str = querystring.stringify({
  name: 'gary',
  age: 18
});
// name=gary&age=18
```

## escape
> querystring.escape(str)  

对给定的str执行url百分比编码

``` javascript
querystring.escape('github.com=123'); // github.com%3D123
querystring.escape('你好'); // %E4%BD%A0%E5%A5%BD
```

## unescape
> querystring.unescape(str)  

对给定的str上执行url百分比编码字符的解码

``` javascript
querystring.unescape('github.com%3D123'); // github.com=123
querystring.unescape('%E4%BD%A0%E5%A5%BD'); // 你好
```

## decode
> querystring.decode()  

`querystring.decode()`函数是`querystring.parse()`的别名

## encode
> querystring.encode()  

`querystring.encode()`函数是`querystring.stringify()`的别名