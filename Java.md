# Java

## 1.Java 程序设计环境

### 1.1 下载JDK

专业术语

| Term                     | 缩写   | 解释                        |
| ------------------------ | ---- | ------------------------- |
| Java Development Kit     | JDK  | 编写Java程序的人员使用的软件          |
| Java Runtime Environment | JRE  | 运行Java程序的用户使用的软件          |
| Standard Edition         | SE   | 用于桌面或简单服务器的应用的Java平台      |
| Enterprise Edition       | EE   | 用于复杂的服务器应用的Java平台         |
| Micro Edition            | ME   | 用于手机和其他小型设备的java平台        |
| Java 2                   | J2   | 过时术语，描述1998~2006之间的Java版本 |
| Software Development Kit | SDK  | 过时术语，描述1998~2006之间的JDK版本  |
| Update                   | u    | Oracle术语，用于发布修改的bug       |
| NetBeans                 |      | Oracle的集成开发环境             |

### 1.2 设置执行路径

在JDK安装完成后还需要配置执行路径：将jdk/bin目录添加到执行路径





## 2.Java 的基本程序设计结构

~~~java
public class FirstSample{
  public static void main(String[] args){
    System.out.println("We will not user 'Hello world!'");
  }
}
~~~

1.Java 对大小写敏感

2.public 关键字称为访问修饰符

3.class 关键字表明java程序中的全部内容都包含在类中

4.FirstSample 类名，以字母开头，后面可以是任何字母数字下划线，不能使用保留字

5.java 程序使用分号作为语句的结束符，所以一条语句可以写在多行。

6.java使用双引号分割字符串

7.**java 程序必须包含一个main方法**

8.java 程序的语句都包含在{}内

9.java程序使用.调用方法

### 2.1 注释

java注释有三种

~~~java
// 单行注释
/* 多行注释 */ 
/**
 * javadoc注释，用于生成java doc文档
 */
~~~

### 2.2 数据类型

1.java是一种强类型语言，必须为每一个**变量**声明一种**类型**（type）。**java包含8中基本数据类型。**其中包含4种整型（byte，short,int,long），2种浮点型(float,double)，一种字符型(char)，一种表示真值的boolean型.



#### 2.2.1整形

用于表示没有小数部分的数值，可以是负数。

| type  | scope                                    |
| ----- | ---------------------------------------- |
| byte  | -128 ~ 127                               |
| short | -32768 ~ 32767                           |
| int   | 大概正负21亿                                  |
| long  | － 9 223 372 036 854 775 808 ～ 9 223 372 036 854 775 807 |



int是最常用的。

在java中，整型的范围与运行java程序的机器无关（可移植性,在每个平台上的数据范围都是固定的）。

长整型后跟一个L/l表示。

java 7 开始支持0b开头的二进制书写方式,0b1001.

java 7 开始支持下划线**_**作为数字分隔符，增加可读性，1000_0000

java 中所有的数据类型的第一位都是符号位，所以java中没有无符号类型。

```java
int a = 1000_0000;
int b = 0b1001;//9
int c = 0x10;//16进制
int d = 010;//八进制
```



#### 2.2 浮点型

用于表示有小数的数值。

| type   | scope                                    |
| ------ | ---------------------------------------- |
| float  | 大约±3.402 823 47E ＋ 38F（有效位数为6 ～ 7 位）     |
| double | 大约±1.797 693 134 862 315 70E ＋ 308（有效位数为15 位） |

java 中只有很小数的情况使用float。

float后面需要跟一个F/f,否则默认为double。

**浮点数值不适用于禁止有舍入误差的金融计算中。**

如果2（2.0-1.0）不会是期待的0.1，因为在二进制系统中无法表示1/10，就像是在十进制中无法表示1/3.



#### 2.3 字符型

char用于表示单个字符，使用单引号分割。

范围从\u0000 到\uffff;

**强烈建议在程序中不要使用char类型。**

#### 2.4 boolean

只包含两个值，true/false。用于判断

```java
boolean has = true;
if(has){
  ...
}else{
  ...
}
```



### 2.3 变量

在java中每一个变量属于一个类(type).

```java
int a;
double b;
char c;
```

变量名必须是以字母开头，有字母数字构成的序列。

在java中，数字和字幕的范围比其他语言大，包括A-Z,a-z,以及在其他语言中代表字母的任何unicode字符。如希腊使用π，同样0-9也是如此。

```java
int 年龄 = 12;//合法但是强烈不推荐
```

$:虽然合法但是不建议使用，因为这个字符通常用于java编译器或其他工具生成的名字中。

java变量名不能是java保留字。



**初始化**

