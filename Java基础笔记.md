# Java 基础复习笔记

> 用途：整理基础知识、回顾代码、查找易错点，为后续后端学习打基础。  
> 整理日期：2026-11-02。  
> 示例基准：Java 17，使用传统的类与 `main` 方法写法；大部分语法也适用于 Java 8。  
> 范围：Java 语言、面向对象、常用集合和异常；Maven、Spring Boot、HTTP、数据库另行整理。

[TOC]

## 0. 这份笔记怎么用

### 0.1 笔记内容

在继续学习后的内容之前，回头整理 Java 基础，作为复习和总结。

### 0.2 建议的复习顺序

1. 先串起来：源代码 → 编译 → JVM 运行。
2. 再理解：变量 → 方法 → 类 → 对象 → 构造方法。
3. 接着区分：实例成员与 `static`、封装与访问权限。
4. 然后掌握：继承、重写、多态、抽象类和接口。
5. 最后串联：集合、`equals/hashCode`、异常、自定义异常。
6. 用“学生信息管理”练习把知识点放在一起。

在 Typora 中直接打开本文件即可。`[TOC]` 用于显示目录；右侧大纲也能按标题定位。代码块标注了语言，可以保留语法高亮。

### 0.3 代码示例约定

- 标注 **“完整示例：文件名.java”** 的代码可以单独保存为对应文件。
- 不同完整示例分别练习，不要把所有 `public` 类放进同一个文件。
- 标注 **“局部片段”** 的代码需要放进已有类或方法，不能直接独立运行。
- 示例采用单文件、默认包写法，先把语言本身弄明白。
- 注释中的“编译错误”“运行时异常”用于说明反例，不表示应该原样执行错误代码。

---

## 1. Java 怎么从代码变成程序

### 1.1 JDK、JVM 与运行环境

| 名称 | 作用 | 初学时怎么记 |
| --- | --- | --- |
| JDK | 开发工具包，包含编译器、运行所需组件和其他工具 | 写、编译、运行 Java 所用的工具包 |
| `javac` | Java 编译器 | 把 `.java` 源文件编译成 `.class` 字节码文件 |
| JVM | Java 虚拟机 | 加载并执行字节码 |
| Java 标准类库 | 提供字符串、集合、文件等通用能力 | 可以直接使用的基础工具 |
| JRE | Java 运行环境这一概念，通常指 JVM 加上运行所需类库等 | 运行程序需要的环境 |

不要把 JRE 理解成“每个 JDK 安装目录里一定有一个 `jre` 文件夹”。现代 JDK 的发行和目录组织方式可能不同。学习开发时，先确保安装的是 JDK，并能使用 `java` 和 `javac`。

```text
HelloJava.java
      │ javac 编译
      ▼
HelloJava.class（字节码）
      │ java 启动 JVM、加载类
      ▼
程序执行，调用标准类库，输出结果
```

**跨平台的含义：**同一份兼容的字节码，可以由不同平台上的相应 JVM 执行。它仍受 Java 版本、使用的类库以及本地平台依赖等条件约束。

### 1.2 第一个完整程序

**完整示例：HelloJava.java**

```java
public class HelloJava {
    public static void main(String[] args) {
        System.out.println("你好，Java！");
    }
}
```

逐个理解：

| 代码 | 含义 |
| --- | --- |
| `public class HelloJava` | 声明一个公开类，名字为 `HelloJava` |
| `main` | 本文采用的传统程序入口方法 |
| `public` | 方法可被外部访问 |
| `static` | 属于类，启动时无需先创建该类对象 |
| `void` | 方法不返回结果 |
| `String[] args` | 接收命令行传入的字符串参数 |
| `System.out.println(...)` | 打印内容，并在末尾换行 |
| `;` | 结束一条语句 |

在保存文件的目录中执行：

```bash
javac -encoding UTF-8 HelloJava.java
java HelloJava
```

第二行写的是**类名**，不用写成 `java HelloJava.class`。

Java 11 起也支持简单的源文件启动方式，例如 `java HelloJava.java`。初学时先理解上面分开的编译、运行过程。

### 1.3 三种常见问题

| 阶段 | 例子 | 怎么理解 |
| --- | --- | --- |
| 编译时 | 少分号、变量未定义、类型不兼容 | 编译器发现代码不符合规则 |
| 运行时 | 空指针、下标越界、字符串转数字失败 | 编译通过，执行过程中出现异常 |
| 逻辑问题 | 总分除以人数时误用了错误人数 | 程序能执行，结果却不符合需求 |

**易错点：**使用 `javac` 编译本文示例时，公开顶层类的名字应与文件名一致；Java 区分大小写，`Student` 和 `student` 不是同一个名字。

---

## 2. 衔接补充：变量、流程、数组与方法

### 2.1 基本类型与引用类型

Java 有 8 种基本类型：

| 类型 | 常见用途 | 示例 |
| --- | --- | --- |
| `byte` | 小范围整数、字节数据 | `byte level = 1;` |
| `short` | 小范围整数 | `short year = 2026;` |
| `int` | 常用整数 | `int age = 20;` |
| `long` | 范围更大的整数 | `long count = 10000000000L;` |
| `float` | 单精度浮点数 | `float rate = 0.5F;` |
| `double` | 常用浮点数 | `double score = 88.5;` |
| `char` | 一个 UTF-16 代码单元 | `char grade = 'A';` |
| `boolean` | 真或假 | `boolean passed = true;` |

`char` 不一定能独自表示完整的 Unicode 字符，例如部分表情需要两个 `char`。

引用类型包括类、接口、数组等，例如 `String`、`Student`、`int[]`。引用变量可以持有指向对象的引用，也可以是 `null`。

**局部片段：放在方法中。**

```java
int age = 20;
String name = "小张";
int[] scores = {80, 90, 100};
String missingName = null;
```

`String` 是类，不是基本类型。`null` 不能赋给 `int` 这样的基本类型变量。

### 2.2 运算、分支、循环和数组

**完整示例：BasicsDemo.java**

```java
public class BasicsDemo {
    public static void main(String[] args) {
        int[] scores = {80, 90, 100};
        int total = 0;

        for (int score : scores) {
            total += score;
        }

        double average = (double) total / scores.length;
        System.out.println("总分：" + total);
        System.out.println("平均分：" + average);

        if (average >= 60) {
            System.out.println("及格");
        } else {
            System.out.println("未及格");
        }

        System.out.println("最大值：" + max(8, 12));
    }

    static int max(int a, int b) {
        return a > b ? a : b;
    }
}
```

这里的 `for (int score : scores)` 是增强 `for`，依次取出数组中的元素。若需要下标，可以写 `for (int i = 0; i < scores.length; i++)`。

**易错点：**

- `5 / 2` 得到 `2`；`5.0 / 2` 得到 `2.5`。
- `double result = 5 / 2;` 得到 `2.0`，因为整数除法已经先算完。
- 数组下标从 `0` 开始，最后一个下标为 `length - 1`。
- 数组长度创建后不能改变；可变元素数量的需求通常交给集合。
- 数组用 `array.length`；字符串用 `text.length()`；集合用 `list.size()`。
- 局部变量需要先赋值再使用；字段有默认值，例如 `int` 为 `0`、引用为 `null`。
- `&&`、`||` 具有短路特性。例如 `name != null && !name.isEmpty()` 会先判断是否为空引用。

### 2.3 方法：输入、处理与输出

