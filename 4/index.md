# 逆向AES加密脚本

<!--more-->

## Nodejs进行AES加密

{{< admonition>}}

本地需要Nodejs环境

{{< /admonition >}}

```js
var CryptoJS = require("crypto-js");
// 只需找到nv和iv
var nv =
{
    "words": [],
    "sigBytes": 
}
var iv = 
{
    "words": [],
    "sigBytes":
}
function encrypt(x) {
    s = CryptoJS.AES.encrypt(x, nv, 
    {
        iv: iv,
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7
    }).toString()
    return s
}
```

## python脚本调用

```python
import execjs
s = execjs.compile(open('test.js','r').read()).call('encrypt','test')
print(s)
```