声明一个变量后，必须使用赋值语句进行显示初始化，不能使用没有经过初始化的变量

```java
int b; //声明
b = 5; //初始化
int c = 25; //声明并初始化
```

变量的声明应该在第一次使用的时候（越晚声明变量越好）



**常量**

在java中使用final声明常量。**const是java保留字**

```java
final int CONST_A = 25;
```



### 2.4 运算符

算术运算符：+，-，*，/，%

自增：+=，

自减：-+，

*=，/=，

关系运算符：

	>, <,=,<=,>=,!=,&&,||,&,|

三元操作符

expression1?val1:val2



### 2.5 数值类型之间的类型转换

如果计算的时候两个操作数的类型不一致，那么将会发生类型转换

- 如果其中一个操作数是double，另一个操作数被转换为double
- 否则，如果其中一个操作数是float，另一个操作符被转换为float
- 否则，如果其中一个操作数是long，另一个操作数被转换为long
- 否则两个操作符都将被转换为int

**java 中所有整形的字面量都是int类型的**

不会发生进度丢失的转换

byte->short-int->long

int -> double, char->int

会发生精度丢失的转换

int -> float

long -> float -> double



#### 强制类型转换

在有些时候需要将范围比较大的数据类型转为范围比较小的类型，例如将double转为int类型。

java允许此类转换，但是需要强制类型转换，因为这可能丢失一些信息。

```java
double a = 9.99;
int b = (int)a;
System.out.println(b); //9
```

强制类型转换通过截取小数部分，将浮点数转换为整数。如果希望通过舍入到最近的整数，可以通过Math.round(doub);但是round返回的long，仍然需要强制类型转换。

```java
double a = 9.99;
int b = (int)Math.round(a);
System.out.println(b);//10
```

如果强制类型转换的时候，范围超出目标类型的范围，得到的结果会是一个完全不同的值

```java
int a = 300;
byte b = (byte)a;
System.out.pringln(b);//144
```



### 2.6 运算级别



| 运 算 符                                    | 结 合 性 |
| ---------------------------------------- | ----- |
| [ ] . ( ) ( 方法调用)                        | 从左向右  |
| ! ~ ++ -- + ( 一元运算) - ( 一元运算) ( ) ( 强制类型转换) new | 从右向左  |
| */ %                                     | 从左向右  |
| + –                                      | 从左向右  |
| << >> >>>                                | 从左向右  |
| < <= > >= instanceof                     | 从左向右  |
| = = !=                                   | 从左向右  |
| &                                        | 从左向右  |
| ^                                        | 从左向右  |
| \|                                       | 从左向右  |
| &&                                       | 从左向右  |
| \|\|                                     | 从左向右  |
| ?:                                       | 从右向左  |
| = += – = *= /= %= &= \|= ^= <<= >>= >>>= | 从右向左  |

运算符级别可以使用括号进行升级（常用做法）

### 2.7 枚举

用于声明有限个数的数据类型。表示声明的变量的取值只能在有限的集合范围内

```java
enum Size{
  SMALL,MEDIUM,LARGE,EXTRA_LARGE
}

Size s = Size.MEDIUM;
```

### 2.8 字符串

从概念上讲，Java 字符串就是Unicode 字符序列，使用双引号括起来。

```java
String e = "";//empty string
String greeting = "Hello";
```

#### 1 子串

```java
String greeting = "Hello world";
String s = greeting.substring(0,3);//substring(start,end); 包含开始，不包含结束
System.out.println(s);//Hel
```

#### 2 拼接

```java
String a = "Hello";
String b = "World";
String c = a+b; //java使用+拼接字符串
```

任何非字符串与字符串相连，非字符串都会被转换为字符串。

####3 不可变字符串

java中的String类没有提供直接修改String中字符的方法，所以如果需要修改字符串可以直接使用拼接,或将变量指向另一个字符串。

```java
String a = "Hel";
String b = a + "p!";
a = "Help";
```

java 中对字符串的拼接和重新复制都是重新创建了一个新的字符串；

通过拼接创建字符单元效率不高，但是不可变字符串可以使编译器让**字符串共享**。

字符串共享可以想象为:将字符串存放在一个公共的存储池中。字符串变量指向池中的相应位置。如果复制一个字符串变量，原始的字符串变量和复制的字符串变量指向同一个位置。

```java
String str1 = "Hello";
boolean same = "Hello" == str1;
System.out.println(same); //true

String str2 = "Hello" + "World";
String str3 = "HelloWorld";
System.out.println(str2 == str3);//true

String str4 = new String("HelloWorld");
System.out.println(str3 == str4);
```

java 认为相同的字符串所指向的内存地址是一样的，也就是说，"Hello"和变量str1 是同一块内存空间。