```text
返回类型 方法名(参数类型 参数名, ...) {
    方法体
    return 返回值;
}
```

- **形参**：定义方法时声明的参数，例如 `max(int a, int b)` 中的 `a`、`b`。
- **实参**：调用时传入的值，例如 `max(8, 12)` 中的 `8`、`12`。
- `void` 方法不返回值，可以使用 `return;` 提前结束。
- 有返回类型的方法，在正常结束前需要返回兼容类型的值。

### 2.4 Java 始终按值传递参数

基本类型传递的是值的副本；引用类型传递的是**引用值的副本**。

**完整示例：PassByValueDemo.java**

```java
public class PassByValueDemo {
    static class Box {
        int value;
    }

    static void changeNumber(int number) {
        number = 99;
    }

    static void changeBox(Box box) {
        box.value = 99;
    }

    static void replaceBox(Box box) {
        box = new Box();
        box.value = 500;
    }

    public static void main(String[] args) {
        int number = 1;
        changeNumber(number);
        System.out.println(number); // 预期：1

        Box box = new Box();
        box.value = 1;
        changeBox(box);
        System.out.println(box.value); // 预期：99

        replaceBox(box);
        System.out.println(box.value); // 预期：99
    }
}
```

`changeBox` 通过复制来的引用找到了同一个对象，所以能修改对象字段。`replaceBox` 只让方法内部的形参指向新对象，调用者的变量没有被重新赋值。

---

## 3. 类、对象与构造方法

### 3.1 类和对象是什么关系

**类定义一类事物的数据和行为；对象是根据类创建出来的具体实例。**

例如：`Student` 是学生的定义；“编号 S001、姓名小张的学生”是一个对象。

| 概念 | 例子 | 作用 |
| --- | --- | --- |
| 字段，也叫成员变量 | `name`、`age` | 保存对象状态 |
| 方法 | `introduce()` | 描述对象行为 |
| 构造方法 | `Student(...)` | 在创建对象时初始化它 |
| 对象引用 | `student` | 通过它访问对象 |
| `new` | `new Student(...)` | 创建对象，并调用相应构造方法 |

### 3.2 一个完整的类

**完整示例：StudentDemo.java**

```java
class Student {
    private String name;
    private int age;

    public Student(String name) {
        this(name, 18);
    }

    public Student(String name, int age) {
        this.name = name;
        setAge(age);
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException("年龄应在 0 到 150 之间");
        }
        this.age = age;
    }

    public void introduce() {
        System.out.println("我叫" + name + "，今年" + age + "岁");
    }
}

public class StudentDemo {
    public static void main(String[] args) {
        Student first = new Student("小张", 20);
        Student second = new Student("小李");

        first.introduce();
        second.introduce();

        first.setAge(21);
        System.out.println(first.getAge());
    }
}
```

暂时把 `throw new IllegalArgumentException(...)` 理解成“发现不合理的参数，立即抛出异常”；异常章节会进一步说明。

### 3.3 `this` 到底指什么

`this` 表示当前对象。

```java
// 局部片段：位于 Student 的构造方法或实例方法中
this.name = name;
```

左边 `this.name` 是当前对象的字段；右边 `name` 是传入的参数。如果只写 `name = name;`，实际是参数给自己赋值，字段没有得到预期的赋值。

`this(...)` 表示调用本类的另一个构造方法。在本文采用的 Java 17 写法中，它必须是构造方法的第一条语句。

### 3.4 构造方法的规则

- 名字必须与类名相同。
- **没有返回类型，连 `void` 都不写。**
- 可以重载，例如 `Student(String name)` 和 `Student(String name, int age)`。
- 如果类没有显式声明任何构造方法，编译器才会提供默认的无参构造方法。
- 一旦自己声明了构造方法，编译器就不会再自动补上无参构造方法。
- 构造方法不能被继承，也不能像实例方法那样重写。

```java
// 局部片段：错误理解示意
// public void Student() { }
// 这是一个恰好叫 Student 的普通方法，不是构造方法。
```

### 3.5 两个引用可能指向同一个对象

```java
// 局部片段：放在前面 StudentDemo 的 main 中
Student a = new Student("小张", 20);
Student b = a;
b.setAge(22);
System.out.println(a.getAge()); // 预期：22
```

`b = a` 复制的是引用值，没有复制出另一个学生对象。

---

## 4. `static` 与 `final`

### 4.1 `static` 表示成员属于类

| 对比 | 实例成员 | 静态成员 |
| --- | --- | --- |
| 写法 | 不加 `static` | 加 `static` |
| 归属 | 某个对象 | 类 |
| 字段特点 | 每个对象分别保存自己的字段值 | 同一个类的静态字段由实例共享 |
| 推荐访问方式 | `对象.成员` | `类名.成员` |
| 方法中的 `this` | 可以使用 | 不能使用 |

初学时可以把静态字段理解为“这一类对象共享的数据”。更严格地说，不同类加载器加载出的类可能各有自己的静态状态；这里先关注普通单应用场景。

**完整示例：CounterDemo.java**

```java
class Member {
    private static int createdCount = 0;
    private final String name;

    public Member(String name) {
        this.name = name;
        createdCount++;
    }

    public String getName() {
        return name;
    }

    public static int getCreatedCount() {
        return createdCount;
    }
}

public class CounterDemo {
    public static void main(String[] args) {
        Member first = new Member("小张");
        Member second = new Member("小李");

        System.out.println(first.getName());
        System.out.println(second.getName());
        System.out.println(Member.getCreatedCount()); // 预期：2
    }
}
```

这里统计的是**累计创建次数**，不是当前仍存活的对象数量。示例也没有处理多线程计数问题。

### 4.2 静态方法能访问什么

- 可以直接使用同一个类中的静态成员。
- 不能在没有对象的情况下直接使用实例字段或实例方法。
- 可以先取得对象，再通过对象访问实例成员。
- 静态方法没有当前实例，所以不能使用 `this`、`super`。

**易错点：**`static` 不表示“常量”，也不自动带来线程安全。能否修改由 `final`、对象设计及其他规则决定。

### 4.3 `final` 的三种常见用法

| 位置 | 含义 |
| --- | --- |
| 变量 | 只能赋值一次 |
| 方法 | 子类不能重写该方法 |
| 类 | 不能被继承 |

```java
// 局部片段：位于类体中
public static final int MAX_AGE = 150;
```

`static final` 常用来声明类级常量。

```java
// 局部片段：位于方法中，需要导入 java.util.List 和 ArrayList
final List<String> names = new ArrayList<>();
names.add("小张"); // 可以修改这个列表的内容
// names = new ArrayList<>(); // 编译错误，不能重新绑定引用
```

**`final` 修饰引用，不等于这个对象本身不可变。**

---

## 5. 封装、包与访问权限

### 5.1 封装的重点是保护规则

封装是把状态和操作状态的行为放进类里，并通过合适的访问入口约束外部使用方式。

学生的年龄不能随意写成负数，因此：

1. 字段声明为 `private`。
2. 对外提供必要的方法。
3. 在方法中校验输入、维护规则。

**不是每个字段都必须配一个 setter。** 例如学生编号在创建后不应更改，可以只提供 getter；账户扣款可以提供 `withdraw()`，而不是把余额任由外部设置。

### 5.2 `package` 和 `import`

`package` 用于组织类，并形成命名空间；`import` 让代码可以用简短类名引用其他包里的类型。

```java
// 局部片段：一个带包名的源文件开头
package study.basic;

import java.util.ArrayList;
import java.util.List;
```

