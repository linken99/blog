### HTTPS, TLS

#### HTTPS理解 ####

证书的核心组成：公钥密文+公钥签名+签名算法(一般是SHA256)。

1）密文=CA.私钥加密（Server.公钥）；2）签名=签名算法SHA256（公钥）；3）签名算法SHA256

客户端认证服务端身份。服务端将证书下发给客户端，注意不是下发公钥，认证通过后取出证书中的公钥，客户端再生成一个随机数(即对称加密密钥)，然后使用证书中的公钥加密该随机数发给服务端，然后客户端和服务端使用该公钥加解密传输的数据。注意随机数/密钥是客户端生成的，不是服务端生成的，如果是服务端生成的则会用服务端私钥加密，而拿到公钥的都能解开该数据获取到密钥。

#### SSL/TLS Cipher Suite ####

一个Cipher Suite是一个四件套，由各类基础的加密算法组成，主要包含了四类：
Key Exchange 密钥交换算法；
Authentication 身份认证算法；
Encryption 对称加密算法；
Message Authentication Code 消息认证码算法（信息摘要缩放）；

![img](https://img-blog.csdnimg.cn/img_convert/74fede4b1a7aaed6bdca62ad603795f3.png)



##### SSL/TLS 参考网址 #####

SSL/TLS CipherSuite介绍 https://cloud.tencent.com/developer/article/1839188
SSL/TLS Cipher Suites https://blog.csdn.net/fuhanghang/article/details/123126052
TLS版本及CipherSuites协商及设置 https://blog.csdn.net/Cara_EDI_Consultant/article/details/128147269
SSL/TLS握手 https://zhuanlan.zhihu.com/p/513404995

#### 用了HTTPS 网站就一定安全吗？307端口是什么意思？360/QQ安全浏览器做了什么能叫安全浏览器？ ####

用来HTTPS不一定就万事无忧，中间人做个假的taobao、jd、银行网站就能欺骗用户/浏览器。360/QQ的安全主要在地址上做文章，如你访问中国银行或支付宝网站，它会让你访问真正的网站，如果遇到钓鱼的它会提示或打不开或帮你跳转到真正的网站。

307只能是同域名不同协议跳转，如你访问http://www.jd.com, 307限定你只能跳转到https://www.jd.com ，而不能是任何其它名字。

#### 区别：CA、CA证书、根证书、中间证书、CA认证的证书、自签名证书、X.509证书、Openssl ####

CA：是证书认证机构，为其他的企业提供证书认证服务并收取费用。

CA证书：CA机构自己的证书，CA会自己创建一对公私钥，私钥严格保管保证不会泄露，公钥自己给自己签发一个证书，这个证书就是该CA机构签发的所有证书的根，也叫根证书，即CA证书就是根证书。CA证书/根证书是安装在操作系统里面的。

中间证书：CA机构为了保护根证书(如果根证书的私钥泄露了、或出现问题则该CA签发过的证书形成的信任链都有问题，信任链就会崩溃)，不会直接用根证书给终端用户、网站签发证书，而是会用自己的根证书签发一些中间证书，再用这些中间证书给终端用户、网站签发证书。如果某个中间证书出问题废弃掉该中间证书即可，用根证书再重新签发一个。

CA认证的证书：即CA机构使用中间证书给终端用户、网站签发的证书。

X.509：证书格式，是当前通用的证书国际标准。X.509证书包含3个文件:**.key, .csr, .crt**

openssl：开放ssl，是一个开源的ssl程序/工具/库，提供了加解密、生成密钥、生成key pair、生成证书等多种功能。

#### .key、.csr、.crt、pem、der含义 ####

key：通常指私钥
CSR：Certificate Signing Request的缩写，即证书签名申请，这不是证书，这是要求CA给证书签名的一种正是申请，该申请包含申请证书的实体的公钥及该实体店某些信息。该数据将成为证书的一部分，CSR始终使用它携带的公钥所对应的私钥进行签名。
CRT：certificate的缩写，即证书
PEM：Privacy Enhanced Mail,打开看文本格式,以"-----BEGIN..."开头, "-----END..."结尾,内容是BASE64编码
der：证书的二进制格式文件

#### JDK keystore文件导入的是什么证书？PKCS12 ####

keystore用来存储JVM程序的私钥、公钥、证书，PKCS12是keystore文件格式/规范。

keystore中导入的一般是对方的CA证书/根证书或者是中间证书。

#### 当前系统需要mTLS(mutual TLS，双向身份认证)吗？ ####

不需要，因为用户数据是经过银行公钥加密、自己私钥签名的，需要客户公钥、银行私钥才能进行验签、解密，其他人最多只能获取到客户公钥，是没办法解开用户数据的。当前的安全性足够API调用的场合下用了。

#### 自签名证书 ####

**生成ca证书**

openssl生成ca key(ca的私钥)文件->根据ca key生成 ca csr文件->根据ca csr生成ca crt文件。自此ca证书已经生成。

**生成 server证书**

openssl生成server key(网站服务器私钥)->根据server key生成server csr文件->根据server csr生成server crt文件->再用ca key、ca crt给server crt签名。自此使用自建的ca证书/根证书给自己的网站签发了证书。将该server crt配置到网站服务器然后用https访问该网站，浏览器会提示“你的连接不是私密连接”，原因是浏览器验证你的网站证书不通过，提示你访问的网站存在风险。

将ca证书/ca crt安装到你的操作系统“受信任的根证书”，此时再访问网站“你的连接不是私密连接”提示没有了，但浏览器地址栏会变红打叉，原因是浏览器从操作系统读到了根证书并验证了该网站证书通过了，但浏览器认为该根证书不是有背书/权威机构的证书，因此在地址栏里提示该网站有安全隐患。



#### 参考网址 ####

自签名证书和CA证书的区别和制作、使用 https://www.cnblogs.com/zhaobowen/p/13321578.html
keytool命令制作CA根证书，签发二级证书 https://www.cnblogs.com/dijia478/p/12103977.html
java 证书工具keytool生成自签名证书和自签CA证书 https://www.jianshu.com/p/8e065153f315
JAVA 导入信任证书 (Keytool 的使用) https://blog.csdn.net/ljskr/article/details/84570573
OpenSSL自签名证书 http://mjpclab.site/linux/openssl-self-signed-certificate
细说 CA 和证书 https://www.barretlee.com/blog/2016/04/24/detail-about-ca-and-certs/
CA & OpenSSL自签名证书 https://juejin.cn/post/7092789498823573518

