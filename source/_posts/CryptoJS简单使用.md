title: CryptoJS简单使用
categories: javascript
tags: javascript
date: 2016-02-25 15:40:23

---

<!--head-->

CryptoJS简单使用. 简单看下几种加密和哈希函数.

1. md5
2. sha1
3. ase
4. base

<!--more-->

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>CryptoJS</title>
    <script src="js/CryptoJS%20v3.1.2/components/core.js"></script>
    <script src="js/CryptoJS%20v3.1.2/components/md5.js"></script>
    <script src="js/CryptoJS%20v3.1.2/components/evpkdf.js"></script>
    <script src="js/CryptoJS%20v3.1.2/components/enc-base64.js"></script>
    <script src="js/CryptoJS%20v3.1.2/components/cipher-core.js"></script>
    <script src="js/CryptoJS%20v3.1.2/components/aes.js"></script>
    <script src="js/CryptoJS%20v3.1.2/components/hmac.js"></script>
    <script src="js/CryptoJS%20v3.1.2/components/sha1.js"></script>
    <script src="js/CryptoJS%20v3.1.2/components/sha256.js"></script>
</head>
<body>
<div id="content"></div>
    <script>
        var md5 = CryptoJS.MD5("Message").toString(CryptoJS.enc.Hex);
        console.log("md5 = %s", md5);

        var sHA1 = CryptoJS.SHA1("Message").toString(CryptoJS.enc.Hex);
        console.log("sHA1 = %s", sHA1);

        var sHA256 = CryptoJS.SHA256("Message").toString(CryptoJS.enc.Hex);
        console.log("sHA256 = %s", sHA256);

        var hmacMD5 = CryptoJS.HmacMD5("Message", "Secret Passphrase").toString(CryptoJS.enc.Hex);
        console.log("hmacMD5 = %s", hmacMD5);

        var hmacSHA1 = CryptoJS.HmacSHA1("Message", "Secret Passphrase").toString(CryptoJS.enc.Hex);
        console.log("hmacSHA1 = %s", hmacSHA1);

        var aesEncrypt = CryptoJS.AES.encrypt("Message", "Secret Passphrase");
        console.log("aesEncrypt = %s", aesEncrypt.iv.toString(CryptoJS.enc.Hex));

        var aesDecrypt = CryptoJS.AES.decrypt(aesEncrypt, "Secret Passphrase");
        console.log("aesDecrypt = %s", aesDecrypt.toString(CryptoJS.enc.Utf8));

        // base64 encrypt
        var rawStr = "hello world!";
        var wordArray = CryptoJS.enc.Utf8.parse(rawStr);
        var base64 = CryptoJS.enc.Base64.stringify(wordArray);
        console.log('base64Encrypt = ', base64);

        // base64 decrypt
        var parsedWordArray = CryptoJS.enc.Base64.parse(base64);
        var parsedStr = parsedWordArray.toString(CryptoJS.enc.Utf8);
        console.log('base64Decrypt = ',parsedStr);
    </script>
</body>
</html>
```

<!--body-->