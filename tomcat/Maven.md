# Tomcat-Manager

## 1. 介绍

在很多的生产环境中，能够不停止或重启整个容器就能够部署一个新的web应用或者卸载一个web应用的能力是非常有用的。另外，你可以请求一个应用出重新加载自己，甚至在server.xml中没有声明reloadable。



为了支持这些能力，Tomcat包含了一个默认的Manager web引用，它支持一下功能：

- 从一个未加载的war文件部署一个新的应用
- 根据指定的在当前服务器路径的上下文路径部署一个新的应用
- 列出当前以部署的应用，以及这些应用当前活动的session
- 重载一个web应用，以相应WEB-INF/classes or WEB-INF/lib 的 变化
- 列出操作系统或JVM的属性值
- 列出有效的全局JNDI源（List the available global JNDI resources, for use in deployment tools that are preparing `<ResourceLink>` elements nested in a `<Context>` deployment description.）
- 启动一个已停止的应用
- 停止一个应用（变为不可用），但是不卸载它
- 卸载一个web应用并删除它的document，除非document存在于文件系统中（不在webapp下）



一个默认的Tomcat安装包含一个Manager应用，添加一个Manager实例到一个新的host，安装manager.xml上下文配置文件到 `$CATALINA_BASE/conf/[enginename]/[hostname]`

示例：

~~~xml
<Context privileged="true" antiResourceLocking="false"
         docBase="${catalina.home}/webapps/manager">
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="127\.0\.0\.1" />
</Context>
~~~



如果你的Tomcat已经配置为多虚拟主机，你应该为每一个虚拟主机配置Manager。

Manager 有三种方式进行访问：

- 作为一个web应用，在浏览器访问用户界面，如访问：`http://localhost:8080/manager/html` .
- 最小版本的使用HTTP请求，它只适合系统管理员通过脚本进行操作。命令作为URL的一部分，响应是简单文本形式的，他可以很容易进行解析和处理
- 在Ant构建工具中定义一系列的方便的task。

## 2.配置Manager访问

使用Tomcat默认配置是非常危险的。因为Tomcat允许网络上的任何一个用户去执行Manager应用，所以当使用Manager的时候都需要进行用户的验证，使用用户名/密码，他们关联了`manager-xxx`的角色，这些角色决定了用户的权限。在默认的配置文件中没有包含默认用户，所以不定义user默认是无法访问`Manager`的

### 2.1 Rolename

你可以在Manager应用下的web.xml中找到roles name。

- manager-gui: 访问HTML接口
- manager-script：对于工具友好的简单的文本格式，以及Status权限
- manager-jmx：访问JMX 代理接口，以及Status权限
- manager-status：只能访问Server Status 页面



