---
title: Java中常用的加密和解密
date: 2025-06-18 17:07:11
categories:
  - Java
tags:
  - Java
  - 加密
  - 解密
---
Java 中的加密和解密，Java 提供了丰富的加密和解密 API，主要通过 `java.security` 和 `javax.crypto` 包来实现。

**加密和解密的基本概念**

*   **加密（Encryption）：** 将原始数据（称为明文）转换为不可读的形式（称为密文）的过程。
*   **解密（Decryption）：** 将密文转换回原始明文的过程。
*   **密钥（Key）：** 加密和解密过程中使用的秘密信息。密钥的保密性至关重要。

**Java 中的加密和解密算法分类**

Java 支持多种加密算法，主要分为以下几类：

1.  **对称加密算法（Symmetric-key Algorithms）：** 加密和解密使用相同的密钥。速度快，适合加密大量数据。
    *   **DES（Data Encryption Standard）：** 已经过时，不推荐使用。
    *   **AES（Advanced Encryption Standard）：** 目前最常用的对称加密算法，安全性高，性能好。
    *   **DESede（Triple DES）：** DES 的升级版，但效率不高。
    *   **Blowfish/Twofish:**  另一种对称密钥分组密码，由 Bruce Schneier 设计。

2.  **非对称加密算法（Asymmetric-key Algorithms）：** 加密和解密使用不同的密钥，分别是公钥和私钥。公钥可以公开，私钥必须保密。适合加密少量数据，如密钥交换、数字签名。
    *   **RSA：** 最常用的非对称加密算法。
    *   **DSA（Digital Signature Algorithm）：** 用于数字签名。
    *   **ECC（Elliptic Curve Cryptography）：** 基于椭圆曲线数学的加密算法，安全性高，密钥长度短。

3.  **哈希算法（Hash Algorithms）：** 将任意长度的数据转换为固定长度的哈希值（也称为摘要）。哈希算法是单向的，即无法从哈希值还原出原始数据。常用于数据完整性校验、密码存储。
    *   **MD5（Message Digest Algorithm 5）：** 已经不安全，不推荐用于密码存储。
    *   **SHA-1（Secure Hash Algorithm 1）：** 逐渐被淘汰。
    *   **SHA-256/SHA-384/SHA-512：** SHA-2 家族，安全性高，推荐使用。
    *   **bcrypt/scrypt/Argon2：**  专门为密码存储设计的哈希算法，具有抗彩虹表攻击的能力。

**代码示例**

下面是一些 Java 加密和解密的代码示例，我会尽量提供实用且易于理解的例子。

**1. AES 对称加密**

```java
import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import javax.crypto.spec.SecretKeySpec;
import java.security.NoSuchAlgorithmException;
import java.security.SecureRandom;
import java.util.Base64;

public class AESExample {

    public static void main(String[] args) throws Exception {
        String plainText = "This is a secret message.";

        // 1. 生成密钥 (如果已经有密钥，则跳过此步骤)
        SecretKey secretKey = generateKey(); // 或者使用已有的密钥 byte[] key = ...; SecretKey secretKey = new SecretKeySpec(key, "AES");

        // 2. 加密
        String cipherText = encrypt(plainText, secretKey);
        System.out.println("Encrypted text: " + cipherText);

        // 3. 解密
        String decryptedText = decrypt(cipherText, secretKey);
        System.out.println("Decrypted text: " + decryptedText);
    }

    public static SecretKey generateKey() throws NoSuchAlgorithmException {
        KeyGenerator keyGenerator = KeyGenerator.getInstance("AES");
        keyGenerator.init(256, new SecureRandom()); // 可以选择 128, 192, 256 位密钥
        return keyGenerator.generateKey();
    }

    public static String encrypt(String plainText, SecretKey secretKey) throws Exception {
        Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding"); //  更安全的模式：AES/CBC/PKCS5Padding ，需要使用 IvParameterSpec
        cipher.init(Cipher.ENCRYPT_MODE, secretKey);
        byte[] cipherBytes = cipher.doFinal(plainText.getBytes("UTF-8"));
        return Base64.getEncoder().encodeToString(cipherBytes);
    }

    public static String decrypt(String cipherText, SecretKey secretKey) throws Exception {
        Cipher cipher = Cipher.getInstance("AES/ECB/PKCS5Padding"); //  与加密时相同的模式
        cipher.init(Cipher.DECRYPT_MODE, secretKey);
        byte[] plainBytes = cipher.doFinal(Base64.getDecoder().decode(cipherText));
        return new String(plainBytes, "UTF-8");
    }
}
```

