---
title: Argon2id密码哈希
date: 2025-06-01 10:55:11
categories:
  - Java
tags:
  - Java
  - Argon2id
  - 密码
  - 哈希
---

Argon2 被认为是密码哈希的最佳选择，主要归功于以下几个关键原因：

1. **强大的抗攻击性:**

    * **抗暴力破解攻击:** Argon2 旨在通过增加计算成本来抵抗暴力破解攻击。 攻击者需要消耗大量的计算资源才能破解密码，这使得攻击变得更加昂贵和耗时。
    * **抗彩虹表攻击:** 像 bcrypt 一样，Argon2 使用盐来防止彩虹表攻击。 每个密码都使用一个唯一的盐值，这使得预先计算哈希值并将其存储在彩虹表中变得不可能。
    * **抗侧信道攻击:** Argon2 的设计考虑了侧信道攻击，例如计时攻击。 它使用固定的执行时间，使得攻击者难以通过测量执行时间来推断密码的信息。

2. **内存硬度 (Memory Hardness):**
    * **抵抗 ASIC 和 GPU 攻击:** Argon2 具有内存硬度，这意味着它需要大量的内存才能高效地运行。 这使得使用定制硬件（例如 ASIC 芯片）或 GPU 进行并行计算的攻击变得更加困难和昂贵。 其他哈希算法，例如 bcrypt，在内存硬度方面不如 Argon2。

3. **三种变体满足不同需求:**

    * **Argon2d:** 优化用于抵抗 GPU 破解（数据依赖）。它使用数据依赖的内存访问，使其对 GPU 破解具有很强的抵抗力。然而，它对侧信道攻击的抵抗力较弱。
    * **Argon2i:** 优化用于抵抗侧信道攻击（数据独立）。它使用数据独立的内存访问，使其对侧信道攻击具有很强的抵抗力。但是，它的 GPU 破解抗性不如 Argon2d。
    * **Argon2id:** 一种混合方法，结合了 Argon2d 和 Argon2i 的优点。它在抵抗 GPU 破解和侧信道攻击之间取得了平衡。通常建议使用 Argon2id，因为它在大多数情况下提供了最佳的安全性。

4. **密码哈希竞赛的胜利者:**

    * Argon2 是密码哈希竞赛（Password Hashing Competition）的获胜者。 该竞赛旨在寻找下一代密码哈希算法。 Argon2 在安全性、性能和实用性方面都表现出色，最终被选为获胜者。

5. **可配置性:**

    * Argon2 允许调整多个参数，例如内存使用量、迭代次数和并行度。 这使得你可以根据你的安全需求和硬件资源优化 Argon2 的性能。

6. **标准化和广泛支持:**

    * Argon2 已经被标准化为 RFC 9106。 这意味着它已经得到了广泛的认可和支持。 许多编程语言和库都提供了 Argon2 的实现。

7. **安全性评估:**

    * Argon2 经过了广泛的安全评估，并且没有发现严重的漏洞。 这表明 Argon2 是一种可靠的密码哈希算法。

**与其他算法的比较:**

* **与 bcrypt 相比:** Argon2 在内存硬度方面优于 bcrypt。 这使得 Argon2 对 ASIC 和 GPU 攻击的抵抗力更强。
* **与 scrypt 相比:** Argon2 在某些情况下可能比 scrypt 更快。 此外，Argon2id 提供了一种更平衡的安全性方法，兼顾了 GPU 破解和侧信道攻击的抵抗力。
* **与 PBKDF2 相比:** Argon2 在内存硬度方面远优于 PBKDF2。 这使得 Argon2 对 ASIC 和 GPU 攻击的抵抗力更强。

**总结:**

由于其强大的抗攻击性、内存硬度、三种变体、密码哈希竞赛的胜利者地位、可配置性、标准化和广泛支持以及安全性评估，Argon2 是目前密码哈希的最佳选择。  它能够有效地抵抗各种攻击，并且可以根据你的安全需求和硬件资源进行优化。  如果你正在寻找一种安全可靠的密码哈希算法，Argon2 是一个很好的选择。


