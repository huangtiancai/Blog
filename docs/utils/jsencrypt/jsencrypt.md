### 什么是RSA加密
RSA加密算法是一种非对称加密算法

### RSA加密与解密
使用公钥加密的数据,利用私钥进行解密，更多参考 [RSA算法](http://www.ruanyifeng.com/blog/2013/06/rsa_algorithm_part_one.html)

### RSA秘钥生成方式
使用git命令行工具,打开git bash,生成私钥，密钥长度为1024bit
```
λ openssl genrsa -out private.pem 1024
Generating RSA private key, 1024 bit long modulus (2 primes)
........+++++
......................................................................+++++
e is 65537 (0x010001)
```

从私钥中提取公钥
```
λ openssl rsa -in private.pem -pubout -out public.pem
writing RSA key
```
这样就生成了`private.pem` 和 `public.pem`两个文件，可以利用终端进行查看

![Snipaste_2021-03-08_13-31-07.png](https://i.loli.net/2021/03/08/thoBCRvuW9jGNLH.png)

秘钥示例：
私钥private.pem
```
-----BEGIN RSA PRIVATE KEY-----
MIICXQIBAAKBgQDLubOGAFwwJ/xVDo1rtlF4mmW6rYpDAwi6EhaPd7SjDNWw4z48
QHbHSJ0R4s4ObAx/Mbd3yfPuiIdzrKvw6oMQQdbXF6guhWLqHKvAV4qlE/t5lKTU
jR2H9TGax+R7md8nOMki2WwpY/MdyPJh/xKeJwf8y+ILKb92oeZzrqYO+wIDAQAB
AoGBAL7cuLMnLTc0jvPFEXtDMOrTg9Ez+p+zfP6OKbK5jHNhd+Yjz8+0+VLU1crG
+ROL6N1VX7SLcMwd/wDBWcj4fFYW9wv0WcY2SWFYj7r7WWvIzuBLZGfR0zjab30q
5MCUEssyXAUsMti+9gMZZ/Lj7u3I8L+ixJFpCcQ3vhpVhKghAkEA871i2Kyq1mRb
DUJEuh3QiAN/NjL7vUO8ANLzSZ+yaPw/hDyMWK/L1l1FVWLoYpN0rPV32ik92clX
mNFnzTMmsQJBANX5DbrpgiP/yDHEmpoZswCpEy4hYT1PptTLBzsEDI/IDX8bwqkc
17GW68VZIRPl4srvw7nDSqXK/tyklFGx02sCQCSR8MfDuGosaoDlxXwLRyNxKuAN
7DlsdUPGYtxUCqe32SvVDdWsoq/KFMIH8ggAScw9lDr2XyJTFEKIgMOH/jECQQDS
wO+0JaGIobxmwLZiiGOWh/IbYsdrY1P4jk195HwW9r3Mb+RpO7577iImDKcW+TxM
FKMdCm0xJeOoIfbxDI0nAkAk8WFutz3j3pKHoC7eqP4RpCuFVpEFREQF9tCwLiSw
8i5QLjskZcbObwar9QI/sJzMKwa/I4DQ62uv1trojqgi
-----END RSA PRIVATE KEY-----
```
公钥public.pem
```
-----BEGIN PUBLIC KEY-----
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDLubOGAFwwJ/xVDo1rtlF4mmW6
rYpDAwi6EhaPd7SjDNWw4z48QHbHSJ0R4s4ObAx/Mbd3yfPuiIdzrKvw6oMQQdbX
F6guhWLqHKvAV4qlE/t5lKTUjR2H9TGax+R7md8nOMki2WwpY/MdyPJh/xKeJwf8
y+ILKb92oeZzrqYO+wIDAQAB
-----END PUBLIC KEY-----
```

或者使用[脚本之家的在线工具](http://tools.jb51.net/password/rsa_encode)生成秘钥

### 关于jsencryp
- `jsencryp` 是使用 一款基于`rsa`加解密的js库
- `应用场景`是在用户注册或登录的时候，用公钥对密码进行加密，再去传给后台，后台用私钥对加密的内容进行解密，然后进行密码校验或者保存到数据库。

### 封装一个jsencryp工具rsaEncrypt.js
1. 安装
```bash
npm install jsencrypt 
```
2. 引入
```js
import JSEncrypt from 'jsencrypt/bin/jsencrypt'
```
3. 秘钥准备
```js
const publicKey = '-----BEGIN PUBLIC KEY-----' +
    'MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDLubOGAFwwJ/xVDo1rtlF4mmW6' +
    'rYpDAwi6EhaPd7SjDNWw4z48QHbHSJ0R4s4ObAx/Mbd3yfPuiIdzrKvw6oMQQdbX' +
    'F6guhWLqHKvAV4qlE/t5lKTUjR2H9TGax+R7md8nOMki2WwpY/MdyPJh/xKeJwf8' +
    'y+ILKb92oeZzrqYO+wIDAQAB' +
    '-----END PUBLIC KEY-----' +
const privateKey = '-----BEGIN RSA PRIVATE KEY-----' +
    'MIICXQIBAAKBgQDLubOGAFwwJ/xVDo1rtlF4mmW6rYpDAwi6EhaPd7SjDNWw4z48' +
    'QHbHSJ0R4s4ObAx/Mbd3yfPuiIdzrKvw6oMQQdbXF6guhWLqHKvAV4qlE/t5lKTU' +
    'jR2H9TGax+R7md8nOMki2WwpY/MdyPJh/xKeJwf8y+ILKb92oeZzrqYO+wIDAQAB' +
    'AoGBAL7cuLMnLTc0jvPFEXtDMOrTg9Ez+p+zfP6OKbK5jHNhd+Yjz8+0+VLU1crG' +
    '+ROL6N1VX7SLcMwd/wDBWcj4fFYW9wv0WcY2SWFYj7r7WWvIzuBLZGfR0zjab30q' +
    '5MCUEssyXAUsMti+9gMZZ/Lj7u3I8L+ixJFpCcQ3vhpVhKghAkEA871i2Kyq1mRb' +
    'DUJEuh3QiAN/NjL7vUO8ANLzSZ+yaPw/hDyMWK/L1l1FVWLoYpN0rPV32ik92clX' +
    'mNFnzTMmsQJBANX5DbrpgiP/yDHEmpoZswCpEy4hYT1PptTLBzsEDI/IDX8bwqkc' +
    'v17GW68VZIRPl4srvw7nDSqXK/tyklFGx02sCQCSR8MfDuGosaoDlxXwLRyNxKuAN' +
    '7DlsdUPGYtxUCqe32SvVDdWsoq/KFMIH8ggAScw9lDr2XyJTFEKIgMOH/jECQQDS' +
    'wO+0JaGIobxmwLZiiGOWh/IbYsdrY1P4jk195HwW9r3Mb+RpO7577iImDKcW+TxM' +
    'FKMdCm0xJeOoIfbxDI0nAkAk8WFutz3j3pKHoC7eqP4RpCuFVpEFREQF9tCwLiSw' +
    '8i5QLjskZcbObwar9QI/sJzMKwa/I4DQ62uv1trojqgi' +
    '-----END RSA PRIVATE KEY-----' 
```

4. 加密
```js
// 加密
export function encrypt(txt) {
	// 创建加密对象实例
	const encryptor = new JSEncrypt()
	// 设置公钥
	encryptor.setPublicKey(publicKey)
	// 对需要加密的数据进行加密 txt-需要加密的内容
	return encryptor.encrypt(txt)
}
```

5. 解密
```js
// 解密
export function decrypt(txt) {
	// 创建解密对象实例
	const decryptor = new JSEncrypt()
	// 设置秘钥
	decryptor.setPrivateKey(privateKey)
	// 解密之前拿公钥加密的内容 txt-公钥加密过的内容
	return decryptor.decrypt(txt)
}
```

6. 页面使用
```js
import { encrypt } from '@/utils/rsaEncrypt';
let rasEncript = encrypt(password);
```