*   **`generateKey()`:**  生成一个 AES 密钥。  `KeyGenerator` 用于创建密钥。 `SecureRandom` 用于生成安全的随机数，确保密钥的随机性。
*   **`encrypt()`:** 使用 AES 算法加密数据。  `Cipher.getInstance("AES/ECB/PKCS5Padding")` 创建一个 Cipher 对象，指定加密算法和填充模式。 `cipher.init(Cipher.ENCRYPT_MODE, secretKey)` 初始化 Cipher 对象，设置加密模式和密钥。 `cipher.doFinal()` 执行加密操作。 `Base64.getEncoder().encodeToString()` 将加密后的字节数组转换为 Base64 编码的字符串，方便传输和存储。
*   **`decrypt()`:** 使用 AES 算法解密数据。 流程与加密类似，只是 `cipher.init()` 设置为解密模式 (`Cipher.DECRYPT_MODE`)。

**重要提示：**

*   **密钥安全：**  绝对不要将密钥硬编码到代码中。  应该使用密钥管理系统、环境变量、配置文件等安全的方式存储密钥。
*   **加密模式和填充：**  `AES/ECB/PKCS5Padding`  只是一个简单的例子。  实际应用中，`AES/CBC/PKCS5Padding`  或  `AES/GCM/NoPadding`  等模式更安全。 CBC 模式需要使用 `IvParameterSpec`  来初始化 Cipher 对象，以增加安全性。 GCM 模式提供认证加密，可以检测数据是否被篡改。
*   **异常处理：**  代码中需要处理可能抛出的异常，例如 `NoSuchAlgorithmException`, `InvalidKeyException`, `IllegalBlockSizeException`, `BadPaddingException` 等。

**2. RSA 非对称加密**

```java
import java.security.*;
import java.security.spec.PKCS8EncodedKeySpec;
import java.security.spec.X509EncodedKeySpec;
import java.util.Base64;

import javax.crypto.Cipher;

public class RSAExample {

    public static void main(String[] args) throws Exception {
        String plainText = "This is a secret message for RSA.";

        // 1. 生成密钥对
        KeyPair keyPair = generateKeyPair();
        PublicKey publicKey = keyPair.getPublic();
        PrivateKey privateKey = keyPair.getPrivate();

        // 2. 加密 (使用公钥)
        String cipherText = encrypt(plainText, publicKey);
        System.out.println("Encrypted text: " + cipherText);

        // 3. 解密 (使用私钥)
        String decryptedText = decrypt(cipherText, privateKey);
        System.out.println("Decrypted text: " + decryptedText);

        // 可选: 保存公钥和私钥到文件 (参考下面的代码)
    }

    public static KeyPair generateKeyPair() throws Exception {
        KeyPairGenerator generator = KeyPairGenerator.getInstance("RSA");
        generator.initialize(2048); // 密钥长度，越大越安全，但性能越低
        return generator.generateKeyPair();
    }

    public static String encrypt(String plainText, PublicKey publicKey) throws Exception {
        Cipher cipher = Cipher.getInstance("RSA/ECB/PKCS1Padding");
        cipher.init(Cipher.ENCRYPT_MODE, publicKey);
        byte[] cipherBytes = cipher.doFinal(plainText.getBytes("UTF-8"));
        return Base64.getEncoder().encodeToString(cipherBytes);
    }

    public static String decrypt(String cipherText, PrivateKey privateKey) throws Exception {
        Cipher cipher = Cipher.getInstance("RSA/ECB/PKCS1Padding");
        cipher.init(Cipher.DECRYPT_MODE, privateKey);
        byte[] plainBytes = cipher.doFinal(Base64.getDecoder().decode(cipherText));
        return new String(plainBytes, "UTF-8");
    }


    // 以下是保存和加载密钥的示例代码 (可选)

    // 保存私钥到字符串
    public static String savePrivateKey(PrivateKey privateKey) throws Exception {
        KeyFactory fact = KeyFactory.getInstance("RSA");
        PKCS8EncodedKeySpec spec = fact.getKeySpec(privateKey, PKCS8EncodedKeySpec.class);
        byte[] packed = spec.getEncoded();
        String key64 = Base64.getEncoder().encodeToString(packed);
        return key64;
    }

    // 从字符串加载私钥
    public static PrivateKey loadPrivateKey(String key64) throws Exception {
        byte[] encKey = Base64.getDecoder().decode(key64);
        PKCS8EncodedKeySpec spec = new PKCS8EncodedKeySpec(encKey);
        KeyFactory kf = KeyFactory.getInstance("RSA");
        return kf.generatePrivate(spec);
    }

    // 保存公钥到字符串
    public static String savePublicKey(PublicKey publicKey) throws Exception {
        KeyFactory fact = KeyFactory.getInstance("RSA");
        X509EncodedKeySpec spec = fact.getKeySpec(publicKey, X509EncodedKeySpec.class);
        return Base64.getEncoder().encodeToString(spec.getEncoded());
    }

    // 从字符串加载公钥
    public static PublicKey loadPublicKey(String stored) throws Exception {
        byte[] data = Base64.getDecoder().decode(stored);
        X509EncodedKeySpec spec = new X509EncodedKeySpec(data);
        KeyFactory fact = KeyFactory.getInstance("RSA");
        return fact.generatePublic(spec);
    }
}
```

