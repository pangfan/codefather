## 什么是 Java 中的不可变类？
> 八股文一网打尽，更多面试题请看[程序员面试刷题神器 - 面试鸭](https://www.mianshiya.com/)

## 回答重点

不可变类是指在创建后其状态（对象的字段）无法被修改的类。一旦对象被创建，它的所有属性都不能被更改。这种类的实例在整个生命周期内保持不变。

**关键特征**：
  1. 声明类为 `final`，防止子类继承。
  2. 类的所有字段都是`private` 和 `final`，确保它们在初始化后不能被更改。
  3. 通过构造函数初始化所有字段。
  4. 不提供任何修改对象状态的方法（如 `setter` 方法）。
  5. 如果类包含可变对象的引用，确保这些引用在对象外部无法被修改。例如 getter 方法中返回对象的副本（new 一个新的对象）来保护可变对象。

Java 中的经典不可变类有：`String`、`Integer`、`BigDecimal`、`LocalDate` 等。

## 扩展知识

### 不可变类的优缺点

**优点**：
  1. **线程安全**：由于不可变对象的状态不能被修改，它们天生是线程安全的，在并发环境中无需同步。
  2. **缓存友好**：不可变对象可以安全地被缓存和共享，如 `String` 的字符串常量池。
  3. **防止状态不一致**：不可变类可以有效避免因意外修改对象状态而导致的不一致问题。
  
**缺点**：
  1. **性能问题**：不可变对象需要在每次状态变化时创建新的对象，这可能会导致性能开销，尤其是对于大规模对象或频繁修改的场景（例如 String 频繁拼接）。
  

### 举例 String

String 就是典型的不可变类，当你创建一个 String 对象之后，这个对象就无法被修改。 

因为无法被修改，所以像执行s += "a"; 这样的方法，其实返回的是一个新建的 String 对象，老的 s 指向的对象不会发生变化，只是 s 的引用指向了新的对象而已。

所以不要在字符串拼接频繁的场景使用 + 来拼接，因为这样会频繁的创建对象。

不可变类的好处就是安全，因为知晓这个对象不可能会被修改，因此可以放心大胆的用，在多线程环境下也是线程安全的。

### 如何实现一个不可变类？

具体按照回答重点内的关键特征实现就行了。我们来一起看下 String 的设计。

String 类用 final 修饰，表示无法被继承。

<img src="https://pic.code-nav.cn/mianshiya/question_picture/1772087337535152129/image-20210228112501169_mianshiya.png" alt="" width="100%" />

String 本质是一个 char 数组，然后用 final 修饰，不过 final 限制不了数组内部的数据，所以这还不够。

所以 value 是用 private 修饰的，并且没有暴露出 set 方法，这样外部其实就接触不到 value 所以无法修改。

当然还是有修改的需求，比如 replace 方法，所以这时候就需要返回一个新对象来作为结果。

<img src="https://pic.code-nav.cn/mianshiya/question_picture/1772087337535152129/image-20210228112521363_mianshiya.png" alt="" width="100%" />

总结一下就是私有化变量，然后不要暴露 set 方法，即使有修改的需求也是返回一个新对象。


> 八股文一网打尽，更多面试题请看[程序员面试刷题神器 - 面试鸭](https://www.mianshiya.com/)