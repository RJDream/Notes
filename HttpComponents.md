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

更方便的方式是是使用迭代器

```java
HttpResponse response = new BasicHttpResponse(HttpVersion.HTTP_1_1, HttpStatus.SC_OK, "OK");
        response.addHeader("Set-Cookie", "c1=a;path=/;domain=localhost");
        response.addHeader("Set-Cookie", "c2=b;path=\"/\",c3=c; domain=\"localhost\"");

        HeaderIterator iter = response.headerIterator("Set-Cookie");
        while (iter.hasNext()){
            System.out.println(iter.next());
        }
```

```
Set-Cookie: c1=a;path=/;domain=localhost
Set-Cookie: c2=b;path="/",c3=c; domain="localhost"
```

以及方便的解析值的方法

```java
HttpResponse response = new BasicHttpResponse(HttpVersion.HTTP_1_1, HttpStatus.SC_OK, "OK");
response.addHeader("Set-Cookie", "c1=a;path=/;domain=localhost");
response.addHeader("Set-Cookie", "c2=b;path=\"/\",c3=c; domain=\"localhost\"");

HeaderElementIterator iterator = 
  new BasicHeaderElementIterator(response.headerIterator("Set-Cookie"));
while (iterator.hasNext()) {
  HeaderElement item = iterator.nextElement();
  NameValuePair[] pairs = item.getParameters();
  for (NameValuePair pair : pairs) {
    System.out.println(pair.toString());
  }
}
```

```
path=/
domain=localhost
path=/
domain=localhost
```





#### 1.1.4 Http entity

Http可以携带一个request/response相关的内容实体，这些实体可以在一些请求对象或响应对象中找到，在HTTP规范中，有两个请求方式可以携带内容实体：POST/PUT。而响应对象一般都希望携带内容实体。有一些例外情况诸如是HEAD请求，或者响应的是204 no content, 304 not modified, 205 reset content 。



Http 区分了三种实体， 如何区分取决于他们的内容来自哪里？

HttpClient distinguishes three kinds of entities, depending on where their content originates:

- streamed: 内容来自于流，或在流中生成。详细的说这类entity来自HTTP Response, 流式entity通常不能重用
- self-contained:内容来自于内存，或通过与连接和其他实体无关的方式获得。
- wrapping:包装的entity，内容来自于其他entity。


这些区别对连接管理很重要，当内容来自于一个HTTP响应时.请求实体被应用创建,且只能通过HttpClient发送.流式和独立的entity之间的区别没有那么重要,在这这种情况下,建议将不可重复的看作是流式的entity,而那些可以重复的看作是子包含的entity.

**可重复**entity意味着可以读取多次,这只可能是self-contanined(类似StringEntity,ByteArrayEntity)

因为binary和character都是可以重复的entity,所以它支持字符编码(支持后者).

entity在一个附件了内容的请求被执行或请求成功后服务端在响应body中返回结果给客户端的时候被创建.

在entity中读取内容,一个方法是使用HttpEntity#getContent()方法,它会返回一个InputStream.或者提供一个输出流,将结果输出到指定的地方,HttpEntity#writeTo(output);,他会一次性将所有的内容输出.