使用Java实现 bcrypt 和 Argon2 加盐哈希的示例。 请注意，你需要添加相应的依赖到你的项目中。

**1. bcrypt (使用 jBCrypt 库)**

* **添加依赖 (Maven):**

```xml
<dependency>
    <groupId>org.mindrot</groupId>
    <artifactId>jbcrypt</artifactId>
    <version>0.4</version>
</dependency>
```

* **代码示例:**

```java
import org.mindrot.jbcrypt.BCrypt;

public class BCryptExample {

    public static void main(String[] args) {
        String password = "mySecretPassword";

        // 1. 加盐并哈希密码
        String hashedPassword = BCrypt.hashpw(password, BCrypt.gensalt());
        System.out.println("Hashed password (bcrypt): " + hashedPassword);

        // 2. 验证密码
        boolean passwordMatches = BCrypt.checkpw(password, hashedPassword);
        System.out.println("Password matches: " + passwordMatches);
    }
}
```

**说明:**

* `BCrypt.gensalt()`  自动生成一个随机盐。  bcrypt 算法内置了盐的处理，不需要单独存储盐。
* `BCrypt.hashpw(password, salt)`  使用指定的盐对密码进行哈希。
* `BCrypt.checkpw(candidatePassword, hashedPassword)`  验证给定的密码是否与哈希密码匹配。  `checkpw`  会自动从哈希密码中提取盐，并使用相同的盐对 `candidatePassword` 进行哈希，然后比较结果。

**2. Argon2 (使用 Argon2id,  使用 de.mkammerer.argon2-jvm 库)**

* **添加依赖 (Maven):**

```xml
<dependency>
    <groupId>de.mkammerer</groupId>
    <artifactId>argon2-jvm</artifactId>
    <version>2.7</version>
</dependency>
```

* **代码示例:**

```java
import de.mkammerer.argon2.Argon2;
import de.mkammerer.argon2.Argon2Factory;

public class Argon2Example {

    public static void main(String[] args) {
        String password = "mySecretPassword";

        // 1. 创建 Argon2 实例
        Argon2 argon2 = Argon2Factory.create(Argon2Factory.Argon2Types.ARGON2id);  // 推荐使用 Argon2id

        try {
            // 2. 加盐并哈希密码
            String hashedPassword = argon2.hash(4, 65536, 1, password.toCharArray()); //iterations, memory, parallelism
            System.out.println("Hashed password (Argon2id): " + hashedPassword);

            // 3. 验证密码
            boolean passwordMatches = argon2.verify(hashedPassword, password.toCharArray());
            System.out.println("Password matches: " + passwordMatches);

        } finally {
            // 即使发生异常，也要擦除密码
            argon2.wipeArray(password.toCharArray());
            // 4. 关闭 Argon2 实例 (重要!)
            argon2.close();
        }
    }
}
```

**说明:**

* `Argon2Factory.create(Argon2Factory.Argon2Types.ARGON2id)` 创建 Argon2 实例，推荐使用 `ARGON2id` 变体，因为它结合了 `ARGON2d` 和 `ARGON2i` 的优点。
* `argon2.hash(iterations, memory, parallelism, password.toCharArray())` 使用指定的参数对密码进行哈希。  `iterations` (迭代次数), `memory` (内存大小，KB), 和 `parallelism` (并行度) 是 Argon2 的关键参数，用于控制哈希的强度和计算成本。  **请根据你的安全需求和硬件资源调整这些参数。**  较高的值会增加安全性，但也会增加计算时间。  常见的设置是  `iterations = 4`, `memory = 65536` (64MB),  `parallelism = 1`。
* `argon2.verify(hashedPassword, password.toCharArray())`  验证给定的密码是否与哈希密码匹配。  与 bcrypt 类似，Argon2 将盐存储在哈希字符串中，所以 `verify` 方法会自动提取盐并进行验证。
* **重要:**  `argon2.close()`  必须在不再需要 `Argon2` 实例时调用，以释放资源。 最好在 `finally` 块中调用，以确保即使发生异常也能正确释放资源。
* **安全最佳实践:**  `argon2.wipeArray(password.toCharArray())` 用于在内存中擦除原始密码，防止密码泄露。  在不再需要密码时，始终执行此操作。

