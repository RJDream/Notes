# Java.Lang

## 1. Java.lang.String

String 表示一个字符串，所有字面的字符串都是String的实例，例如"abc"。

Strings是不变的，他们的值在被创建后是不可变的， 字符串缓冲区支持可改变的字符串，因为字符串是不可改变的，所以他们可以被共享。例如：

```java
String str = "abc";
//equivalent
char[] data = {'a','b','c'};
String str = new String(data);
```

String 提供了一些方法用于检查字符序列中的字符，如比较字符串，提取字符串，搜索字符串，创建字符串的拷贝，转换字符串的大小写等。

Java 为 String的连接和将其他对象转换为字符串提供了特殊支持。字符串的连接通过StringBuilder/StringBuffer的append方法实现，将其他对象转换为字符串通过定义在Object中的toString方法实现，java中的所有类都继承Object对象。



除非其他说明，传递一个null参数给String的构造器或方法将抛出一个NullPointerException异常。

## 2.比较字符串

==： 比较的是在内存中的地址是否一致，也就是说是否是同一个对象

​	Java在处理String的时候，使用字面量直接给String对象赋值时候会将该该字面量存储到一个静态池中,下次再使用字面量创建字符串的时候就直接将变量指向这个字符串对象，所以两次对象是一个对象。

使用new创建的字符串对象，会强制JVM创建一个新的字符串对象，所以两次创建的对象不是同一个对象，即使内容一样。

equals： 比较的是两个字符串对象的内容，只要内容一样，返回true。该方法返回boolean值

equalsIgnoreCase: 忽略大小写比较字符串。返回boolean值

compareTo(String b): 比较字符串的大小(字典顺序).



**CharSequence ** 是String， StringBuilder, StringBuffer 的父接口



char charAt(int index);

int compareTo(String b);

int compareToIgnoreCase(String b);

String concat(String b); 

boolean contains(CharSequence s);

String copyValueOf(char[] data);

String copyValueOf(char[], int offset, int count);

boolean endWith();

boolean startWith();

boolean equals(Object b);

boolean equalsIgnoreCase(Object b);

String format(Local l, String format, Object ...args);

String format(String format, Object ... args);

getBytes(Charset c); getBytes(String charset);

getChars(int begin, int end, char[] dst, int dstBegin);

indexOf(int ch);

indexOf(int ch, int fromIndex);

indexOf(String s);

indexOf(String s, int fromIndex);

String join(CharSequence delimiter, CharSequence ... args);将数组或集合中的字符串使用指定的分隔符（delimiter）连接起来

lastIndexOf();