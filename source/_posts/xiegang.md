---
title: nginx 带/ 的那点事(总是会忘)
date: 2020-04-01 10:23:24
index_img: img/nginx.png
tags: nginx   
categories: nginx   
---

注意：当location为正则表达式匹配模式时，proxy_pass中的url末尾是不允许有"/"的，因此正则表达式匹配模式不在讨论范围内。

proxy_pass配置中url末尾带/时，nginx转发时，会将原uri去除location匹配表达式后的内容拼接在proxy_pass中url之后。

测试地址：http://192.168.171.129/test/tes.jsp

场景一：

```
location ^~ /test/ {
proxy_pass http://192.168.171.129:8080/server/;

}
```

代理后实际访问地址：http://192.168.171.129:8080/server/tes.jsp

场景二：

```
location ^~ /test {
proxy_pass http://192.168.171.129:8080/server/;

}
```

代理后实际访问地址：http://192.168.171.129:8080/server//tes.jsp

场景三：

```
location ^~ /test/ {
proxy_pass http://192.168.171.129:8080/;

}
```

代理后实际访问地址：http://192.168.171.129:8080/tes.jsp

场景四：

```
location ^~ /test {
proxy_pass http://192.168.171.129:8080/;

}
```

代理后实际访问地址：http://192.168.171.129:8080//tes.jsp

proxy_pass配置中url末尾不带/时，如url中不包含path，则直接将原uri拼接在proxy_pass中url之后；如url中包含path，则将原uri去除location匹配表达式后的内容拼接在proxy_pass中的url之后。

测试地址：http://192.168.171.129/test/tes.jsp

场景一：

```
location ^~ /test/{
proxy_pass http://192.168.171.129:8080/server;

}
```

代理后实际访问地址：http://192.168.171.129:8080/servertes.jsp

场景二：

location ^~ /test {
proxy_pass http://192.168.171.129:8080/server;

}

代理后实际访问地址：http://192.168.171.129:8080/server/tes.jsp

场景三：

location ^~ /test/ {
proxy_pass http://192.168.171.129:8080;

}

代理后实际访问地址：http://192.168.171.129:8080/test/tes.jsp

场景四：

location ^~ /test {
proxy_pass http://192.168.171.129:8080;

}

代理后实际访问地址：http://192.168.171.129:8080/test/tes.jsp

总结：

如果proxy_pass中配置的地址中带/，不论/在不在末尾(即：不论/是不是最后一个字符)，都会把访问链接uri中匹配到location表达式的部分去掉，然后在拼接到proxy_pass的url之后



