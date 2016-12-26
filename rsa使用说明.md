# RSA使用说明

## 一、OpenSSL工具安装

Linux用户（以Ubuntu为例） sudo apt-get install openssl

Windows用户 开发者可以在OpenSSL官方网站下载Windows的OpenSSL安装包进行安装。

## 二、RSA私钥及公钥生成

**1.Linux用户（以Ubuntu为例）**

$ openssl 进入OpenSSL程序

OpenSSL&gt; genrsa -out rsa\_private\_key.pem 1024 生成私钥

OpenSSL&gt; pkcs8 -topk8 -inform PEM -in rsa\_private\_key.pem -outform PEM -out rsa\_private\_key.pem -nocrypt Java开发者需要将私钥转换成PKCS8格式

OpenSSL&gt; rsa -in rsa\_private\_key.pem -pubout -out rsa\_public\_key.pem 生成公钥

OpenSSL&gt; exit \#\# 退出OpenSSL程序

**2.Windows用户在cmd窗口中进行以下操作：**

C:\Users\Hammer&gt;cd C:\OpenSSL-Win32\bin 进入OpenSSL安装目录

C:\OpenSSL-Win32\bin&gt;openssl.exe 进入OpenSSL程序

OpenSSL&gt; genrsa -out rsa\_private\_key.pem 1024 生成私钥

OpenSSL&gt; pkcs8 -topk8 -inform PEM -in rsa\_private\_key.pem -outform PEM -out rsa\_private\_key.pem -nocrypt Java开发者需要将私钥转换成PKCS8格式

OpenSSL&gt; rsa -in rsa\_private\_key.pem -pubout -out rsa\_public\_key.pem 生成公钥

OpenSSL&gt; exit \#\# 退出OpenSSL程序

注意：对于使用Java的开发者，将pkcs8在console中输出的私钥去除头尾、换行和空格，作为开发者私钥，对于.NET和PHP的开发者来说，无需进行pkcs8命令行操作。

经过以上步骤，开发者可以在当前文件夹中（Windows用户在C:\OpenSSL-Win32\bin）看到rsa\_private\_key.pem和rsa\_public\_key.pem两个文件，前者为私钥，后者为公钥。开发者将私钥保留，将公钥上传到云钱包，用于信息加密及解密。以下为使用OpenSSL生成的私钥文件和公钥文件示例。

```
· 公钥文件示例
```

-----BEGIN PRIVATE KEY----- MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCDPgtBScLNsjjKRp1c2Uwyrly0SHIxrT4F6HYc dkw8GxZTlifA7jcLqaTbVgZQSlt95fvBgPA0ca6g++CkeZU2pKEmg6qSgQx496GfX2behDaZCbi4 4nch/Lfvc3+i341F2m/BQ4/hEmIqFrZ02FRgpntnyNKnQs6fL02PGKkwdQIDAQAB -----END PRIVATE KEY-----

```
· 私钥文件示例
```

-----BEGIN RSA PRIVATE KEY----- MIICdgIBADANBgkqhkiG9w0BAQEFAASCAmAwggJcAgEAAoGBAIM+C0FJws2yOMpGnVzZTDKuXLRI cjGtPgXodhx2TDwbFlOWJ8DuNwuppNtWBlBKW33l+8GA8DRxrqD74KR5lTakoSaDqpKBDHj3oZ9f Zt6ENpkJuLjidyH8t+9zf6LfjUXab8FDj+ESYioWtnTYVGCme2fI0qdCzp8vTY8YqTB1AgMBAAEC gYAbqVcL8rT5T8gCcjG2oSIbSH7HjMFs0PvSmPVT+GcHABqLkYldc5fsgFv70rzP7WwtM+0FEt0N 4KwSyCPH5sxZAyG4rf6N27wvCyHOMlxgbWBJ1tmAESPGJUcbVQMO74hT+NxuE7VGw6QgbUF5qXhf gXkcIm3wd8gFBw+jJiTcwQJBANBJ2P7jsO15tKwjhVs6UiGKBhB1Hv56LDwn4rVzjvz1JSvVNXeV XpXNV0sulEaoUZRjA3jkpZExa3Xuh7saNUUCQQChTitd+3/01cmD8p06jvuD9xLoANiKKQuaGftI JlvbfboYz3WvHZ8agVy7JjCdfuTMlYkld/hbtA6ed5TaO0lxAkEAm5tgCuSN7IwtJyEOYt5KN5ZG +4qUUidx3qspmsevPlnioEGTxTgJRr72hUtSKQtcjw/9qxaefr89+gfuzSBCRQJAW6ko2ZIFxyIJ DfK6x8DiSb4Hv1BjvDbQwfPLp9csUZCjRF/3Vtg1RgGGqU5tR8IIz/yVX3ZJ6gpqWEBJlK0l8QJA R43FxuJz6dnmrNZIhy76l67Gw/Iq6FxInCY0F9Gw4TNATdc6S76fz0PwZvtu9iwNd1lmWQ8P4iOH HIiE4vUF9w== -----END RSA PRIVATE KEY-----

通过程序生成RSA私钥及公钥，以Java生成为例：

package example;

import java.security.Key;