`package` 声明位于导入和类声明之前。`import` 不是复制代码，也不会因为导入某个类型就创建对象。`java.lang` 中的常见类型，如 `String`、`System`，通常不需要显式导入。

### 5.3 四种访问范围

| 修饰符 | 声明它的类内部 | 同一个包中的其他类 | 不同包的子类 | 不同包的非子类 |
| --- | --- | --- | --- | --- |
| `private` | 可以 | 不可以 | 不可以直接访问 | 不可以 |
| 不写，包访问权限 | 可以 | 可以 | 不可以 | 不可以 |
| `protected` | 可以 | 可以 | 可以，但受子类访问规则限制 | 不可以 |
| `public` | 可以 | 可以 | 可以 | 可以 |

注意：

- “不写”不是写一个 `default` 关键字。
- 普通顶层类只能声明为 `public` 或包访问权限；成员内部类另有规则。
- 表格关注普通类之间的直接访问。嵌套类与其外围类之间的私有成员访问有特殊规则，不必据此把 `private` 理解成对同包开放。
- 实际能否访问还取决于所在类是否可访问；采用模块系统时，还会受模块导出等规则影响。

### 5.4 `protected` 的跨包限制

**局部片段：下面对应两个文件，用于说明访问规则。**

```java
// a/Parent.java
package a;

public class Parent {
    protected int value = 10;
}
```

```java
// b/Child.java
package b;

import a.Parent;

public class Child extends Parent {
    public void show(Child otherChild, Parent parent) {
        System.out.println(this.value);       // 可以
        System.out.println(otherChild.value); // 可以
        // System.out.println(parent.value);  // 编译错误
    }
}
```

不同包中，子类代码通过对象引用访问父类的受保护实例成员时，引用的编译时类型需要是当前子类或其子类型。**“我是子类”不等于可以通过任意父类对象直接访问它的 `protected` 字段。**

---

## 6. 继承、重写与多态

### 6.1 继承表示“是一种”

```java
// 局部片段：类声明
class Dog extends Animal {
    // Dog 是一种 Animal
}
```

- 一个普通类只能直接继承一个类。
- 一个类可以实现多个接口。
- 除 `Object` 外，类都有父类；普通类未显式写 `extends` 时，直接父类是 `Object`。
- 父类的私有状态仍属于对象状态的一部分，但子类不能直接访问父类的 `private` 字段。
- 继承不应只为了“少写几行代码”。如果关系是“拥有一个”，通常考虑把另一个对象作为字段，即组合。

例如：狗是一种动物适合说明继承；学生拥有一个地址，适合说明组合。

### 6.2 `super` 与父类构造方法

- `super(...)`：调用父类构造方法。
- `super.method()`：调用父类可访问的相应方法实现。
- `this(...)`：调用本类的另一个构造方法。

在 Java 17 中，构造方法里显式的 `this(...)` 或 `super(...)` 必须是第一条语句，不能在同一个构造方法中同时直接写两者。

如果没有显式调用本类其他构造方法或父类构造方法，编译器会尝试插入 `super()`。如果父类没有可访问的无参构造方法，就需要显式调用合适的父类构造方法。

### 6.3 一个完整的多态示例

**完整示例：PolymorphismDemo.java**

```java
class Animal {
    private final String name;

    public Animal(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void speak() {
        System.out.println(name + "发出声音");
    }
}

class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }

    @Override
    public void speak() {
        System.out.println(getName() + "：汪汪");
    }

    public void fetch() {
        System.out.println("捡球");
    }
}

class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }

    @Override
    public void speak() {
        System.out.println(getName() + "：喵喵");
    }
}

public class PolymorphismDemo {
    static void makeSound(Animal animal) {
        animal.speak();
    }

    public static void main(String[] args) {
        Animal animal = new Dog("小黑");
        animal.speak(); // 预期：小黑：汪汪

        makeSound(new Cat("小白")); // 预期：小白：喵喵

        // animal.fetch(); // 编译错误：Animal 没有声明 fetch
        if (animal instanceof Dog) {
            Dog dog = (Dog) animal;
            dog.fetch();
        }
    }
}
```

### 6.4 多态的两个视角

```java
Animal animal = new Dog("小黑");
```

- **编译时类型是 `Animal`**：编译器据此判断有哪些成员可以访问，并完成重载方法的选择。
- **运行时对象类型是 `Dog`**：调用可重写的实例方法时，执行对象实际类型对应的重写实现。

因此，`animal.speak()` 执行 `Dog` 的实现，但 `animal.fetch()` 无法直接通过编译。

这个规则针对可重写的实例方法。字段访问不具有这种动态分派行为；静态方法是隐藏关系，也不能套用实例方法重写的规则。

### 6.5 重载与重写

| 对比 | 重载 Overload | 重写 Override |
| --- | --- | --- |
| 核心 | 同名方法，参数列表不同 | 子类提供父类实例方法的新实现 |
| 参数 | 数量、类型或类型顺序不同 | 方法签名匹配 |
| 返回类型 | 不能只靠返回类型构成重载 | 相同，或满足协变返回规则 |
| 选择时机 | 编译时选择签名 | 运行时选择重写实现 |
| 例子 | `print(int)`、`print(String)` | `Dog.speak()` 重写 `Animal.speak()` |

重写时还要注意：

- 建议加 `@Override`，让编译器帮助发现签名写错等问题。
- 访问权限不能比被重写的方法更严格。
- 不能扩大受检异常的声明范围。
- `final` 方法不能重写；`private` 方法对子类不可见，不构成子类重写。
- 构造方法只能重载，不能重写。

**易错点：**把父类引用强转成不匹配的子类类型，可能触发 `ClassCastException`。`instanceof` 用于判断类型；引用是 `null` 时，`instanceof` 结果为 `false`。

---

## 7. 抽象类与接口

### 7.1 抽象类：保留共同状态与部分实现

抽象类用 `abstract class` 声明，不能直接实例化。它可以有字段、构造方法、普通方法和抽象方法。

**局部片段：独立的类定义，用于说明结构。**

```java
abstract class Shape {
    public abstract double area();

    public void describe() {
        System.out.println("这是一个图形");
    }
}

class Rectangle extends Shape {
    private final double width;
    private final double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double area() {
        return width * height;
    }
}
```

抽象方法没有方法体；具体子类需要实现尚未实现的抽象方法，否则子类也应声明为抽象类。

### 7.2 接口：声明需要提供的能力

接口强调“能做什么”。变量声明成接口类型后，可以在不同实现之间切换。

**完整示例：InterfaceDemo.java**

```java
interface Payment {
    void pay(int amountInCents);
}

class BalancePayment implements Payment {
    @Override
    public void pay(int amountInCents) {
        System.out.println("余额支付：" + amountInCents + "分");
    }
}

class CardPayment implements Payment {
    @Override
    public void pay(int amountInCents) {
        System.out.println("银行卡支付：" + amountInCents + "分");
    }
}

public class InterfaceDemo {
    static void checkout(Payment payment) {
        payment.pay(1000);
    }

    public static void main(String[] args) {
        checkout(new BalancePayment());
        checkout(new CardPayment());
    }
}
```

这是能力和多态的演示，只打印信息，不涉及实际支付逻辑。金额用整数“分”表示，是为了避免引入浮点误差。

逐句理解：