而对于在编译器就可以计算出来的字面量值，java会直接在内存中开辟最终结果的内存空间，所以"Hello" + "World"和str3也是同一块内存空间。

new String();强制JVM开辟了一块新的内存空间，也就是说**Java只将字符串常量（字面量）共享**，所以str3和str4不是同一块内存空间。

总而言之，Java设计者认为字符串共享带来的效率远远高于拼接，提取字符串带来的低效率。其实在程序中和很少需要修改字符串，而字符串的比较更多（例外的是将文件中的或键盘输入的较短的字符串拼接为一个长字符串，Java提供了额外的class应对这种情况）。



#### 4 比较字符串

```java
String a = "Hello";
String b = "hello";
System.out.println(a.equals(b));//false
System.out.pringln("Hello".equals(a)); //true
System.out.pringln(a.equalsIngoreCase(b));//true 不区分大小写
```





#### 5.Methods of String

##### 5.1 Constructor

| Constructor                              | Means                                    |
| ---------------------------------------- | ---------------------------------------- |
| new String()                             | 创建一个空的字符串对象                              |
| new String(byte[] bytes)                 | 使用平台默认的字符集和提供的解码的字节数组构建一个字符串             |
| new String(byte[] bytes, int offset, int length) | 从给定的字节数组中的第offset个字节开始，构建length个长度的字符串。  |
| new String(byte[] bytes, int offset, int length, Charset charset) | 以指定的字符集，从给定的字节数组中的第offset个字节开始，构建length个长度的字符串。 |
| new String(byte[] bytes, int offset, int length, String charsetName) | 以指定的字符集名称，从给定的字节数组中的第offset个字节开始，构建length个长度的字符串。 |
| new String(byte[] bytes, String charsetName) | 以指定的字符集名称，字节数组构建一个字符串                    |
| String(char[] value)                     |                                          |
| String(StringBuffer buffer)              |                                          |
| String (StringBuilder builde)            |                                          |



##### 5.2 Methods

| methods                                  | means                                    |
| ---------------------------------------- | ---------------------------------------- |
| char charAt(int index)                   |                                          |
| boolean comparTo(String other)           | 以字典顺序比较两字符串的大小，基于String中每个字符的unicode value比较。如果this 字符序列在参数String之前返回正数，如果this字符序列在参数String之后返回负数，如果两个自负序列相等，返回0; 如果两个字符串不想等，要么两个字符串对象所表示的字符序列在某些索引下的字符不想等，要么两个字符序列的长度不等，或都有。如果两个字符串所表示的字符序列存在不相等的字符，假设k是最小的那个索引，比较的就是这个位置上的两个字符大小。this.charAt(k)-another.charAt(k);如果两个字符串的字符序列中没有不相同的字符，那么比较的就是长度。this.length() - another.length(); |
| contains(CharSequence s)                 | 当String包含特定的 字符序列时返回true，CharSequence可以是String，StringBuilder,StringBuffer 等 |
| endWith(String suffix)                   |                                          |
| startWith(String perfix)                 |                                          |
| equals(Object object)                    |                                          |
| fomat(Locale l,String format,Object ... args) |                                          |
| format(String format, Object ... args)   |                                          |
|                                          |                                          |

##### 5.3 formatter String'

public static String format(Locale l, String format, Object ... args);

public static String format(String format, Object ... args);

如果参数l是null，那么本地化将不会有效（没有exception）

- java.util.Formatter

  格式化

一个格式翻译器，提供了支持正确布局和对齐。通常为数字，时间，日期，字符串提供格式化，通常类型可以是如type，BigDecimal，Calendar等。

Formatter为用户自定义的类提供格式化支持



每一个格式化方法都需要一个格式字符串和一个参数列表。格式字符串可以包含固定的文本和一些格式说明符。

```java
Calendar c = Calendar.getIntance();
String s = String.format("Duke's Birthday : %1$tm %1$te %1$tY", c);
```



syntax

%\[argument_index$\]\[flags]\[width][.precision]conversion 

| key            | mean                                     |
| -------------- | ---------------------------------------- |
| argument_index | (optional)十进制整数，用于指明参数列表中的参数的位置，第一个参数使用1$,第二个参数使用2\$ |
| flags          | (optional)一个字符集合，用于修饰输出格式 The set of valid flags depends on the conversion |
| width          | (optional)一个十进制正整数，表示最少输出的字符数量           |
| .precision     | (optional)非负整数，通常用于约束字符的数量，特定的行为取决于conversion |
| conversion     | (required)一个字符，表示参数需要怎样格式化， The set of valid conversions for a given argument depends on the argument's data type。 |







