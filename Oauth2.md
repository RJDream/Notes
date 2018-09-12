# OAuth2



```sequence
User -> agent : req
agent -> Client : req
Client -> User : (A)Authorization Request
User -> Client : (B)Authorization Grant
Client -> Authorization Ser: (C)Authorization Grant
Authorization Ser -> Client : (D)Access Token
Client -> Resource : (E)Access Token
Resource -> Client : (F)Protected Resource
Client -> agent : res


```



> （A）用户打开客户端以后，客户端要求用户给予授权。
>
> （B）用户同意给予客户端授权。
>
> （C）客户端使用上一步获得的授权，向认证服务器申请令牌。
>
> （D）认证服务器对客户端进行认证以后，确认无误，同意发放令牌。
>
> （E）客户端使用令牌，向资源服务器申请获取资源。
>
> （F）资源服务器确认令牌无误，同意向客户端开放资源。





不难看出来，上面六个步骤之中，B是关键，即用户怎样才能给于客户端授权。有了这个授权以后，客户端就可以获取令牌，进而凭令牌获取资源。

下面一一讲解客户端获取授权的四种模式。



## 授权模式

客户端必须得到用户的授权（authorization grant），才能获得令牌（access token）。OAuth 2.0定义了四种授权方式。

- 授权码模式（authorization code）
- 简化模式（implicit）
- 密码模式（resource owner password credentials）
- 客户端模式（client credentials）





### 授权码模式

授权码模式（authorization code）是功能最完整、流程最严密的授权模式。它的特点就是通过客户端的后台服务器，与"服务提供商"的认证服务器进行互动。 



==客户端要求授权==

```sequence
Client -> Agent : A
Agent -> User :B 
Agent -> Client : C - local to Authorization Server
```

==授权==

```sequence
Agent -> Auth : Client Identifier & Redirection URI
Agent -> Auth : User authenticates
Auth -> Agent : Authorization Code
```



![1531967400333](C:\B_docs\Notes\Oauth2.assets\1531967400333.png)

> （A）用户访问客户端，后者将前者导向认证服务器。
>
> （B）用户选择是否给予客户端授权。
>
> （C）假设用户给予授权，认证服务器将用户导向客户端事先指定的"重定向URI"（redirection URI），同时附上一个授权码。
>
> （D）客户端收到授权码，附上早先的"重定向URI"，向认证服务器申请令牌。这一步是在客户端的后台的服务器上完成的，对用户不可见。
>
> （E）认证服务器核对了授权码和重定向URI，确认无误后，向客户端发送访问令牌（access token）和更新令牌（refresh token）。