- `interface Payment` 定义接口。
- `implements Payment` 表示类承诺实现接口。
- `void pay(...)` 在这里隐含 `public abstract`。
- 实现方法写成 `public`，不能缩小接口方法的访问范围。
- `checkout` 接收接口类型，只需要知道“可以支付”，不必绑定某一个实现类。

### 7.3 接口并非只能有抽象方法

在 Java 17 中，接口还可以声明 `default` 方法、静态方法和私有辅助方法。先掌握抽象方法和实现关系，再扩展其他写法即可。

接口字段隐含 `public static final`；接口没有普通实例字段，也没有构造方法。

### 7.4 怎么选择

| 需要 | 可以考虑 |
| --- | --- |
| 共享对象状态、构造过程与部分实现 | 抽象类 |
| 定义某种能力，允许不同类各自实现 | 接口 |
| 一个类需要具有多种能力 | 实现多个接口 |
| 一个对象需要使用另一个对象提供的功能 | 组合，把另一个对象作为字段 |

---

## 8. 字符串、`==` 与 `equals`

### 8.1 `==` 和 `equals` 不是一回事

| 场景 | `==` | `equals` |
| --- | --- | --- |
| 基本类型 | 比较值，可能发生数值提升 | 基本类型本身没有方法 |
| 引用类型 | 比较是否指向同一个对象，或都为 `null` | 按类定义的规则比较是否相等 |
| `String` | 比较引用 | 比较字符串内容 |
| 未重写 `equals` 的普通对象 | 比较引用 | 继承 `Object.equals`，默认也比较引用身份 |

**完整示例：StringDemo.java**

```java
import java.util.Objects;

public class StringDemo {
    public static void main(String[] args) {
        String first = new String("Java");
        String second = new String("Java");

        System.out.println(first == second);      // 预期：false
        System.out.println(first.equals(second)); // 预期：true

        String value = null;
        System.out.println("Java".equals(value));          // 预期：false
        System.out.println(Objects.equals(value, "Java")); // 预期：false

        String text = "hello";
        text.concat(" Java");
        System.out.println(text); // 预期：hello

        text = text.concat(" Java");
        System.out.println(text); // 预期：hello Java
    }
}
```

### 8.2 字符串不可变

字符串对象的内容创建后不能修改。`concat`、`replace` 等操作不会改变原字符串对象；使用结果通常需要接收返回值。

`text = ...` 是让变量指向另一个字符串对象，不是修改原字符串对象。

循环中反复拼接较多内容时，可以使用 `StringBuilder`：

```java
// 局部片段：位于方法中
StringBuilder builder = new StringBuilder();
builder.append("姓名：").append("小张").append("，年龄：").append(20);
String message = builder.toString();
```

### 8.3 常见空值陷阱

- `value.equals("Java")`：`value` 是 `null` 时会抛出 `NullPointerException`。
- `"Java".equals(value)`：可用于与确定的非空字符串比较。
- `Objects.equals(a, b)`：处理任意两个可能为 `null` 的引用。
- `null` 是“没有引用到对象”；`""` 是一个内容为空的字符串对象。

**易错点：**字符串常量池可能让两个字面量引用相同，不能因此总结为“字符串内容相等时 `==` 就一定为真”。比较内容应使用 `equals`。

---

## 9. 泛型与集合：List、Set、Map

### 9.1 先看整体关系

```text
Iterable
└── Collection
    ├── List：按位置保存元素，允许重复
    │   └── 常用 ArrayList
    └── Set：按相等性规则保存不重复元素
        ├── HashSet
        └── LinkedHashSet

Map：单独的一套接口，不继承 Collection
├── HashMap
└── LinkedHashMap
```

以上是入门用的简化关系，不是完整集合体系。

| 需求 | 常见选择 | 说明 |
| --- | --- | --- |
| 按顺序存储、通过下标访问 | `ArrayList` | 允许重复元素 |
| 去重，不要求迭代顺序 | `HashSet` | 不保证迭代顺序 |
| 去重，并保留插入顺序 | `LinkedHashSet` | 按插入顺序迭代 |
| 根据键查询值 | `HashMap` | 不保证迭代顺序 |
| 根据键查询值，并保留键的插入顺序 | `LinkedHashMap` | 本文使用默认的插入顺序模式 |

`List` 的“有序”指元素具有顺序和位置，**不是自动按大小排序**。`HashSet`、`HashMap` 的“不保证顺序”也不等于每次一定随机。

### 9.2 泛型：说明集合里存什么

```java
// 局部片段：位于方法中，需要相应 import
List<String> names = new ArrayList<>();
Map<String, Integer> scores = new HashMap<>();
```

- `List<String>` 表示元素类型为 `String`。
- `Map<String, Integer>` 表示键为 `String`、值为 `Integer`。
- `<>` 让编译器根据上下文推断类型参数。
- 泛型帮助在编译时发现放错类型的问题。
- 泛型类型参数不能直接使用基本类型：写 `List<Integer>`，不写 `List<int>`。

包装类型包括 `Integer`、`Long`、`Double`、`Boolean` 等。基本类型与包装类型之间可发生自动装箱、拆箱。

```java
// 局部片段：位于方法中
Integer boxed = 10; // 装箱
int number = boxed; // 拆箱

Integer missing = null;
// int value = missing; // 拆箱 null 会抛出 NullPointerException
```

不要用 `==` 比较两个包装对象的数值是否相等；缓存和装箱行为会让结果容易误导。按需要使用 `equals`、`Objects.equals` 或明确拆箱后的基本类型比较。

补充：`List<Integer>` 不是 `List<Number>` 的子类型，即使 `Integer` 是 `Number` 的子类。通配符可以后续单独学习。

### 9.3 List：保存有位置的元素

**完整示例：ListDemo.java**

```java
import java.util.ArrayList;
import java.util.List;

public class ListDemo {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        names.add("小张");
        names.add("小李");
        names.add("小张");

        System.out.println(names.size()); // 预期：3
        System.out.println(names.get(0)); // 预期：小张

        names.set(1, "小王");
        names.remove("小张"); // 删除第一个相等的元素

        for (String name : names) {
            System.out.println(name); // 依次输出：小王、小张
        }
    }
}
```

常见方法：`add`、`get`、`set`、`remove`、`contains`、`size`、`isEmpty`。

`ArrayList` 按下标访问通常为 O(1)；尾部添加的摊销成本通常为 O(1)；在中间插入、删除通常需要移动后面的元素。

#### `List<Integer>.remove` 的重载陷阱

```java
// 局部片段：位于方法中，需要 List 和 ArrayList 的 import
List<Integer> numbers = new ArrayList<>();
numbers.add(10);
numbers.add(20);
numbers.add(30);

numbers.remove(1);                   // 按下标删除 20
numbers.remove(Integer.valueOf(10)); // 按值删除 10
System.out.println(numbers);         // 预期：[30]
```

#### 遍历时删除元素

不要在 `ArrayList` 的增强 `for` 循环中直接调用 `list.remove(...)` 进行结构性修改。这种方式不可靠，可能出现 `ConcurrentModificationException` 或遗漏处理。

简单条件删除可以使用：

```java
// 局部片段：names 是已经创建的可修改 List<String>
names.removeIf(name -> name.startsWith("小"));
```

这里的 `name -> ...` 是一个简单 lambda，表示对每个元素计算删除条件。需要逐步控制遍历时，可以使用迭代器自己的 `remove()`。

### 9.4 Set：按规则去重

**完整示例：SetDemo.java**

