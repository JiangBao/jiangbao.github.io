# Node.js核心模块—Crypto

`crypto`是Node.js核心模块之一，主要用于各类加密场景。使用C/C++实现各类加密算法，然后暴露为javascript接口，包括对OpenSSL的哈希、HMAC、加密、解密、签名以及验证功能的一整套封装。我个人使用最多的场景是各类第三方SDK接入时接口数据的签名认证，使用起来简单方便。

如果对于加密相关的安全基础知识不了解，可以查看[这篇文章](https://www.cnblogs.com/chyingp/p/nodejs-learning-crypto-theory.html)。

## 哈希
MD5是最常用的哈希算法，但是存在一定的碰撞问题，所以可以使用`sha1`、`sha256`等，使用crypto模块进行哈希的时候，使用Hash类，主要有三个步骤：
* `crypto.createHash(algorithm)`  
  创建并返回hash实例，用于生成哈希摘要，`algorithm`用于指定算法类型，比如：`md5`、`sha1`
* `hash.update(data)`  
  使用data更新哈希内容，默认UTF-8编码，流式传输时，可多次调用此方法
* `hash.digest(encoding='binary')`  
  计算传入数据(调用hash.update())的摘要，按照指定encoding返回，默认是Buffer  
  调用此方法后，hash实例已被重置，再次调用会报错

以MD5签名为例：
```js
const crypto = require('crypto);

const hash = crypto.createHash('md5');

hash.update('Hello,');
hash.update('world');

hash.digest('hex');   // 7add895cc0518859d8cc85da0ff4756b
```

## Hmac
Hmac全称`keyed-Hash Message Authentication Code`，Hmac也属于一种哈希算法，不过需要多一个秘钥输入，可以有效防止类似MD5的彩虹表等攻击  
使用Hmac类生成实例，其它`update`和`digest`与hash对象类似
```js
const crypto = require('crypto');

const hmac = crypto.createHmac('sha256', 'this-is-your-secret-key');

hmac.update('Hello,');
hmac.update('world');

hmac.digest('hex');    // 7054a95555e11a1721730e6b04295915b83871c6e090462fcd8c25af7d7afc77
```

## 对称加密-AES
通信一方使用秘钥加密原始数据，接收方收到数据后，使用**同一秘钥**进行解密，即为对称加密。AES是我在业务中比较常用的对称加密算法
> **踩坑更新：** `crypto.createCipher`方法已于v10.x版本之后废弃，必须使用`crypto.createCipheriv`方法。否则第三方渠道使用java做aes加密逻辑时，会造成加解密结果不一致的问题！！！
```js
const crypto = require('crypto');

function aesEncrypt(data, key) {
    const cipher = crypto.createCipher('aes192', key);
    let crypted = cipher.update(data, 'utf8', 'hex');
    crypted += cipher.final('hex');
    return crypted;
}

function aesDecrypt(encrypted, key) {
    const decipher = crypto.createDecipher('aes192', key);
    let decrypted = decipher.update(encrypted, 'hex', 'utf8');
    decrypted += decipher.final('utf8');
    return decrypted;
}

const data = 'Hello, World!';
const key = 'Password!';

const encrypted = aesEncrypt(data, key);        //98b549b7cf97859d11b814a1bff4eecb
const decrypted = aesDecrypt(encrypted, key);   //Hello, World!
```

## 非对称加密-RSA
非对称加密指双方使用不同的秘钥进行加密和解密，通信双方都会持有公钥和私钥，假设有两组公私钥，`a-prv.pem`、`a-pub.pem`和`b-prv.pem`、`b-pub.pem`，使用其中一把秘钥加密的密文，只能使用对应的另一把秘钥才能解密，比如，使用了`a-prv.pem`加密，则必须使用`a-pub.pem`才能解密。RSA是比较常用的非对称加密算法，主要使用几个方法：
```js
// 私钥加密
crypto.privateEncrypt(privateKey, buffer)
// 公钥解密
crypto.publicDecrypt(publicKey, buffer)

// 公钥加密
crypto.publicEncrypt(publicKey, buffer)
// 私钥解密
crypto.privateDecrypt(privateKey, buffer)
```

> [https://cnodejs.org/topic/504061d7fef591855112bab5](https://cnodejs.org/topic/504061d7fef591855112bab5)
> [https://www.liaoxuefeng.com/wiki/1022910821149312/1023025778520640](https://www.liaoxuefeng.com/wiki/1022910821149312/1023025778520640)