**选择哪个算法？**

* **Argon2:** 通常被认为是更现代、更安全的哈希算法，并且是密码哈希竞赛的获胜者。  它提供了更好的抗侧信道攻击能力，并可以更好地利用现代硬件的并行性。  如果可以，推荐使用 Argon2id。

* **bcrypt:**  仍然是一种安全的选择，并且被广泛使用。 它在安全性方面略逊于 Argon2，但计算成本仍然很高，足以阻止大多数暴力破解攻击。

**参数调整:**

对于 Argon2，调整 `iterations`, `memory`, 和 `parallelism` 参数非常重要。  你应该根据你的安全需求和硬件资源进行调整。  一个好的起点是 `iterations = 4`, `memory = 65536`, `parallelism = 1`，然后逐渐增加这些值，直到哈希时间达到可接受的范围。  目标是找到一个平衡点，既能提供足够的安全性，又不会对用户体验造成太大的影响。

**总结:**

这两个例子都演示了如何使用加盐哈希来安全地存储密码。  选择 bcrypt 或 Argon2 取决于你的具体需求和偏好，但强烈建议避免使用 MD5 或 SHA-1 等过时的哈希算法，尤其是在存储密码时。  并始终确保擦除内存中的原始密码，以提高安全性。

## Argon2id 密码哈希的 Java 实现

```java
import de.mkammerer.argon2.Argon2;
import de.mkammerer.argon2.Argon2Factory;
import de.mkammerer.argon2.Argon2Helper;
import java.util.Arrays;

public class Argon2idExample {

    private static final int ITERATIONS = 2;  // 迭代次数，根据性能调整
    private static final int MEMORY = 65536;   // 内存使用量 (KB)， 根据性能调整，但要记住服务器的内存资源
    private static final int PARALLELISM = 1;  // 并行度， 根据 CPU 核心数调整

    public static String hashPassword(String password) {
        Argon2 argon2 = Argon2Factory.create(Argon2Factory.Argon2Types.ARGON2id);
        try {
            return argon2.hash(ITERATIONS, MEMORY, PARALLELISM, password.toCharArray());
        } finally {
            // 擦除内存中的密码
            if (password != null) {
                Arrays.fill(password.toCharArray(), ' '); // 用空格覆盖
            }
            argon2.wipeArray(password.toCharArray()); // 更安全地擦除密码，确保没有残留信息
            argon2.erase(); // 释放 Argon2 实例
        }
    }

    public static boolean verifyPassword(String password, String hashedPassword) {
        Argon2 argon2 = Argon2Factory.create(Argon2Factory.Argon2Types.ARGON2id);
        try {
            return argon2.verify(hashedPassword, password.toCharArray());
        } finally {
            // 擦除内存中的密码
            if (password != null) {
                Arrays.fill(password.toCharArray(), ' '); // 用空格覆盖
            }
            argon2.wipeArray(password.toCharArray()); // 更安全地擦除密码，确保没有残留信息
            argon2.erase(); // 释放 Argon2 实例
        }
    }

    public static void main(String[] args) {
        String password = "mySecretPassword";

        // Hash the password
        String hashedPassword = hashPassword(password);
        System.out.println("Hashed password: " + hashedPassword);

        // Verify the password
        boolean valid = verifyPassword("mySecretPassword", hashedPassword);
        System.out.println("Password is valid: " + valid);  // Output: true

        boolean invalid = verifyPassword("wrongPassword", hashedPassword);
        System.out.println("Password is valid: " + invalid); // Output: false

        // Check if native library is available
        System.out.println("Native library available: " + Argon2Helper.isNativeAvailable());
    }
}
```

关键点说明：

