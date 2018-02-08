# HttpComponents

Apache HttpComponents 项目负责创建和维持一个低级别的java组件工具集，它们关注于HTTP和其相关的协议



## Overview

Hyper Text Transfer Protocol 可以说是如今在Internet最流行的协议，web服务，使用网路的电器，以及因为网路计算任务而持续扩张的web浏览器，然而这些增长都需要http支持

。。。

## HttpCore

一组低级的HTTP传输组件，他可以使用最小的成本去构建自定义的客户端和服务端Http services。

HttpCore支持两种IO模式：

- 阻塞IO：基于经典的Java IO
- 非阻塞IO：基于Java NIO





# HttpClient

- 基于httpcore
- 基于经典的阻塞IO
- 内容无关的（不关心内容）



HttpClient 不是一个浏览器，他是一个客户端的Http传输库，他的目的是传输和接收HTTP消息。

他不会尝试去处理内容，执行嵌入在html中的js，猜测内容类型，如果没有明确地指定，或者重新格式化请求。



## 1.基本原理

### 1.1 执行请求

HttpClient 最基本的功能就是执行Http 方法。执行一个Http方法通常包含一个或多个 http请求/http响应的交换，这些都是在HttpClient内部处理的。

用户希望提供一个请求对象，HttpClient希望传输这个请求对象到目标服务器，并返回一个相关的reponse对象，或者当执行没成功的时候抛出一个Exception，

自然而然的，HttpClient 主要的入口是HttpClient接口，它定义了上面描述的这些约定。

下面是HttpClient 处理一个http请求最简单的形式：

```java
CloseableHttpClient client = HttpClients.createDefault();
HttpGet httpget = new HttpGet("http:localhost/");
CloseableHttpResponse response = client.execute(httpget);
try{
  ...
}finally{
  response.close();
}
```



#### 1.1.1 Http request

所有的请求都有一个请求行，它包含一个请求方式名称，一个请求URI，以及一个HTTP协议版本

```
GET /API/setp HTTP/1.1
```

HttpClient对Http 1.1 规范定义的所有请求方式都提供了开箱即用的类，

HttpGet,HttpPost,HttpPut,HttpDelete,Httptrace,HttpHead,HttpOptions

**Request-URI**是统一资源标识符，它定义了在请求的资源，他包含一个请求协议方案，主机名称，可选的端口，资源路径，可选的查询参数

```java
HttpGet httpget = new HttpGet("http://www.google.com/search?hl=en&q=httpclient&btnG=Google+Search&aq=f&oq=");
```

HttpClient 提供一个**URIBuilder**实用类用于简化创建或修改一个请求URIHtt

```java
URI uri = new URIBuilder()
  .setScheme("http")
  .setHost("www.google.com")
  .setPath("/search")
  .setParameter("q","HttpClient")
  .builder();

HttpGet get = new HttpGet(uri);
System.out.print(get.getURI);
```

```java
http://www.google.com/search?q=HttpClient
```



#### 1.1.2Http response

Http response 是服务器接收请求消息并解析请求消息后发送回来的消息。消息的第一行包含一个协议版本，一个状态码，一个描述文本

```
HTTP/1.1 200 OK
```

```java
HttpReponse reponse = new BasicHttpReponse(HttpVersion.HTTP_1_1, HttpStatus.SC_OK, "OK");
System.out.println(response.getProtocolVersion());
System.out.println(response.getStatusLine().getStatusCode());
System.out.println(response.getStatusLine().getReasonPhrase());
System.out.println(response.getStatusLine().toString());
```

```java
HTTP/1.1
200
OK
HTTP/1.1 200 OK
```





#### 1.1.3 Working with message headers

一个Http Message可以包含一系列的描述性的消息头，诸如content length，content type等等。HttpClient提供一些方法用于检索，添加，删除，列举这些消息头

```java
HttpResponse response = new BasicHttpResponse(HttpVersion.HTTP_1_1, HttpStatus.SC_OK,"OK");

response.addHeader("Set-Cookie","c1=a;path=/;domain=localhost");
response.addHeader("Set-Cookie","c2=b;path=\"/\",c3=c; domain=\"localhost\"");
Header h1 = response.getFirstHeader("Set-Cookie");
System.out.println(h1);
Header h2 = response.getLastHeader("Set-Cookie");
System.out.println(h2);
Header[] hs = reponse.getHeaders("Set-Cookie");
System.out.println(hs.length);
```

```
Set-Cookie: c1=a;path=/;domain=localhost
Set-Cookie: c2=b;path="/",c3=c; domain="localhost"
2
```









