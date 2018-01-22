[Android 安全加密专题](http://blog.csdn.net/axi295309066/article/details/52491077)

### 对称加密

加密和解密都使用同一把密钥。

##### 常见对称加密算法：

AES、DES、3DES、TDEA、Blowfish、RC2、RC4、RC5、IDEA、SKIPJACK 等。

##### **对称加密应用场景**：

- 本地数据加密：加密SharedPreferences或数据库里面的一些数据
- 网络传输：请求参数加密
- 加密用户登录结果信息并序列化到本地磁盘
- 网页交互数据加密（https）

##### **注意事项**：

把密钥写在代码里有一定风险，当别人反编译代码的时候，可能会看到密钥，android 开发里建议用JNI 把密钥值写到C代码里，甚至拆分成几份，最后再组合成真正的密钥

### 非对称加密

非对称加密算法需要两个密钥：公钥（publickey）和私钥（privatekey）。公钥与私钥是一对，如果用公钥对数据进行加密，只有用对应的私钥才能解密；如果用私钥对数据进行加密，那么只有用对应的公钥才能解密。

![非对称加密](http://img.blog.csdn.net/20160910133438466)



##### 常见非对称加密算法：

RSA、Elgamal、背包算法、Rabin、D-H、ECC（椭圆曲线加密算法）等，其中支付宝使用的就是RSA算法。

##### 非对称加密应用场景：

- 身份认证：一条加密信息若能用A 的公钥能解开，则该信息一定是用A 的私钥加密的，该能确定该用户是A。
- 陌生人通信：A 和B 两个人互不认识，A 把自己的公钥发给B，B 也把自己的公钥发给A，则双方可以通过对方的公钥加密信息通信。C 虽然也能得到A、B 的公钥，但是他解不开密文。
- 密钥交换

非对称加密一般不会单独拿来使用，它并不是为了取代对称加密而出现的，非对称加密速度比对称加密慢很多，极端情况下会慢1000 倍，所以一般不会用来加密大量数据，通常我们经常会将对称加密和非对称加密两种技术联合起来使用，例如用非对称加密来给称加密里的秘钥进行加密（即秘钥交换）。

### 消息摘要（Message Digest）

**消息摘要保证了消息的完整性**。如果发送者发送的消息，在传输过程中被恶意篡改，那么接收者收到消息后，用同样的摘要算法计算其摘要，如果新摘要与发送者原始摘要不同，那么接收者就知道消息被篡改了。

##### 常见算法：

- Hash 算法（散列算法），如MD5、SHA1、SHA256等。
- MAC 算法（消息认证码算法），基于HASH算法之上，增加了密钥的支持以提高安全性。如HmacMD5/HmacSHA1/HmacSHA256 等。

##### 算法主要特点：

- 变长输入，定长输出。无论输入的消息有多长，计算出来的消息摘要的长度总是固定的。
- 只要输入的消息不同，对其进行摘要以后产生的摘要消息也必不相同；但相同的输入必会产生相同的输出。
- 消息摘要是**单向、不可逆**的。能进行正向的信息摘要，而无法从摘要中恢复出任何的原始消息。

### 数字签名

数字签名是非对称加密与数字摘要的组合应用。

##### 应用场景：

- 校验用户身份（使用私钥签名，公钥校验，只要用公钥能校验通过，则该信息一定是私钥持有者发布的）
- 校验数据的完整性（用解密后的消息摘要跟原文的消息摘要进行对比）

数字签名有两种功效：一是能确定消息确实是由发送方签名并发出来的，因为别人假冒不了发送方的签名。二是数字签名能确定消息的完整性。因为数字签名的特点是它代表了文件的特征，文件如果发生改变，数字摘要的值也将发生变化。不同的文件将得到不同的数字摘要。一次数字签名涉及到一个哈希函数、发送者的[公钥](http://baike.baidu.com/view/355291.htm)、发送者的[私钥](http://baike.baidu.com/view/493846.htm)。

数字签名一般不单独使用，基本都是用在数字证书里实现SSL通信协议。下面将学习的数字证书就是基于数字签名技术实现的。