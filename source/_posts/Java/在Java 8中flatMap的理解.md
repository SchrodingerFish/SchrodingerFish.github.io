---
title: 在Java 8中flatMap的理解
date: 2026-02-03 09:24:59
categories:
  - Web
tags:
  - Java8
  - FlatMap
  - Stream
---

## 在Java 8中flatMap的理解

在 Java 8 中，`flatMap` 是一个非常强大的流操作，它结合了 `map` 和 `flat` 的功能。 简单来说，`flatMap`  先使用一个函数对流中的每个元素进行转换（就像 `map` 一样），然后将转换得到的多个流合并（扁平化）成一个流。

**理解 `flatMap` 的关键点:**

1. **`map` 的进阶:** `map` 转换流中的每个元素，但它创建的是一个与原始流相同长度的新流，只是元素不同。  `flatMap` 也进行转换，但允许你将每个元素转换为 **0 个或多个** 新元素。

2. **返回 `Stream`:**  传递给 `flatMap` 的函数必须返回一个 `Stream` 对象（或者 `Optional` 对象等，它们可以被扁平化）。

3. **扁平化 (Flattening):**  `flatMap` 的核心在于“扁平化”。  假设你 `map` 之后得到的是一个 `Stream<Stream<String>>` (一个包含多个 `String` 流的流)。 `flatMap` 会将这些内部流合并成一个单独的 `Stream<String>`。

**工作流程:**

1. **转换:** 对于原始流中的每个元素 `x`，应用一个函数 `f(x)`。 这个函数 `f(x)` 返回一个 `Stream<T>`。
2. **扁平化:** 将所有由 `f(x)` 返回的 `Stream<T>` 合并成一个 `Stream<T>`。

**使用场景:**

`flatMap` 主要用于以下场景：

* **一对多转换:**  当你需要将一个元素转换成零个或多个元素时。  例如，将一个包含句子的字符串列表转换为一个包含所有单词的字符串流。
* **展开嵌套集合:**  当你有一个包含集合的集合，并且想要将它展平成一个包含所有元素的单个集合时。例如，`List<List<String>>`  转换成 `List<String>`。
* **处理 `Optional` 类型:**  当你的 `map` 操作可能返回 `Optional.empty()` 时，使用 `flatMap` 可以避免得到 `Stream<Optional<T>>`，而是直接得到 `Stream<T>`，跳过空的 `Optional`。

**代码示例:**

* **将句子列表转换为单词流:**

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Stream;

public class FlatMapExample {
    public static void main(String[] args) {
        List<String> sentences = Arrays.asList("Hello World", "Java is awesome");

        Stream<String> words = sentences.stream()
                .flatMap(sentence -> Arrays.stream(sentence.split(" "))); // 将每个句子分割成单词流

        words.forEach(System.out::println);
        // Output:
        // Hello
        // World
        // Java
        // is
        // awesome
    }
}
```

在这个例子中：

1. `sentences.stream()` 创建一个 `Stream<String>`，包含两个句子。
2. `flatMap(sentence -> Arrays.stream(sentence.split(" ")))`  将每个句子 `sentence` 分割成单词数组，然后使用 `Arrays.stream()` 将数组转换为 `Stream<String>`。  `flatMap` 将这些单词流合并成一个包含所有单词的 `Stream<String>`。
3. `words.forEach(System.out::println)` 打印每个单词。

* **展开列表的列表:**

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class FlatMapExample2 {
    public static void main(String[] args) {
        List<List<Integer>> listOfLists = Arrays.asList(
                Arrays.asList(1, 2, 3),
                Arrays.asList(4, 5, 6),
                Arrays.asList(7, 8, 9)
        );

        List<Integer> numbers = listOfLists.stream()
                .flatMap(List::stream)  // 将每个 List<Integer> 转换为 Stream<Integer>
                .collect(Collectors.toList());

        System.out.println(numbers); // Output: [1, 2, 3, 4, 5, 6, 7, 8, 9]
    }
}
```

这里，`flatMap(List::stream)`  将 `List<List<Integer>>` 中的每个内部列表 `List<Integer>` 转换为一个 `Stream<Integer>`，然后将所有这些 `Stream<Integer>` 合并成一个 `Stream<Integer>`。

* **处理 `Optional` 的情况:**

```java
import java.util.Arrays;
import java.util.List;
import java.util.Optional;
import java.util.stream.Collectors;

public class FlatMapExample3 {

    static Optional<Integer> maybeGetEven(int num) {
        if (num % 2 == 0) {
            return Optional.of(num);
        } else {
            return Optional.empty();
        }
    }
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

        List<Integer> evenNumbers = numbers.stream()
                .flatMap(num -> maybeGetEven(num).stream()) // 将 Optional<Integer> 转换为 Stream<Integer>, 自动跳过 Optional.empty()
                .collect(Collectors.toList());

        System.out.println(evenNumbers); // Output: [2, 4, 6]
    }
}
```

在这个例子中,  `maybeGetEven` 函数返回 `Optional<Integer>`.  `flatMap(num -> maybeGetEven(num).stream())` 将 `Optional<Integer>` 转换成 `Stream<Integer>`.  `Optional.empty()` 会被转换成一个空的 `Stream`,  `flatMap` 会将它忽略，因此最终得到的流只包含偶数。

**总结:**

`flatMap` 是一个高级的流操作，理解它的关键是认识到它既执行了转换（像 `map` 一样），又执行了扁平化操作。  它在处理嵌套集合、一对多转换以及 `Optional` 类型时非常有用。  只要你理解了它背后的机制，就能在各种场景中灵活运用。 记住，`flatMap` 要求你传入的函数返回一个 `Stream` (或可以转换为 `Stream` 的类型，比如 `Optional`)。

