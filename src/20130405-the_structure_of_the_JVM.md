>看完周志明版的`深入理解Java虚拟机`之后，对JVM的理解从原来的无知到初步了解，算是有了一个质的飞跃，但是理解JVM最好的方式还是阅读JVM规范，但是我英文实在是不敢恭维，只好用笨办法了，看完一遍之后，我再试图把它翻译成中文来加强理解。翻译并不会严格按照英文描述来翻，只求准确表达原意即可。

这篇文档针对的是一个抽象的机器。它不描述任何特定的JVM实现。

为了正确地实现JVM，你仅需要能够读懂`class`文件格式并且正确地执行里面的操作即可。实现细节不是JVM规范的一部分因此不会约束实现者的创造性。比如，运行时数据区(rum-time data areas)的内存部局，使用的GC算法，以及任何内部的JVM指令优化（比如，把它们翻译成机器码）都留给了实现者来自由发挥。

在本篇规范中的所有Unicode的引用都来自*The Unicode Standard,Version 6.0.0*，详见<http://www.unicode.org>。

## 2.1 class文件格式
能够被JVM执行的编译代码是用一个硬件和操作系统无关的二进制格式描述的，典型地（但不是必须地）的场景是存储在文件中，比如我们熟知的class文件格式。class文件格式精确地定义了一个类或接口的描述，包括像字节顺序的细节，这也许被用来做特定平台对象文件格式的认证。

第四章，`class文件格式`，会详细介绍这些细节。

## 2.2 数据类型
像Java编程语言一样，JVM操作两种类型：`基本类型（primitive）`和`引用类型（reference）`。相应地，这两种类型的值能够被存储在变量里，作为参数被传递，被方法返回，以及在基本值和引用值上做操作。

JVM期望几乎所有的类型检查在运行时之前做，比如靠一个编译器，而不是靠JVM自己来做。基本类型的值不需要被打标或另外地做检查确定它们在运行时的类型，或者从引用类型的值中区分开。代替的是，JVM指令集使用指定特定类型的指令来区分它的操作数类型。比如，*iadd*,*ladd*,*fadd*和*dadd*都是JVM对两个数值求和的指令，但每一个都被分别绑定它的操作数类型：`int`,`long`,`float`和`double`。JVM指令集支持的类型汇总，详见[2.11.1](#2.11.1)

JVM包含对对象的明确支持。一个对象是一个动态创建的类实例或者一个数组。对对象的引用被视为是JVM的`reference`。`reference`类型的值可以被看作是一个对象的指针。对一个对象的引用可以有多个。对象总是通过引用类型的值来做操作、传递和测试。

## 2.3 基本类型和值
被JVM支持的基本类型是`数值(Numeric)`类型，`布尔(boolean)`类型[2.3.4](#2.3.4)和`returnAddress`类型[2.3.3](#2.3.3)。

数值类型包含`整形(integal)`[2.3.1](#2.3.1)和`浮点型(floating-point)`[2.3.2](#2.3.2)。

整形包括：
* `byte`，它的值是8位有符号补码整数，默认值是0
* `short`，它的值是16位有符号补码整数，默认是0
* `int`，它的值是32位有符号补码整数，默认是0
* `long`，它的值是64位有符号补码整数，默认是0
* `char`，它的值是用UTF-16编码表示的16位无符号整数，默认是null('\u0000')

浮点型包括：
* `float`，它的值是float值集里的元素，或者是扩展float指数集里的，默认值是正零
* `double`，它的值是double值集里的元素，或者是扩展double指数集里的，默认值是正零

`boolean`类型的值对事实的编码是`true`和`false`，默认值是`false`

>Java虚拟机规范第一版并没有把`boolean`作为一个JVM类型。然而，`boolan`值确实在JVM受到了限制性的支持。第二版把`boolean`作为一个类型解决了这个问题。

`returnAddress`类型的值是指向JVM指令操作数的指针。基本类型中，仅`returnAddress`类型没有和Java编程语言直接关联。

### 2.3.1 整形
JVM整形的值如下：
* `byte`，-128 到 127（-2^7 ~ 2^7 - 1)，包含
* `short`,-32768 到 32767（-2^15 ~ 2^15 - 1)，包含
* `int`, -2147483648 到  2147483647 (-2^31 to 2^31 - 1), 包含
* `long`, -9223372036854775808 到 9223372036854775807 (-2^63 to 2^63 - 1), 包含
* `char`, 0 到 65535 包含

### 2.3.2 浮点型
浮点型是`float`和`double`，在概念上它们和32位单精度和64位双精度格式IEEE 754值以及操作相关，它是由*IEEE Standard for Binary Floating-Point Arithmetic*定义。



