```java
import java.util.LinkedHashSet;
import java.util.Set;

public class SetDemo {
    public static void main(String[] args) {
        Set<String> languages = new LinkedHashSet<>();

        System.out.println(languages.add("Java"));   // 预期：true
        System.out.println(languages.add("Python")); // 预期：true
        System.out.println(languages.add("Java"));   // 预期：false
        System.out.println(languages); // 预期：[Java, Python]
    }
}
```

`HashSet`、`LinkedHashSet` 判断重复需要配合 `hashCode` 和 `equals`。自定义对象如果仍使用默认的引用身份比较，两个字段相同但分别创建的对象仍可能作为两个元素保存。

### 9.5 Map：键和值的对应关系

**完整示例：MapDemo.java**

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class MapDemo {
    public static void main(String[] args) {
        Map<String, Integer> scores = new LinkedHashMap<>();

        scores.put("小张", 90);
        scores.put("小李", 85);
        Integer oldScore = scores.put("小张", 95);

        System.out.println(oldScore);       // 预期：90
        System.out.println(scores.size()); // 预期：2
        System.out.println(scores.get("小张")); // 预期：95
        System.out.println(scores.get("小王")); // 预期：null

        for (Map.Entry<String, Integer> entry : scores.entrySet()) {
            System.out.println(entry.getKey() + "：" + entry.getValue());
        }
    }
}
```

- 键按实现规定的相等性规则不能重复，值可以重复。
- `put` 相同键会更新值，并返回旧值。
- `get` 查不到键时通常返回 `null`。
- 如果实现允许 `null` 值，`get` 返回 `null` 不能区分“没有键”和“键对应的值就是 null”；需要 `containsKey`。
- 遍历键和值时，使用 `entrySet()` 很直接。

**重要区别：**`HashMap`、`LinkedHashMap` 的键通过 `hashCode` 和 `equals` 参与定位与比较；不能把这条规则直接推广给所有 Map 实现。

### 9.6 集合常见易错点

| 问题 | 说明 |
| --- | --- |
| 所有集合都能随意增删吗 | 不一定。`List.of(...)`、`Set.of(...)`、`Map.of(...)` 创建的集合不支持修改，也不允许 `null` |
| `Arrays.asList(...)` 能任意增删吗 | 它返回由原数组支持的固定大小列表，可以替换元素，不能改变大小 |
| 所有集合都允许 `null` 吗 | 不同接口实现有不同约束，应看具体实现 |
| `final List` 不可修改吗 | 只限制变量重新赋值，不限制列表内容 |
| `HashMap` 会按放入顺序遍历吗 | 没有这个保证；需要保留顺序时可选 `LinkedHashMap` |
| `ArrayList`、`HashMap` 自动线程安全吗 | 不是；本笔记练习按单线程使用 |
| `TreeSet`、`TreeMap` 如何判断等价 | 主要依据自然排序或比较器的比较结果，比较结果为 0 时视作等价；后续再学 |

---

## 10. `equals` 与 `hashCode`

### 10.1 先决定“业务上相等”是什么意思

两个学生对象是否相等，可以按编号判断，而不是按名字判断，因为不同学生可能同名。

这需要先确定规则，再实现代码。不是随意把所有字段都拿来比较，也不是所有类都必须重写 `equals`。

### 10.2 两个方法的关系

必须记住：

1. 如果 `a.equals(b)` 为 `true`，两者的 `hashCode()` 必须相同。
2. 两者 `hashCode()` 相同，不代表 `equals` 一定为 `true`，因为可能发生哈希冲突。
3. 参与相等性计算的信息不变时，哈希值应保持一致；不要求不同程序运行之间保持相同。
4. 重写 `equals` 时，应配套重写 `hashCode`。

`equals` 还应满足自反性、对称性、传递性和一致性；对于非空对象 `a`，`a.equals(null)` 应为 `false`。

### 10.3 一个按编号判等的完整例子

**完整示例：EqualityDemo.java**

```java
import java.util.HashSet;
import java.util.Objects;
import java.util.Set;

final class StudentKey {
    private final String id;

    public StudentKey(String id) {
        this.id = Objects.requireNonNull(id, "编号不能为 null");
    }

    @Override
    public boolean equals(Object other) {
        if (this == other) {
            return true;
        }
        if (other == null || getClass() != other.getClass()) {
            return false;
        }
        StudentKey that = (StudentKey) other;
        return id.equals(that.id);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}

public class EqualityDemo {
    public static void main(String[] args) {
        StudentKey first = new StudentKey("S001");
        StudentKey second = new StudentKey("S001");

        System.out.println(first == second);      // 预期：false
        System.out.println(first.equals(second)); // 预期：true

        Set<StudentKey> students = new HashSet<>();
        students.add(first);
        students.add(second);
        System.out.println(students.size()); // 预期：1
    }
}
```

代码说明：

- `equals` 参数必须是 `Object`，写成 `equals(StudentKey other)` 会成为另一个重载方法。
- `this == other` 先快速判断是否为同一对象。
- `getClass()` 判断实际类是否相同；这里的类声明为 `final`，不用额外处理子类之间的相等性设计。
- `Objects.requireNonNull` 在参数为 `null` 时抛出 `NullPointerException`。
- `id` 使用 `final`，并且本身为不可变字符串，使判等依据保持稳定。

### 10.4 为什么哈希集合的键不要随便修改

如果一个对象已放入 `HashSet`，或被用作 `HashMap` 的键，再修改参与 `equals/hashCode` 的字段，就可能导致集合无法按新状态正确查找、删除它。

可以把它理解成：对象最初按旧哈希信息存放，后来查询却拿着新哈希信息去定位。

**实用习惯：**用稳定的编号字符串作为 Map 的键，或让键对象中参与判等的状态保持不可变。普通 Map 值对象的其他字段可按业务规则修改，它们不直接参与键定位。

---

## 11. 异常处理

### 11.1 异常是什么

异常是程序执行过程中出现问题时，用来传递失败信息的一种机制。

方法抛出异常后，当前执行流程被打断，JVM 沿调用链寻找匹配的处理代码。如果没有找到，异常会继续传播，最终可能使当前线程终止。

```text
Throwable
├── Error
└── Exception
    ├── RuntimeException
    │   ├── NullPointerException
    │   ├── IllegalArgumentException
    │   └── NumberFormatException（IllegalArgumentException 的子类）
    └── 其他受检异常，例如 IOException
```

这是简化结构。`Error` 通常表示更严重的运行环境或系统问题，普通业务逻辑一般不尝试捕获后继续。

### 11.2 受检异常与非受检异常

| 分类 | 代表 | 编译器是否要求捕获或声明 |
| --- | --- | --- |
| 受检异常 checked exception | `IOException` | 是 |
| 非受检异常 unchecked exception | `RuntimeException` 及其子类、`Error` 及其子类 | 否 |

“非受检”不等于“不需要考虑”；“受检异常”也不是“编译时发生的异常”。它们都是运行中可能抛出的对象，区别在于编译器是否强制处理其传播声明。

### 11.3 `try`、`catch`、`finally`

**完整示例：ExceptionDemo.java**

```java
public class ExceptionDemo {
    public static void main(String[] args) {
        try {
            int age = Integer.parseInt("abc");
            System.out.println(age); // 转换失败时不会执行这一行
        } catch (NumberFormatException e) {
            System.out.println("年龄必须是整数");
        } finally {
            System.out.println("本次转换流程结束");
        }

        System.out.println("继续执行后面的代码");
    }
}
```

- `try`：放可能抛出异常的代码。
- `catch`：处理匹配的异常，包括指定类型的子类型。
- `finally`：执行离开 `try/catch` 时的收尾逻辑。

**易错点：**

- 异常发生后，`try` 中异常语句后面的代码不会接着从断点恢复执行。
- 有多个 `catch` 时，更具体的子类异常应写在更一般的父类异常之前。
- 不要把 `catch` 写空；这样会隐藏失败原因。
- 不要为了“程序不报错”而用 `catch (Exception e)` 吞掉所有问题。
- 不建议在 `finally` 中写 `return` 或再抛新异常，这可能覆盖原本的返回结果或异常。
- `finally` 并非在所有情况下绝对执行，例如 JVM 被强制终止时，不能依赖它。

### 11.4 `throw` 和 `throws`

| 关键字 | 出现位置 | 作用 |
| --- | --- | --- |
| `throw` | 方法体中 | 实际抛出一个异常对象 |
| `throws` | 方法声明中 | 声明异常可能向调用者传播 |

```java
// 局部片段：位于类体中
static void checkAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("年龄不能为负数");
    }
}
```

```java
// 局部片段：位于类体中，需要 import java.io.IOException
static void requireFile(boolean exists) throws IOException {
    if (!exists) {
        throw new IOException("文件不存在");
    }
}
```

第二段用布尔参数模拟文件是否存在，目的是区分两个关键字。调用 `requireFile` 时，调用者需要捕获 `IOException`，或者继续用 `throws` 声明它。

`throws` 不会自动处理异常，也不会主动制造异常。

### 11.5 用 try-with-resources 管理资源

实现了 `AutoCloseable` 的资源，可以放在 `try (...)` 中，在离开作用域时自动调用关闭方法。

**完整示例：ResourceDemo.java**

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.StringReader;

public class ResourceDemo {
    public static void main(String[] args) {
        try (BufferedReader reader =
                 new BufferedReader(new StringReader("Java\n基础复习"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("读取失败：" + e.getMessage());
        }
    }
}
```

