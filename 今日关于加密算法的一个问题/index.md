<div align=center><img width=568 height=168 src="https://jiangbao-1258001083.cos.ap-shanghai.myqcloud.com/encrypted-file-recovery.png" /></div>

今天在接入一个 SDK 时，对方加密算发使用 `AES/ECB/PKCS5Padding`，不指定 `iv`，这边使用 Node.js 的后端服务器，结果遇到了加密结果与 java 不一致的问题，主要还是自己没仔细翻看官方文档的问题，记录一下。

刚开始使用 `crypto` 模块 `createCipher` 函数来实现加解密逻辑，由于这么使用会默认不补全填充量，所以加密结果肯定和对方不一致，后来还是再仔细看了官方文档，发现在目前 LTS 版本(12.16.1)中，`crypto.createCipher` 函数已被[废弃](https://nodejs.org/dist/latest-v12.x/docs/api/crypto.html#crypto_crypto_createcipher_algorithm_password_options) 🤯🤯🤯，
原来自己需要的是 `crypto.createCipheriv(algorithm, key, iv[, options])` 函数：
> Creates and returns a Cipher object, with the given algorithm, key and initialization vector (iv).

然后轻松解决加密问题，示例代码如下：
```js
const crypto = require('crypto');

/**
 * AES/ECB/PKCS5Padding 加密
 * @param {string}  data  待加密字符串
 * @param {string}  key   秘钥
 * @return {object}
 */
const encrypt = (data, key) => {
  const iv = "";
  const chunk = [];
  const cipher = crypto.createCipheriv('aes-128-ecb', key, iv);
  cipher.setAutoPadding(true);

  chunk.push(cipher.update(data, 'utf8', 'base64'));
  chunk.push(cipher.final('base64'));

  return chunk.join('');
};

/**
 * AES/ECB/PKCS5Padding 解密
 * @param {string}  data  待解密字符串base64
 * @param {string}  key   秘钥
 * @return {object}
 */
const decrypt = (data, key) => {
  const iv = "";
  const chunk = [];
  const decipher = crypto.createDecipheriv('aes-128-ecb', key, iv);
  decipher.setAutoPadding(true);

  chunk.push(decipher.update(data, 'base64', 'utf8'));
  chunk.push(decipher.final('utf8'));

  return chunk.join('');
};
```
官方文档还是要常看常新啊，对于 Node.js 这样的版本帝，还是要经常把脑子里那点陈旧的文档更新一下，看了一下本地开发的 node 版本，生产环境用的 `v8.9.4`，个人用的 `v10.1.0`，目前最新版本已经是 `13.9.0`，LTS版本是 `12.16.1`。看来之前断更的「每日一个 Node.js 模块」系列还是有必要重新开始了。

