# 在 NodeJS 中加密和解密

> 原文：<https://medium.com/geekculture/encrypt-and-decrypt-in-nodejs-11c5d9cb81f?source=collection_archive---------13----------------------->

# 如何加密文本

创建一个名为 **encdec.js** 的文件并粘贴:

```
const crypto = require("crypto")const encrypt = (plainText, password) => {
  try {
    const iv = crypto.randomBytes(16);
    const key = crypto.createHash('sha256').update(password).digest('base64').substr(0, 32);
    const cipher = crypto.createCipheriv('aes-256-cbc', key, iv); let encrypted = cipher.update(plainText);
    encrypted = Buffer.concat([encrypted, cipher.final()])
    return iv.toString('hex') + ':' + encrypted.toString('hex'); } catch (error) {
    console.log(error);
  }
}
```

让我们来测试一下:

追加这些行:

```
const encrypt = (plainText, password) => {
  ...
}const text = "Hello World"
const pass = "secret1234"const encText = encrypt(text, pass)
console.log('encrypted text', encText);
```

然后运行:

```
node encdec.js# Output:
encrypted text af9efafd353a5a7e27f31262dac12d6b:eb1dd75ea6c84e25300d5a244138ab3c
```

# 如何解密加密的文本

添加解密功能:

```
const decrypt = (encryptedText, password) => {
  try {
    const textParts = encryptedText.split(':');
    const iv = Buffer.from(textParts.shift(), 'hex'); const encryptedData = Buffer.from(textParts.join(':'), 'hex');
    const key = crypto.createHash('sha256').update(password).digest('base64').substr(0, 32);
    const decipher = crypto.createDecipheriv('aes-256-cbc', key, iv);

    const decrypted = decipher.update(encryptedData);
    const decryptedText = Buffer.concat([decrypted, decipher.final()]);
    return decryptedText.toString();
  } catch (error) {
    console.log(error)
  }
}
```

还有一个测试:

```
const text = "Hello World"
const pass = "secret1234"const encText = encrypt(text, pass)
console.log('encrypted text', encText);const decText = decrypt(encText, pass)
console.log('decrypted text', decText);
```

然后运行:

```
node encdec.js# Output
encrypted text 71596b9f5a99532f438fc5669b845680:248f6cb24a4ebeb174bbb73953115fd5
decrypted text Hello World
```

来源:[https://gist . github . com/vlu cas/2 BD 40 f 62d 20 C1 d 49237 a 109d 491974 EB](https://gist.github.com/vlucas/2bd40f62d20c1d49237a109d491974eb)