* **引入依赖:**  确保在你的项目中添加 `de.mkammerer.argon2:argon2-jvm` 依赖。  在 Maven 项目中，添加到 `pom.xml`：

  ```xml
  <dependency>
      <groupId>de.mkammerer.argon2</groupId>
      <artifactId>argon2-jvm</artifactId>
      <version>2.7</version> <!-- 检查最新的版本 -->
  </dependency>
  ```
  在 Gradle 项目中，添加到 `build.gradle`：

  ```gradle
  implementation 'de.mkammerer.argon2:argon2-jvm:2.7' // 检查最新的版本
  ```

* **选择 Argon2id:**  `Argon2Factory.create(Argon2Factory.Argon2Types.ARGON2id)`  确保你使用的是推荐的 `Argon2id` 变体，它兼顾了 GPU 抗性和侧信道抗性。

* **调整参数:**  `ITERATIONS`、`MEMORY`、`PARALLELISM`  这些参数控制了 Argon2 的强度。  你应该根据你的服务器的资源和所需的安全性级别调整这些参数。  增加这些值会提高安全性，但也会增加计算时间。

    * `ITERATIONS`:  迭代次数。 增加迭代次数会增加计算成本。 通常从 2 开始，并根据性能调整。
    * `MEMORY`:  内存使用量，以 KB 为单位。  较大的内存使用量会使 GPU 破解更加困难。 常见的取值是 65536 (64MB)。
    * `PARALLELISM`:  并行线程数。  这应该与你的 CPU 核心数相匹配。  如果你的 CPU 有多个核心，增加并行度可以提高性能。  但是，不要超过你的 CPU 核心数。

* **擦除密码:**  在哈希和验证后，使用 `argon2.wipeArray(password.toCharArray())` 安全地擦除内存中的密码。  这可以防止攻击者从内存中恢复密码。 在 `try-finally` 块中使用确保在任何情况下都会执行。 此外，使用空格填充密码字符数组是一种额外的安全措施。

* **处理异常:**  代码没有显式地处理异常，但你应该在生产环境中添加适当的异常处理。

* **性能考虑:** Argon2  比 bcrypt 更耗费资源。  在部署到生产环境之前，务必测试你的代码，以确保它不会对你的应用程序的性能产生负面影响。

* **检查 Native Library:** `Argon2Helper.isNativeAvailable()`可以检查是否可以使用优化的本地库。  如果可用，这将显著提高性能。 如果返回false，Argon2-JVM会使用纯Java实现，性能较低。  确保安装了正确的本地库以获得最佳性能。 (参考下文)

**如何安装 Argon2 的 Native Library (可选但强烈推荐，提升性能)**

要获得最佳性能，你应该安装 Argon2 的本地库。 安装方法取决于你的操作系统。

* **Linux (Debian/Ubuntu):**

  ```bash
  sudo apt-get update
  sudo apt-get install libargon2-1
  ```

* **Linux (Fedora/CentOS/RHEL):**

  ```bash
  sudo yum install libargon2
  ```

* **macOS (Homebrew):**

  ```bash
  brew install libargon2
  ```

* **Windows:**

    1.  下载预编译的库：从  [https://github.com/P-H-C/phc-winner-argon2/releases](https://github.com/P-H-C/phc-winner-argon2/releases)  下载适用于 Windows 的 ZIP 文件。 选择与你的 Java 版本 (32 位或 64 位) 匹配的版本。
    2.  提取 ZIP 文件。
    3.  将 `argon2.dll` 文件复制到你的 Java 项目的根目录，或者添加到你的系统路径。  更推荐添加到项目根目录。

**重要安全提示：**

* **定期评估参数:**  随着计算能力的提高，你可能需要定期评估和增加 Argon2 的参数，以保持安全性。
* **避免手动实现:**  始终使用信誉良好且维护良好的库来实现密码哈希。 避免尝试自己实现密码哈希算法，因为这很容易出错。
* **理解参数的影响:** 花时间理解 `ITERATIONS`, `MEMORY`, 和 `PARALLELISM` 参数如何影响安全性以及性能。  根据你的具体需求进行调整。
* **监控性能:** 在生产环境中部署后，监控你的应用程序的性能，以确保密码哈希不会导致任何问题。

遵循这些指南将帮助你安全有效地使用 Argon2id 进行密码哈希。