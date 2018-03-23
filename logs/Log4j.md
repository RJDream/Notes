# 

## 摘要

log4j 允许开发人员以任意粒度输出日志信息，log4j可以在运行时使用可扩展文件进行配置，最好的是它非常容易学习。



## 三大组件

log4j包含三个主要的组件Logger,Appender,Layout. 这三个组件一起工作使得开发人员可以根据消息的类型和级别记录信息，并且在运行时控制这些消息输出的格式以及输出的目的地。

### Logger 继承

日志API相较于简单的System.out而言的优势在于有能力去禁止一些日志的语句而不影响其他的日志的输出.这个能力可以根据开发人员指定的类型对所有的日志语句进行分类。

Loggers是一些被命名的实体。Logger的名字是大小写敏感的，并且遵循分层命名规则。

如果Logger A的名字加点（.）是另一个Logger B 的名字的前缀，那么Logger A就是Logger B 祖先Logger。



例如：com.foo 这个Logger是com.foo.Bar 的父级Logger。

java 是java.util 的父级Logger， 是java.util.Vector 的祖Logger



RootLogger 总是在Logger继承树的最上层，它与其他Logger有两个区别：

- 总是存在
- 无法通过名字获取它



如果要调用RootLogger， 可以使用Logger.getRootLogger();





### Logger Level

log4j 定义了OFF, FATAL, ERROR, WARN, INFO, DEBUG, ALL 这几个日志级别。