这里读取的是内存中的字符串，不需要额外准备文件。多个资源通常按声明的逆序关闭；若主体和关闭过程都出现异常，关闭时的异常可能作为被抑制异常保留下来。

---

## 12. 自定义异常

### 12.1 为什么自定义

当标准异常不足以清楚表达某个业务失败时，可以定义自己的异常类型，例如：

- 学生编号重复：`DuplicateStudentException`。
- 找不到学生：`StudentNotFoundException`。
- 余额不足：`InsufficientBalanceException`。

它的作用是让调用者能够明确识别失败原因，并选择相应的处理方式。

### 12.2 继承 `Exception` 还是 `RuntimeException`

| 写法 | 特点 |
| --- | --- |
| `extends Exception` | 自定义受检异常，调用者需要捕获或声明 |
| `extends RuntimeException` | 自定义非受检异常，编译器不强制调用者捕获或声明 |

根据接口设计和调用方是否适合被强制处理来选择。业务失败既可以设计为受检异常，也可以设计为非受检异常；没有“业务异常只能继承某一个类”的语言规则。

### 12.3 完整的自定义异常示例

**完整示例：CustomExceptionDemo.java**

```java
class InvalidAgeException extends RuntimeException {
    public InvalidAgeException(String message) {
        super(message);
    }
}

public class CustomExceptionDemo {
    static void register(String name, int age) {
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("姓名不能为空");
        }
        if (age < 0 || age > 150) {
            throw new InvalidAgeException("年龄应在 0 到 150 之间：" + age);
        }
        System.out.println("注册成功：" + name);
    }

    public static void main(String[] args) {
        try {
            register("小张", -1);
        } catch (InvalidAgeException e) {
            System.out.println("注册失败：" + e.getMessage());
        }
    }
}
```

这里使用自定义年龄异常是为了展示写法；简单的参数校验通常直接使用 `IllegalArgumentException` 就够了，不必为每个条件创建异常类。

逐句理解：

1. `extends RuntimeException`：获得异常类的基本能力。
2. 构造方法接收描述文本。
3. `super(message)`：把说明交给父类保存。
4. `throw new InvalidAgeException(...)`：创建异常对象并抛出。
5. `catch (InvalidAgeException e)`：调用方捕获这一类失败。
6. `e.getMessage()`：读取描述文本。

### 12.4 转换异常时保留原始原因

```java
// 局部片段：异常类定义，可用于其他程序
class DataReadException extends RuntimeException {
    public DataReadException(String message, Throwable cause) {
        super(message, cause);
    }
}
```

捕获底层异常后，如果需要转换成自己的异常，使用 `new DataReadException("读取学生数据失败", e)` 能保留原始异常链。只复制 `e.getMessage()` 会丢失原始异常类型、调用栈等有用信息。

---

## 13. 综合练习：内存版学生信息管理

### 13.1 要完成什么

编写一个普通 Java 程序，实现：

1. 新增学生：编号、姓名、年龄。
2. 编号不能为空，年龄范围为 0～150。
3. 编号重复时抛出自定义异常。
4. 按编号查询，查不到时抛出自定义异常。
5. 按新增顺序列出全部学生。
6. 删除学生，并返回是否真的删除了数据。

只使用内存集合，不引入构建工具、框架或数据库。

先尝试自己写，再看参考实现。

### 13.2 设计思路

| 部分 | 职责 | 对应知识 |
| --- | --- | --- |
| `Student` | 保存学生数据并保证基本有效性 | 类、对象、构造、封装、`final` |
| `StudentService` | 声明管理学生的能力 | 接口 |
| `MemoryStudentService` | 用 Map 实现管理操作 | 实现类、集合 |
| 两个自定义异常 | 表达重复和不存在 | 继承、异常 |
| `StudentBookApp` | 调用功能并展示结果 | 多态、异常捕获 |

选择 `Map<String, Student>`，用稳定的编号字符串作为键。这里的去重由 **Map 的字符串键** 完成，所以不需要为了这个功能给学生类额外重写 `equals/hashCode`。

### 13.3 参考实现

**完整示例：StudentBookApp.java**