*   **`generateKeyPair()`:** 生成 RSA 密钥对（公钥和私钥）。
*   **`encrypt()`:** 使用公钥加密数据。
*   **`decrypt()`:** 使用私钥解密数据。
*   **密钥长度：**  RSA 密钥长度通常为 2048 位或更高。  密钥长度越长，安全性越高，但性能越低。
* **密钥的存储:** 上面的代码展示了将公钥和私钥存储到字符串，你可以将其保存到文件、数据库等。 注意**私钥的存储一定要安全**.

**3. SHA-256 哈希算法**

```java
import java.nio.charset.StandardCharsets;
import java.security.MessageDigest;
import java.security.NoSuchAlgorithmException;
import java.util.Base64;

public class SHA256Example {

    public static void main(String[] args) throws NoSuchAlgorithmException {
        String data = "This is a string to hash.";

        String hash = hashSHA256(data);
        System.out.println("SHA-256 hash: " + hash);
    }

    public static String hashSHA256(String data) throws NoSuchAlgorithmException {
        MessageDigest digest = MessageDigest.getInstance("SHA-256");
        byte[] hashBytes = digest.digest(data.getBytes(StandardCharsets.UTF_8));
        return Base64.getEncoder().encodeToString(hashBytes); // 使用 Base64 编码，方便显示和存储
        // 或者使用十六进制编码
        // return bytesToHex(hashBytes);
    }

    // 将字节数组转换为十六进制字符串
    private static String bytesToHex(byte[] hash) {
        StringBuilder hexString = new StringBuilder(2 * hash.length);
        for (int i = 0; i < hash.length; i++) {
            String hex = Integer.toHexString(0xff & hash[i]);
            if (hex.length() == 1) {
                hexString.append('0');
            }
            hexString.append(hex);
        }
        return hexString.toString();
    }
}
```

*   **`hashSHA256()`:**  计算 SHA-256 哈希值。
*   **单向性：**  无法从哈希值还原出原始数据。
*   **数据完整性：**  如果数据被篡改，哈希值会发生变化。
*   **密码存储：**  永远不要直接存储用户的密码。  应该使用加盐哈希（salted hash）算法，例如 bcrypt, scrypt, Argon2 等。

**4. 使用 bcrypt 安全存储密码 (需要添加依赖)**

首先，你需要添加 bcrypt 的依赖。 如果你使用 Maven，可以在 `pom.xml` 文件中添加：

```xml
<dependency>
    <groupId>org.mindrot</groupId>
    <artifactId>jbcrypt</artifactId>
    <version>0.4</version>
</dependency>
```

然后，可以使用下面的代码：

```java
import org.mindrot.jbcrypt.BCrypt;

public class BCryptExample {

    public static void main(String[] args) {
        String password = "mySecretPassword";

        // 1. 加盐哈希密码
        String hashedPassword = hashPassword(password);
        System.out.println("Hashed password: " + hashedPassword);

        // 2. 验证密码
        boolean passwordMatches = verifyPassword(password, hashedPassword);
        System.out.println("Password matches: " + passwordMatches);
    }

    public static String hashPassword(String password) {
        // BCrypt 会自动生成 salt
        return BCrypt.hashpw(password, BCrypt.gensalt());
    }

    public static boolean verifyPassword(String password, String hashedPassword) {
        return BCrypt.checkpw(password, hashedPassword);
    }
}
```

*   **`BCrypt.hashpw()`:**  对密码进行加盐哈希。  `BCrypt.gensalt()`  自动生成随机盐值。
*   **`BCrypt.checkpw()`:**  验证密码是否与哈希值匹配。
*   **加盐：**  盐值是随机生成的字符串，与密码组合后进行哈希。  加盐可以防止彩虹表攻击。

**总结**

Java 提供了强大的加密和解密 API，可以满足各种安全需求。  在实际应用中，需要根据具体的场景选择合适的算法和模式，并注意密钥的安全管理。  永远不要自己实现加密算法，应该使用经过安全审计的库。

