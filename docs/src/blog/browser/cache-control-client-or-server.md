
# 浏览器http请求缓存，cache-control是服务器设置，还是浏览器设置

服务器和浏览器都可以分别在响应头和请求头设置cache-control字段。

1. **如果都不设置，那么就不用缓存，毫无疑问**

2. **如果客户端设置，但是服务器没有设置**
    - 当且仅当，设置的值为是cache-control：max-age=0 ｜ no-cache ｜no-store时，才会生效，（此时代表着客户端明确表示自己不想使用强缓存）
    - 在点击浏览器的刷新按钮 / 或者直接按 f5 ，实际上就是给请求头设置了Cache-Control: max-age=0走协商缓存（if 条件请求）
    - ctrl + f5【command+shift+r】 强制刷新，实际上就是给请求头设置了Cache-Control: no-store 就是不使用任何缓存，直接请求服务器资源
    - 所以，浏览器用“Cache-Control”做缓存控制，在请求头中设置某些值，可以起到刷新数据的效果

3. **客户端没有设置，服务器设置了**
    - 当客户端设置了cache-control：max-age=0 ｜ no-cache ｜no-store这三个值时，服务器设置的无效
    - 客户端设置了其他值，以服务器设置的值为准，忽略请求头的cache-control

所以有的人果断的说，如果客户端和服务端同时设置了以服务端为准这句话是不正确的。

不过在实际应用中，建议不要纠结在哪里设置，客户端设置不生效，就服务端设置即可。**不同的浏览器可能有不同的处理**。


## 参考
[HTTP请求头和响应头中cache-control的区别](https://segmentfault.com/a/1190000038996958)