import java.security.KeyPair;

import java.security.KeyPairGenerator;

import java.security.interfaces.RSAPrivateKey;

import java.security.interfaces.RSAPublicKey;

import java.util.HashMap;

import java.util.Map;

import sun.misc.BASE64Decoder;

import sun.misc.BASE64Encoder;

@SuppressWarnings\({ "unused", "restriction" }\)

public class Keys {

public static final String KEY\_ALGORITHM = "RSA";

public static final String SIGNATURE\_ALGORITHM = "MD5withRSA";

private static final String PUBLIC\_KEY = "RSAPublicKey";

private static final String PRIVATE\_KEY = "RSAPrivateKey";

public static void main\(String\[\] args\) {

```
Map keyMap;

try {

  keyMap = initKey\(\);

  String publicKey = getPublicKey\(keyMap\);

  System.out.println\("公钥 ："\);

  System.out.println\(publicKey\);

  String privateKey = getPrivateKey\(keyMap\);

  System.out.println\("私钥 ："\);

  System.out.println\(privateKey\);

} catch \(Exception e\) {

  e.printStackTrace\(\);

}
```

}

public static String getPublicKey\(Map keyMap\) throws Exception {

```
Key key = \(Key\) keyMap.get\(PUBLIC\_KEY\);

byte\[\] publicKey = key.getEncoded\(\);

return encryptBASE64\(key.getEncoded\(\)\);
```

}

public static String getPrivateKey\(Map keyMap\) throws Exception {

```
Key key = \(Key\) keyMap.get\(PRIVATE\_KEY\);

byte\[\] privateKey = key.getEncoded\(\);

return encryptBASE64\(key.getEncoded\(\)\);
```

}

public static byte\[\] decryptBASE64\(String key\) throws Exception {

```
return \(new BASE64Decoder\(\)\).decodeBuffer\(key\);
```

}

public static String encryptBASE64\(byte\[\] key\) throws Exception {

```
return \(new BASE64Encoder\(\)\).encodeBuffer\(key\);
```

}

public static Map initKey\(\) throws Exception {

```
KeyPairGenerator keyPairGen = KeyPairGenerator.getInstance\(KEY\_ALGORITHM\);

keyPairGen.initialize\(1024\);

KeyPair keyPair = keyPairGen.generateKeyPair\(\);

RSAPublicKey publicKey = \(RSAPublicKey\) keyPair.getPublic\(\);

RSAPrivateKey privateKey = \(RSAPrivateKey\) keyPair.getPrivate\(\);

Map keyMap = new HashMap\(2\);

keyMap.put\(PUBLIC\_KEY, publicKey\);

keyMap.put\(PRIVATE\_KEY, privateKey\);

return keyMap;
```

}

}

输出结果

公钥 ： MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCYrCpn1+bIopUCGwju4DBGndxbC7dvpbK/T50E FYYcxVviH7Bsfvvk6sg3+T02jmRsZ+G+wHrBq+K3JeT95uR/W8wS5LB7L6BYziGgQSPjK/BYzcp1 VvpiXZeRtkSqfpZpKXxwoDRCII43ct/v0EMKibJVJzXX42WzDsIcIWJOMwIDAQAB

私钥 ： MIICdgIBADANBgkqhkiG9w0BAQEFAASCAmAwggJcAgEAAoGBAJisKmfX5siilQIbCO7gMEad3FsL t2+lsr9PnQQVhhzFW+IfsGx+++TqyDf5PTaOZGxn4b7AesGr4rcl5P3m5H9bzBLksHsvoFjOIaBB I+Mr8FjNynVW+mJdl5G2RKp+lmkpfHCgNEIgjjdy3+/QQwqJslUnNdfjZbMOwhwhYk4zAgMBAAEC gYBSLhJhdVDv3LwStxS26Ixz5pNvmr3x5iJyYltlkGRxZjbQYDhqHmxey5ZcstelXz5lMAHO2PL6 /xf5d/dsSHXj1dn7pRfVvnabW2uETsur+20E1cxQ4uRaAk+C+WG9bph2vUmOdPsiNxAOq45GbKoj l0q2BlkJSQzXh0f7Z91/2QJBAN7raOqRIASikWiyPTUkJLzWGvknkNAQc3u38RiJAYITCRuJqvjD xgixxl4TqjL+yDmid9twBNGOscy9KCovz0cCQQCvVBwRmEzN8QA5daE4qGbqZ1f8Uqujl1YbMutk O1iFHBQoKG5dXyWNxpYFfIMSs0AwzBj/aI/kcoPby61Kz7e1AkEAoVD2MZkn9HK4i21Awe4P7994 0YkCUK83AvbPsBOlVb30v0rWwQLbknsjs/zDE/gwaRTba58avZNns2PHZxAGDQJAdeNWJDaFnguo HPqM9u20lXP7YzurEQpW6V7pi7GjqYzhuMbGvp2VQKkAgpvf/hjs1mLFhCaoafDd3FItKRpV6QJA agJGCyurnbo976NGmCo8UUuOzKa6OAGEmOY/wNxoEYVMLApl4lt+ZKnRHIcU0h14O13uRluvV89x lsHZzM9vew==