```java
import java.util.ArrayList;
import java.util.LinkedHashMap;
import java.util.List;
import java.util.Map;
import java.util.Objects;

final class Student {
    private final String id;
    private final String name;
    private final int age;

    public Student(String id, String name, int age) {
        if (id == null || id.trim().isEmpty()) {
            throw new IllegalArgumentException("编号不能为空");
        }
        if (name == null || name.trim().isEmpty()) {
            throw new IllegalArgumentException("姓名不能为空");
        }
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException("年龄应在 0 到 150 之间");
        }
        this.id = id.trim();
        this.name = name.trim();
        this.age = age;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    @Override
    public String toString() {
        return id + " | " + name + " | " + age;
    }
}

class DuplicateStudentException extends RuntimeException {
    public DuplicateStudentException(String id) {
        super("学生编号已存在：" + id);
    }
}

class StudentNotFoundException extends RuntimeException {
    public StudentNotFoundException(String id) {
        super("未找到学生：" + id);
    }
}

interface StudentService {
    void add(Student student);
    Student findById(String id);
    List<Student> findAll();
    boolean removeById(String id);
}

class MemoryStudentService implements StudentService {
    private final Map<String, Student> students = new LinkedHashMap<>();

    @Override
    public void add(Student student) {
        Objects.requireNonNull(student, "学生对象不能为 null");
        String id = student.getId();
        if (students.containsKey(id)) {
            throw new DuplicateStudentException(id);
        }
        students.put(id, student);
    }

    @Override
    public Student findById(String id) {
        String normalizedId = normalizeId(id);
        Student student = students.get(normalizedId);
        if (student == null) {
            throw new StudentNotFoundException(normalizedId);
        }
        return student;
    }

    @Override
    public List<Student> findAll() {
        return new ArrayList<>(students.values());
    }

    @Override
    public boolean removeById(String id) {
        return students.remove(normalizeId(id)) != null;
    }

    private String normalizeId(String id) {
        if (id == null || id.trim().isEmpty()) {
            throw new IllegalArgumentException("编号不能为空");
        }
        return id.trim();
    }
}

public class StudentBookApp {
    public static void main(String[] args) {
        StudentService service = new MemoryStudentService();

        service.add(new Student("S001", "小张", 20));
        service.add(new Student("S002", "小李", 21));

        System.out.println("查询：" + service.findById("S001"));

        System.out.println("全部学生：");
        for (Student student : service.findAll()) {
            System.out.println(student);
        }

        try {
            service.add(new Student("S001", "小王", 22));
        } catch (DuplicateStudentException e) {
            System.out.println(e.getMessage());
        }

        System.out.println("删除 S002：" + service.removeById("S002"));

        try {
            service.findById("S002");
        } catch (StudentNotFoundException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

预期输出：

```text
查询：S001 | 小张 | 20
全部学生：
S001 | 小张 | 20
S002 | 小李 | 21
学生编号已存在：S001
删除 S002：true
未找到学生：S002
```

### 13.4 重点看懂这几处

**① 为什么用 `StudentService service = new MemoryStudentService()`？**

左边声明需要的接口能力，右边创建具体实现。调用方通过接口使用功能，这就是接口多态的一种用法。

**② 为什么字段都是 `private final`？**

`private` 限制直接访问，`final` 限制字段再次赋值。这里字段分别是不可变字符串和基本类型，并且类也为 `final`，因此学生数据在创建后保持不变。

**③ 为什么列表返回 `new ArrayList<>(students.values())`？**

返回一个独立列表，使调用者对列表增删时不会直接增删内部 Map。它是浅拷贝，元素仍是同一批学生对象；本例的学生对象不可变，因此也不会暴露可修改状态。

**④ 为什么 `findById` 可以用 `null` 判断不存在？**

因为 `add` 拒绝空学生对象，内部没有存入 `null` 值，`get` 返回 `null` 在本例中就表示没有该键。

**⑤ 为什么 `removeById` 可以根据返回值判断是否删除？**

`Map.remove` 返回原来的值，找不到时返回 `null`。由于本例不保存 `null` 值，返回非空就意味着确实删除了学生。

**⑥ 为什么这里 `containsKey` 后再 `put` 可以使用？**

本练习按单线程执行。多线程中这两个步骤不是一个原子操作，不能把这段代码直接当成并发安全实现。

### 13.5 自己扩展

- 增加按姓名查找，考虑同名学生应该返回一个人还是一个列表。
- 增加年龄统计，空列表时明确返回什么或抛出什么异常。
- 增加修改学生信息，编号保持不变，用新学生对象替换旧值。
- 增加控制台输入，区分“数字格式错误”和“年龄范围不合法”。

---

## 14. 基础练习与参考答案

### 14.1 练习一：数组统计

给定 `int[] scores = {80, 90, 100};`，计算总分、平均分、最高分。

要求：平均分保留小数；数组为 `null` 或空数组时明确处理。

**参考答案，局部片段：放在类体中。**

```java
static void printStats(int[] scores) {
    if (scores == null || scores.length == 0) {
        throw new IllegalArgumentException("成绩数组不能为空");
    }

    long total = 0;
    int max = scores[0];
    for (int score : scores) {
        total += score;
        if (score > max) {
            max = score;
        }
    }

    double average = (double) total / scores.length;
    System.out.println("总分：" + total);
    System.out.println("平均分：" + average);
    System.out.println("最高分：" + max);
}
```

关键点：最高分从第一个元素开始；不要先用整数除法算平均分再赋给 `double`。

### 14.2 练习二：构造与封装

写一个 `Book` 类：

- 标题创建时不能为空。
- 页数必须大于 0。
- 用构造方法完成初始化。
- 提供 getter，不提供随意修改字段的 setter。

**参考答案，局部片段：一个类定义。**

```java
final class Book {
    private final String title;
    private final int pages;

    public Book(String title, int pages) {
        if (title == null || title.trim().isEmpty()) {
            throw new IllegalArgumentException("标题不能为空");
        }
        if (pages <= 0) {
            throw new IllegalArgumentException("页数必须大于 0");
        }
        this.title = title.trim();
        this.pages = pages;
    }

    public String getTitle() {
        return title;
    }

    public int getPages() {
        return pages;
    }
}
```

关键点：封装不等于机械生成所有 getter/setter；对象创建时就应满足基本规则。

### 14.3 练习三：用接口实现多态

定义接口 `Notifier`，包含 `void send(String message)`；创建两个实现类，分别打印“短信通知”和“邮件通知”。让一个接收 `Notifier` 参数的方法调用它。

**参考答案，局部片段：放在独立练习文件的类定义中或同一文件的顶层类型位置。**

```java
interface Notifier {
    void send(String message);
}

class SmsNotifier implements Notifier {
    @Override
    public void send(String message) {
        System.out.println("短信通知：" + message);
    }
}

class EmailNotifier implements Notifier {
    @Override
    public void send(String message) {
        System.out.println("邮件通知：" + message);
    }
}
```

**调用片段：前两个方法放在练习类体中。**

```java
static void notifyUser(Notifier notifier, String message) {
    notifier.send(message);
}

static void demonstrate() {
    notifyUser(new SmsNotifier(), "复习 Java");
    notifyUser(new EmailNotifier(), "复习 Java");
}
```

关键点：变量或参数可以使用接口类型；执行时调用具体实现的方法。

### 14.4 练习四：去重并保留顺序

把 `Java、Python、Java、SQL、Python` 去重，保留首次出现的顺序。

**参考答案，局部片段：放在方法中，需要集合类的 import。**

```java
List<String> source = List.of("Java", "Python", "Java", "SQL", "Python");
Set<String> unique = new LinkedHashSet<>(source);
System.out.println(unique); // 预期：[Java, Python, SQL]
```

追问：换成 `HashSet` 后，是否还能依赖这个遍历顺序？答案：不能。

### 14.5 练习五：统计词语出现次数

给定 `{"java", "sql", "java", "java", "sql"}`，统计每个词的次数，按首次出现顺序输出。

**参考答案，局部片段：放在方法中，需要 Map 和 LinkedHashMap 的 import。**

```java
String[] words = {"java", "sql", "java", "java", "sql"};
Map<String, Integer> counts = new LinkedHashMap<>();

for (String word : words) {
    int oldCount = counts.getOrDefault(word, 0);
    counts.put(word, oldCount + 1);
}

