# Node.js核心模块——Buffer

JavaScript语言本身对二进制支持比较欠缺，但在处理类似TCP流或者文件流这样的流数据时，必须使用到二进制，为此，Node.js定义了Buffer类，专门处理原始内存数据。Buffer是代表原始堆分配额的数据类型，在Node.js中以类似数组的方式来使用，全局可用，不需要require。

## 创建Buffer
> 在Node.js v6之前的版本中，使用构造函数`new Buffer()`的方式来创建Buffer实例，因为`new Buffer()`的行为会根据所传入的第一个参数的值而明显改变，所以如果应用程序没有正确地校验传入的参数、或未能正确地初始化新分配的Buffer内容，就有可能在无意中引入安全性和可靠性问题。

所以为创建更可靠、更安全的Buffer实例，v6之后，官方建议使用[`Buffer.from()`、`Buffer.alloc()`和`Buffer.allocUnsafe()`](https://nodejs.org/dist/latest-v8.x/docs/api/buffer.html#buffer_buffer_from_buffer_alloc_and_buffer_allocunsafe)创建Buffer实例。
```javascript
// Buffer.alloc(size[, fill[, encoding]])
// 创建长度为3，且默认用0填充的Buffer
const buf1 = Buffer.alloc(3);
// 创建长度为3，且用1填充的Buffer
const buf2 = Buffer.alloc(3, 1);

// Buffer.allocUnsafe(size)
// 创建一个长度为10，且未初始化的Buffer
// allocUnsafe比alloc方法快
// 但返回的Buffer可能包含旧数据，因此需用fill()或write()重写
const buf3 = Buffer.allocUnsafe(10);

// Buffer.from()
// 创建[0x1, 0x2, 0x3]的Buffer
const buf4 = Buffer.from([1, 2, 3]);
```

## Buffer转为其它格式
`Buffer.toString()`方法，Node.js目前支持的字符编码包括：
* 'ascii' - 仅支持7位ASCII数据。如果设置去掉高位的话，这种编码是非常快的 
* 'utf8' - 多字节编码的Unicode字符。许多网页和其它文档格式都使用UTF-8 
* 'utf16le' - 2或4个字节，小字节序编码的Unicode字符，支持代理对（U+10000至U+10FFFF） 
* 'ucs2' - utf16le的别名 
* 'base64' - Base64编码 
* 'latin1' - 一种把Buffer编码成一字节编码的字符串的方式 
* 'binary' - 'latin1'的别名 
* 'hex' - 将每个字节编码为两个十六进制字符

## 使用Buffer修改字符串编码
### 创建基本验证信息的头部
```javascript
const user = 'JiangBao';
const password = '123456';
const encoded = Buffer.from(`${user}:${password}`).toString('base64');  // SmlhbmdCYW86MTIzNDU2

const decoded = Buffer.from(encoded, 'base64').toString();  // JiangBao:123456
```

### data URIs
Data URIs允许一个资源以行内编码的形式存在于web页面中，遵循以下格式：  
`data:[MIME-type][;charset=<encoding>[;base64]],<data>`  
1. 输出data URI：
  ```javascript
  const fs = require('fs');
  const mime = 'image/jpeg';
  const encoding = 'base64';
  const data = fs.readFileSync('./icon.jpeg').toString(encoding);

  // data:image/jpeg;base64,/9j/2wCEAAgGBgcGBQgHBwcJCQgKDBQNDA...
  const uri = `data:${mime};${encoding},${data}`;
  ```
2. 解析data URI：
  ```javascript
  const fs = require('fs');
  const uri = 'data:image/jpeg;base64,/9j/2wCEAAgGBgcGBQgHBwcJCQgKDBQNDA...';
  const imageData = uri.split(',')[1];
  const buf = Buffer.from(imageData, 'base64');
  fs.writeFileSync('./icon.bak.jpeg', buf);
  ```

## 创建自定义二进制协议
TODO

