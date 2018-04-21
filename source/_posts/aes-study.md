---
title: AES学习
date: 2018-02-13 16:11:02
tags: [技术,安全,密码学,学习笔记,Crypto]
---

最近在写一个常见加密编码的类库[Crypto](https://github.com/WWILLV/Crypto)，其中涉及到很多加密及编码方式。写到了AES这里，遇到点麻烦，也是学习了一下AES加密。

>高级加密标准（英语：Advanced Encryption Standard，缩写：AES），在密码学中又称Rijndael加密法，是美国联邦政府采用的一种区块加密标准。这个标准用来替代原先的DES，已经被多方分析且广为全世界所使用。经过五年的甄选流程，高级加密标准由美国国家标准与技术研究院（NIST）于2001年11月26日发布于FIPS PUB 197，并在2002年5月26日成为有效的标准。2006年，高级加密标准已然成为对称密钥加密中最流行的算法之一。
该算法为比利时密码学家Joan Daemen和Vincent Rijmen所设计，结合两位作者的名字，以Rijndael为名投稿高级加密标准的甄选流程。（Rijndael的发音近于"Rhine doll"）（来自[维基百科](https://zh.wikipedia.org/wiki/%E9%AB%98%E7%BA%A7%E5%8A%A0%E5%AF%86%E6%A0%87%E5%87%86)）

<!--more-->
## AES
[高级加密标准 - 维基百科](https://zh.wikipedia.org/wiki/%E9%AB%98%E7%BA%A7%E5%8A%A0%E5%AF%86%E6%A0%87%E5%87%86)
严格地说，AES和Rijndael加密法并不完全一样（虽然在实际应用中两者可以互换），因为Rijndael加密法可以支持更大范围的区块和密钥长度：AES的区块长度固定为128比特，密钥长度则可以是128，192或256比特；而Rijndael使用的密钥和区块长度均可以是128，192或256比特。加密过程中使用的密钥是由Rijndael密钥生成方案产生。

大多数AES计算是在一个特别的有限域完成的。

AES加密过程是在一个4×4的字节矩阵上运作，这个矩阵又称为“体（state）”，其初值就是一个明文区块（矩阵中一个元素大小就是明文区块中的一个Byte）。（Rijndael加密法因支持更大的区块，其矩阵行数可视情况增加）加密时，各轮AES加密循环（除最后一轮外）均包含4个步骤：

1. AddRoundKey—矩阵中的每一个字节都与该次回合密钥（round key）做XOR运算；每个子密钥由密钥生成方案产生。
2. SubBytes—通过一个非线性的替换函数，用查找表的方式把每个字节替换成对应的字节。
3. ShiftRows—将矩阵中的每个横列进行循环式移位。
4. MixColumns—为了充分混合矩阵中各个直行的操作。这个步骤使用线性转换来混合每内联的四个字节。最后一个加密循环中省略MixColumns步骤，而以另一个AddRoundKey取代。

## 分组密码工作模式
[分组密码工作模式 - 维基百科](https://zh.wikipedia.org/wiki/%E5%88%86%E7%BB%84%E5%AF%86%E7%A0%81%E5%B7%A5%E4%BD%9C%E6%A8%A1%E5%BC%8F)
密码学中，分组（block）密码的工作模式（mode of operation）允许使用同一个分组密码密钥对多于一块的数据进行加密，并保证其安全性。
分组密码自身只能加密长度等于密码分组长度的单块数据，若要加密变长数据，则数据必须先被划分为一些单独的密码块。通常而言，最后一块数据也需要使用合适填充方式将数据扩展到匹配密码块大小的长度。一种工作模式描述了加密每一数据块的过程，并常常使用基于一个通常称为初始化向量的附加输入值以进行随机化，以保证安全。

工作模式主要用来进行加密和认证。对加密模式的研究曾经包含数据的完整性保护，即在某些数据被修改后的情况下密码的误差传播特性。后来的研究则将完整性保护作为另一个完全不同的，与加密无关的密码学目标。部分现代的工作模式用有效的方法将加密和认证结合起来，称为认证加密模式。

虽然工作模式通常应用于对称加密，它亦可以应用于公钥加密，例如在原理上对RSA进行处理，但在实用中，公钥密码学通常不用于加密较长的信息，而是使用结合对称加密和公钥加密的混合加密方案。

### 初始化向量（IV）
[初始化向量 - 维基百科](https://zh.wikipedia.org/wiki/%E5%88%9D%E5%A7%8B%E5%90%91%E9%87%8F)
初始化向量（IV，Initialization Vector）是许多任务作模式中用于将加密随机化的一个位块，由此即使同样的明文被多次加密也会产生不同的密文，避免了较慢的重新产生密钥的过程。

初始化向量与密钥相比有不同的安全性需求，因此IV通常无须保密，然而在大多数情况中，不应当在使用同一密钥的情况下两次使用同一个IV。对于CBC和CFB，重用IV会导致泄露明文首个块的某些信息，亦包括两个不同消息中相同的前缀。对于OFB和CTR而言，重用IV会导致完全失去安全性。另外，在CBC模式中，IV在加密时必须是无法预测的；特别的，在许多实现中使用的产生IV的方法，例如SSL2.0使用的，即采用上一个消息的最后一块密文作为下一个消息的IV，是不安全的。

### 填充
[填充 (密码学) - 维基百科](https://zh.wikipedia.org/wiki/%E5%A1%AB%E5%85%85_(%E5%AF%86%E7%A0%81%E5%AD%A6)
块密码只能对确定长度的数据块进行处理，而消息的长度通常是可变的。因此部分模式（即ECB和CBC）需要最后一块在加密前进行填充。有数种填充方法，其中最简单的一种是在明文的最后填充空字符以使其长度为块长度的整数倍，但必须保证可以恢复明文的原始长度；例如，若明文是C语言风格的字符串，则只有串尾会有空字符。稍微复杂一点的方法则是原始的DES使用的方法，即在数据后添加一个1位，再添加足够的0位直到满足块长度的要求；若消息长度刚好匹配块长度，则添加一个填充块。最复杂的则是针对CBC的方法，例如密文窃取，残块终结等，不会产生额外的密文，但会增加一些复杂度。布鲁斯·施奈尔和尼尔斯·弗格森提出了两种简单的可能性：添加一个值为128的字节（十六进制的80），再以0字节填满最后一个块；或向最后一个块填充n个值均为n的字节。

CFB，OFB和CTR模式不需要对长度不为密码块大小整数倍的消息进行特别的处理。因为这些模式是通过对块密码的输出与明文进行异或工作的。最后一个明文块（可能是不完整的）与密钥流块的前几个字节异或后，产生了与该明文块大小相同的密文块。流密码的这个特性使得它们可以应用在需要密文和明文数据长度严格相等的场合，也可以应用在以流形式传输数据而不便于进行填充的场合。

### 常用模式

#### 电子密码本（ECB）
最简单的加密模式即为电子密码本（Electronic codebook，ECB）模式。需要加密的消息按照块密码的块大小被分为数个块，并对每个块进行独立加密。

![](https://upload.wikimedia.org/wikipedia/commons/c/c4/Ecb_encryption.png)
![](https://upload.wikimedia.org/wikipedia/commons/6/66/Ecb_decryption.png)

本方法的缺点在于同样的明文块会被加密成相同的密文块；因此，它不能很好的隐藏数据模式。在某些场合，这种方法不能提供严格的数据保密性，因此并不推荐用于密码协议中。
ECB模式也会导致使用它的协议不能提供数据完整性保护，易受到重放攻击的影响，因此每个块是以完全相同的方式解密的。例如，“梦幻之星在线：蓝色脉冲”在线电子游戏使用ECB模式的Blowfish密码。在密钥交换系统被破解而产生更简单的破解方式前，作弊者重复通过发送加密的“杀死怪物”消息包以非法的快速增加经验值。

#### 密码块链接（CBC）
1976年，IBM发明了密码分组链接（CBC，Cipher-block chaining）模式。在CBC模式中，每个明文块先与前一个密文块进行异或后，再进行加密。在这种方法中，每个密文块都依赖于它前面的所有明文块。同时，为了保证每条消息的唯一性，在第一个块中需要使用初始化向量。

![](https://upload.wikimedia.org/wikipedia/commons/d/d3/Cbc_encryption.png)
![](https://upload.wikimedia.org/wikipedia/commons/6/66/Cbc_decryption.png)

若第一个块的下标为1，则CBC模式的加密过程为
![1.png](https://ooo.0o0.ooo/2018/02/13/5a82bb911e3e7.png)
而其解密过程则为
![2.png](https://ooo.0o0.ooo/2018/02/13/5a82bb911f0d7.png)
CBC是最为常用的工作模式。它的主要缺点在于加密过程是串行的，无法被并行化，而且消息必须被填充到块大小的整数倍。解决后一个问题的一种方法是利用密文窃取。

注意在加密时，明文中的微小改变会导致其后的全部密文块发生改变，而在解密时，从两个邻接的密文块中即可得到一个明文块。因此，解密过程可以被并行化，而解密时，密文中一位的改变只会导致其对应的明文块完全改变和下一个明文块中对应位发生改变，不会影响到其它明文的内容。

#### 填充密码块链接（PCBC）
填充密码块链接（PCBC，Propagating cipher-block chaining）或称为明文密码块链接（Plaintext cipher-block chaining），是一种可以使密文中的微小更改在解密时导致明文大部分错误的模式，并在加密的时候也具有同样的特性。
![](https://upload.wikimedia.org/wikipedia/commons/0/08/Pcbc_encryption.png)
![](https://upload.wikimedia.org/wikipedia/commons/2/23/Pcbc_decryption.png)
加密和解密算法如下：
![3.png](https://ooo.0o0.ooo/2018/02/13/5a82bc400dbbc.png)
PCBC主要用于Kerberos v4和WASTE中，而在其它场合的应用较少。对于使用PCBC加密的消息，互换两个邻接的密文块不会对后续块的解密造成影响。正因为这个特性，Kerberos v5没有使用PCBC。

#### 密文反馈（CFB）
密文反馈（CFB，Cipher feedback）模式类似于CBC，可以将块密码变为自同步的流密码；工作过程亦非常相似，CFB的解密过程几乎就是颠倒的CBC的加密过程：

![4.png](https://ooo.0o0.ooo/2018/02/13/5a82bca915655.png)
![](https://upload.wikimedia.org/wikipedia/commons/f/fd/Cfb_encryption.png)
![](https://upload.wikimedia.org/wikipedia/commons/7/75/Cfb_decryption.png)

上述公式是描述的是最简单的CFB，在这种模式下，它的自同步特性仅仅与CBC相同，即若密文的一整块发生错误，CBC和CFB都仍能解密大部分数据，而仅有一位数据错误。若需要在仅有了一位或一字节错误的情况下也让模式具有自同步性，必须每次只加密一位或一字节。可以将移位寄存器作为块密码的输入，以利用CFB的自同步性。

与CBC相似，明文的改变会影响接下来所有的密文，因此加密过程不能并行化；而同样的，与CBC类似，解密过程是可以并行化的。在解密时，密文中一位数据的改变仅会影响两个明文块：对应明文块中的一位数据与下一块中全部的数据，而之后的数据将恢复正常。

CFB拥有一些CBC所不具备的特性，这些特性与OFB和CTR的流模式相似：只需要使用块密码进行加密操作，且消息无需进行填充（虽然密文窃取也允许数据不进行填充）。

#### 输出反馈（OFB）
输出反馈模式（Output feedback, OFB）可以将块密码变成同步的流密码。它产生密钥流的块，然后将其与明文块进行异或，得到密文。与其它流密码一样，密文中一个位的翻转会使明文中同样位置的位也产生翻转。这种特性使得许多错误校正码，例如奇偶校验位，即使在加密前计算，而在加密后进行校验也可以得出正确结果。

由于XOR操作的对称性，加密和解密操作是完全相同的：

![5.png](https://ooo.0o0.ooo/2018/02/13/5a82bd41cd19a.png)
![](https://upload.wikimedia.org/wikipedia/commons/a/a9/Ofb_encryption.png)
![](https://upload.wikimedia.org/wikipedia/commons/8/82/Ofb_decryption.png)

每个使用OFB的输出块与其前面所有的输出块相关，因此不能并行化处理。然而，由于明文和密文只在最终的异或过程中使用，因此可以事先对IV进行加密，最后并行的将明文或密文进行并行的异或处理。

可以利用输入全0的CBC模式产生OFB模式的密钥流。这种方法十分实用，因为可以利用快速的CBC硬件实现来加速OFB模式的加密过程。

#### 计数器模式（CTR）
注意：CTR模式（Counter mode，CM）也被称为ICM模式（Integer Counter Mode，整数计数模式）和SIC模式（Segmented Integer Counter）。
与OFB相似，CTR将块密码变为流密码。它通过递增一个加密计数器以产生连续的密钥流，其中，计数器可以是任意保证长时间不产生重复输出的函数，但使用一个普通的计数器是最简单和最常见的做法。使用简单的、定义好的输入函数是有争议的：批评者认为它“有意的将密码系统暴露在已知的、系统的输入会造成不必要的风险”。目前，CTR已经被广泛的使用了，由输入函数造成的问题被认为是使用的块密码的缺陷，而非CTR模式本身的弱点。无论如何，有一些特别的攻击方法，例如基于使用简单计数器作为输入的硬件差错攻击。

CTR模式的特征类似于OFB，但它允许在解密时进行随机存取。由于加密和解密过程均可以进行并行处理，CTR适合运用于多处理器的硬件上。

注意图中的“nonce”与其它图中的IV（初始化向量）相同。IV、随机数和计数器均可以通过连接，相加或异或使得相同明文产生不同的密文。

![](https://upload.wikimedia.org/wikipedia/commons/3/3f/Ctr_encryption.png)
![](https://upload.wikimedia.org/wikipedia/commons/3/34/Ctr_decryption.png)

## 代码实现
代码为C#，为128位AES加解密（CBC模式PKCS7填充），需要引入命名空间System.Security.Cryptography
``` cs
/// <summary>
/// AES加密（CBC模式PKCS7填充）
/// </summary>
/// <param name="text">加密字符</param>
/// <param name="password">加密的密码</param>
/// <param name="iv">密钥向量</param>
/// <returns></returns>
public static string AESEncrypt(string text, string password, string iv)
{
    int size = 128;
    int bytesNum = size / 8;
    RijndaelManaged rijndaelCipher = new RijndaelManaged();
    rijndaelCipher.Mode = CipherMode.CBC;
    rijndaelCipher.Padding = PaddingMode.PKCS7;
    rijndaelCipher.KeySize = size;
    rijndaelCipher.BlockSize = 128;

    byte[] pwdBytes = System.Text.Encoding.UTF8.GetBytes(password);
    byte[] keyBytes = new byte[bytesNum];
    int len = pwdBytes.Length;
    if (len > keyBytes.Length) len = keyBytes.Length;
    System.Array.Copy(pwdBytes, keyBytes, len);
    rijndaelCipher.Key = keyBytes;
    byte[] ivBytes = System.Text.Encoding.UTF8.GetBytes(iv);
    rijndaelCipher.IV = ivBytes;
    ICryptoTransform transform = rijndaelCipher.CreateEncryptor();
    byte[] plainText = Encoding.UTF8.GetBytes(text);
    byte[] cipherBytes = transform.TransformFinalBlock(plainText, 0, plainText.Length);
    return Convert.ToBase64String(cipherBytes);
}

/// <summary>
/// 随机生成密钥向量
/// </summary>
/// <param name="n">位数</param>
/// <returns></returns>
public static string GetIv(int n)
{
	char[] arrChar = new char[]{

	'a','b','d','c','e','f','g','h','i','j','k','l','m','n','p','r','q','s','t','u','v','w','z','y','x',
    '0','1','2','3','4','5','6','7','8','9',

'A','B','C','D','E','F','G','H','I','J','K','L','M','N','Q','P','R','T','S','V','U','W','X','Y','Z'
	};

	StringBuilder num = new StringBuilder();
    Random rnd = new Random(DateTime.Now.Millisecond);
    for (int i = 0; i < n; i++)
    {
    	num.Append(arrChar[rnd.Next(0, arrChar.Length)].ToString());
	}

	return num.ToString();
}

/// <summary>
/// AES解密（CBC模式PKCS7填充）
/// </summary>
/// <param name="text">密文</param>
/// <param name="password">密码</param>
/// <param name="iv">密钥向量</param>
/// <returns></returns>
public static string AESDecrypt(string text, string password, string iv)
{
	int size = 128;
    int bytesNum = size / 8;
    RijndaelManaged rijndaelCipher = new RijndaelManaged();
    rijndaelCipher.Mode = CipherMode.CBC;   //CBC加密模式
    rijndaelCipher.Padding = PaddingMode.PKCS7; //PKCS7填充
    rijndaelCipher.KeySize = size;
    rijndaelCipher.BlockSize = 128;

	byte[] encryptedData = Convert.FromBase64String(text);
    byte[] pwdBytes = System.Text.Encoding.UTF8.GetBytes(password);
    byte[] keyBytes = new byte[bytesNum];
    int len = pwdBytes.Length;
    if (len > keyBytes.Length) len = keyBytes.Length;
    System.Array.Copy(pwdBytes, keyBytes, len);
    rijndaelCipher.Key = keyBytes;
    byte[] ivBytes = System.Text.Encoding.UTF8.GetBytes(iv);
    rijndaelCipher.IV = ivBytes;
    ICryptoTransform transform = rijndaelCipher.CreateDecryptor();
    byte[] plainText = transform.TransformFinalBlock(encryptedData, 0, encryptedData.Length);
    return Encoding.UTF8.GetString(plainText);
}
```
测试运行：
```
请输入要加解密的字符串：时光
随机生成的iv为:8EAD20lxJlHdRcwr
请输入密码:willv
128位AES(CBC)加密:elbuc+G8ycb6ydwVrRLyhQ==

请输入要加解密的字符串：elbuc+G8ycb6ydwVrRLyhQ==
请输入iv:8EAD20lxJlHdRcwr
请输入密码:willv
128位AES(CBC)解密:时光
```