System.out.println(counts); // 预期：{java=3, sql=2}
```

关键点：Map 的键不重复，重复遇到同一个词时更新其次数。本例只存放非空的次数值。

### 14.6 练习六：自定义受检异常

定义 `InsufficientBalanceException extends Exception`。扣款金额必须大于 0，余额不足时抛出该异常；调用方捕获并打印失败原因。

**参考答案，局部片段：异常类放在顶层类型位置，`withdraw` 放进练习类体。**

```java
class InsufficientBalanceException extends Exception {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}
```

```java
static long withdraw(long balanceInCents, long amountInCents)
        throws InsufficientBalanceException {
    if (balanceInCents < 0) {
        throw new IllegalArgumentException("余额不能为负数");
    }
    if (amountInCents <= 0) {
        throw new IllegalArgumentException("扣款金额必须大于 0");
    }
    if (amountInCents > balanceInCents) {
        throw new InsufficientBalanceException("余额不足");
    }
    return balanceInCents - amountInCents;
}
```

**调用片段：放在方法中，并使用上面的异常类和方法。**

```java
try {
    long remaining = withdraw(1000, 1500);
    System.out.println("剩余：" + remaining + "分");
} catch (InsufficientBalanceException e) {
    System.out.println("扣款失败：" + e.getMessage());
}
```

关键点：金额使用整数“分”；`throw` 负责抛出，`throws` 声明传播。这个函数返回新余额，不会自动修改调用者传入的基本类型变量；成功扣款时需要由调用者接收返回值。

---

## 15. 自测问答

先遮住答案，尝试自己解释，并举一行代码说明。

| 问题 | 参考答案 |
| --- | --- |
| `.java` 和 `.class` 有什么区别 | 前者是源代码，后者通常是编译后的字节码文件 |
| JVM 和 JDK 有什么区别 | JVM 负责运行字节码；JDK 提供开发工具和运行所需组件 |
| 类和对象有什么区别 | 类是定义，对象是具体实例 |
| `new` 做什么 | 创建对象并调用相应构造方法 |
| 构造方法要写 `void` 吗 | 不写任何返回类型 |
| 自己写了有参构造后，还一定有无参构造吗 | 不一定；编译器不再自动补默认无参构造 |
| `this` 和 `super` 的区别 | `this` 指当前对象；`super` 用于访问父类实现或调用父类构造方法 |
| `static` 字段是每个对象一份吗 | 普通同一类场景中属于类，由实例共享 |
| `final` 引用指向的集合一定不可修改吗 | 不一定，只是引用不能再次赋值 |
| 封装就是加 getter/setter 吗 | 不是，重点是维护状态和业务规则 |
| Java 支持一个类继承多个类吗 | 不支持，但一个类可以实现多个接口 |
| 重载能只靠返回类型区分吗 | 不能 |
| 重写为什么常写 `@Override` | 让编译器检查是否确实重写了方法 |
| 父类引用调用重写方法时，看谁的实现 | 看运行时对象类型 |
| 接口一定没有方法实现吗 | 不是，Java 17 接口可以有 default、static、private 方法 |
| Java 对对象参数是引用传递吗 | 不是，传的是引用值的副本，仍是按值传递 |
| List 的有序等于排好序吗 | 不等于，指有顺序和位置 |
| Map 是 Collection 的子接口吗 | 不是 |
| 两个对象 hashCode 相同就 equals 相等吗 | 不一定，可能发生哈希冲突 |
| equals 相等，hashCode 能不同吗 | 不能，这违反约定 |
| `HashMap.get` 返回 null 就一定没有键吗 | 不一定；如果允许 null 值，需结合 containsKey 判断 |
| 修改 HashMap 键对象的判等字段有什么风险 | 可能导致无法正常查找或删除 |
| `throw` 和 `throws` 的区别 | 一个实际抛出异常，一个在方法签名中声明异常可能传播 |
| 自定义 RuntimeException 必须捕获吗 | 编译器不强制；是否捕获取决于调用方处理策略 |
| finally 里适合写 return 吗 | 不适合，可能覆盖已有返回或异常 |

---

## 16. 易错点速查

| 容易写错或理解错 | 正确理解 |
| --- | --- |
| `java HelloJava.class` | 本文的类启动命令使用 `java HelloJava` |
| `double average = total / count;` | 若两者为整数，先发生整数除法；应在除法前转为浮点数 |
| `name = name;` | 同名参数给自己赋值；字段赋值使用 `this.name = name;` |
| 构造方法写成 `public void Student()` | 这是普通方法，不是构造方法 |
| `static` 就是不变 | `static` 表示类成员，`final` 才约束再次赋值 |
| `private` 字段对子类能直接访问 | 不能直接访问，可通过父类提供的可访问方法操作 |
| `protected` 对所有外部代码都开放 | 不是，跨包访问有子类条件和引用类型限制 |
| 父类引用能直接调用所有子类方法 | 只能访问编译时类型允许的成员 |
| 用 `==` 比较字符串内容 | 使用 `equals` 或 `Objects.equals` |
| `List<Integer>.remove(1)` 删除数字 1 | 它按下标删除，按值删除可传 `Integer.valueOf(1)` |
| 增强 for 中直接对原 List 调用 remove | 改用 removeIf 或迭代器自己的 remove |
| HashSet 按字段相同自动去重 | 自定义对象需要先定义合适的 equals/hashCode |
| 只重写 equals 就够了 | 通常应配套重写 hashCode，维护约定 |
| `Map.put` 相同键会多存一条 | 会更新该键对应的值 |
| `Integer value = null; int n = value;` | 自动拆箱时抛出空指针异常 |
| 捕获异常后继续执行 try 中剩下的语句 | 不会从异常位置恢复执行 |
| `throws` 已经处理了异常 | 它只是声明异常可以继续传播 |
| finally 保证任何情况下都会执行 | JVM 被强制终止等情况不作保证 |

---

## 17. 我的复习记录

### 17.1 掌握清单

- [ ] 能画出 `.java → javac → .class → JVM` 的过程。
- [ ] 能说明 JDK、JVM 与标准类库的关系。
- [ ] 能区分基本类型、引用类型与 `null`。
- [ ] 能写数组遍历、分支、方法和正确的平均数计算。
- [ ] 能解释 Java 按值传参，以及修改对象与重新绑定形参的差别。
- [ ] 能独立写一个带私有字段、构造方法和必要 getter 的类。
- [ ] 能解释 `this`、`super`、构造重载和默认无参构造。
- [ ] 能区分实例成员、`static` 与 `final`。
- [ ] 能说明封装为什么不等于生成 setter。
- [ ] 能区分四种访问权限，知道 `protected` 的跨包限制。
- [ ] 能区分继承、重载、重写与多态。
- [ ] 能用接口类型接收不同实现对象。
- [ ] 能正确比较字符串和处理可能为 `null` 的引用。
- [ ] 能根据需求选择 List、Set、Map 及常见实现。
- [ ] 能解释装箱、拆箱与包装类型的比较陷阱。
- [ ] 能按业务规则实现配套的 `equals/hashCode`。
- [ ] 能说明哈希集合的键为什么应保持稳定。
- [ ] 能区分受检异常、非受检异常、`throw` 与 `throws`。
- [ ] 能编写并使用一个自定义异常。
- [ ] 能不看答案完成学生信息管理的核心操作。

### 17.2 错题与疑问模板

```text
日期：
知识点：
我原来的理解：
出现问题的代码：
正确理解：
我重新写出的最小例子：
下次复习日期：
```

### 17.3 后续学习边界

基础稳定后，可分别建立“Java 工程工具”“Web 基础”“Spring Boot”“数据库”笔记。这样可以清楚地区分：Java 语言本身提供的能力、工程工具的作用，以及框架在语言之上封装的功能。
