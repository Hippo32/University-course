# Cookie #
Cookie是一些存储在客户端的信息，每次连接的时候由浏览器向服务器递交，服务器也向浏览器发起存储Cookie的请求，依靠这样的手段服务器可以识别客户端。

cookie是客户端请求服务时，服务端记录的用户信息，存储在客户端，下一次客户端发送请求时会将cookie一起发送。当客户端访问web服务器时，服务器会记录用户的信息然后在响应信息的响应头中设置set-cookie头部，然后存储在客户端，下一次再请求web服务器时请求头中会加上cookie这一项并发送到服务器，服务器通过cookie判断用户是否访问过该网站。

cookie是有时限的，有一个属性maxAge可以设置cookie的存储时间，超过时间后cookie会被删除，默认的是浏览器关闭时清除cookie。cookie一般用于用户的自动登录，记住密码等，将账户信息保存在cookie中，登录时cookie被传送到服务器完成自动登录。

## HTTP会话功能 ##
浏览器首次向服务器发起请求时，服务器生成一个唯一标识符并发送给客户端浏览器，浏览器将这个唯一标识符存储在Cookie中，以后每次再发起请求，客户端浏览器都会向服务器传送这个唯一标识符，服务器通过这个唯一标识符来识别用户。

# session #
session是一种标识对话的技术说法。通过session，我们能快速识别用户的信息，针对用户提供不一样的信息。

session和cookie的作用是一样的，也是存储用户信息，但是session是存储在服务器端的。session还需借助cookie将唯一标识sessionID存到客户端。当客户端访问web服务器时，会生成一个全局唯一标识sessionID，然后服务器会记录用户的信息然后存储到该session中，由于客户端下一次需要匹配sessionID，故需要将色杀死哦你ID发送到客户端，方法有两种，一种就是设置set-cookie头将sessionID发送到客户端，另一种就是url重写。第二次请求该网站时，请求头中就会带上cookie，cookie的值就是sessioID。session一般用于用户的身份验证，即利用sessionID判断用户是否合法。

注意：session数据不会保存在cookie本身，只是sessionID。session数据存储在服务器端。

session的技术实现上：会对一次对话产生一个唯一的标识进行标识。

## session生命周期 ##
session标识产生的时机和清除时机：

1. 用户已经登录：这个唯一标识会在用户进入网站时产生，用户点击退出时或者关闭浏览器时清除。
2. 用户未登录：这个唯一标识会在用户进入网站时产生，用户关闭所有网站相关页面时清除。

session生命周期：在生成和清除之间，在网站内的页面任意跳转，session标识不会发生变化。

从session开始到清除，我们叫一次会话，也就是生成session。

## session使用流程 ##
1. 用户登录后，将sessionid存到cookie中。
2. 用户在请求的网站别的服务时，由浏览器请求带上cookie，发送到服务器。
3. 服务器拿到sessionid后，通过该id找到保存在服务器的用户信息。
4. 然后再根据用户信息，进行相应的处理。

session登录验证的流程大致为：客户端若在未登录的状态下请求主页，那么服务器将该请求重定向到登陆页面；客户端在登录后，服务器需要记录保存该客户端的登录状态，并给予一个活动期限，这样下一次服务器请求主页的时候，就能够判断该客户端的登录状态，若登录状态有效，直接返回客户端需要的页面，否则重定向到登陆页面。


# cookie 与 session 的区别 #
1. cookie存储在浏览器（有大小限制），session存储在服务端（没有大小限制）
2. 通常session的实现是基于cookie的，session id存储于cookie中
3. session更安全，cookie可以直接在浏览器查看甚至编辑

参考：

- [express-session](https://www.npmjs.com/package/express-session)
- [express-session中文](https://blog.csdn.net/liangklfang/article/details/50998959)