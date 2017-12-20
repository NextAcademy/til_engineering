---
layout: post
title: "Cross Origin Resource Sharing"
author: "Ming Hao"
date: 2017-12-20 00:00:00 +0800
tags: [http]
preview: https://images.idgesg.net/images/article/2017/09/networking-100735059-large.jpg
---


Some time ago, I had to request information using JS from a 3rd party website. I'd initially thought that it would be as simple as making an XMLHttpRequest, but to my surprise, it didn't work. So after some research, I discovered what is known as the Cross Origin Resource Sharing (CORS) mechanism. While this ultimately wasn't able to solve my problem, it helped me realize how it is possible to access certain websites through XMLHttpRequests. 

Before we get into it though, here's the reason why you can't request information from 3rd party websites:

## The Reason - same-origin policy
On browsers, JS is held to something known as the same-origin policy, which dictates that a request through JS cannot be made to a server of a different origin. E.g: On a page `http://example.com`, JS cannot request information from another site with a URL `http://example.hello.com`. 

The composition of an origin in this context is as follows: 

+ The Protocol
+ The Domain
+ The Port

 A same-origin will need to have the same 3 attributes to pass the same-origin policy. It can only make requests to URLs like `http://example.com/request`. This is for security reasons, as it prevents the JS scripts in a non-same-origin webpage from making requests to a server. 

However, sometimes websites legitimately require access to third party information through JS, so the same-origin policy puts quite a dampener on things. The CORS mechanism is one of the solutions that exists to allow such access.

## Cross Origin Resource Sharing

For clarity's sake, we'll refer to the requesting server as **Client** and the 3rd party server as the **Server**. 

Essentially, CORS is meant to facilitate the sharing of resources across different origins.  This is done through the inclusion of specific headers in the response of the **Server**, which would include the specifications of allowed requests. 

We'll start off with the simplest scenario, which we'll refer to as the simple request. A simple request requires the following:
+ The method must either be GET, POST, or HEAD
+ Content-Type headers must be one of the following: 
  - `application/x-www-form-urlencoded`
  - `multipart/form-data`
  - `text/plain`
+ A limited set of header values that can be manually set ([Reference](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS#Simple_requests))
+ No event listeners on **XMLHttpRequestUpload** object
+ No **ReadableStream** object

#### Request
```
GET /all_origin HTTP/1.1
Host: localhost:3002
Connection: keep-alive
Accept: application/json, text/javascript, */*; q=0.01
Origin: http://localhost:3001
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36
Content-Type: text/plain
Referer: http://localhost:3001/
```

For every request that is sent to a cross-origin site, an Origin header is added, containing the following `<protocol>://<domain>:<port>`. 


#### Response
```
HTTP/1.1 200 OK
Content-Type: text/html;charset=utf-8
Access-Control-Allow-Origin: *
Content-Length: 32
X-Xss-Protection: 1; mode=block
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Server: WEBrick/1.3.1 (Ruby/2.4.1/2017-03-22)
Date: Wed, 20 Dec 2017 07:12:32 GMT
Connection: Keep-Alive
```

In order for the **Server** to allow this request to pass, the **Server** will have to append an `Access-Control-Allow-Origin` header with the **Client** origin as one of the values, there by allowing the request to go through. Additionally, one could set `Access-Control-Allow-Origin = *`, which would allow all 3rd party requests, however, this would pose a security risk and therefore is inadvisable.

## Preflight Request
Given the limited scope of simple requests, all other requests outside of this scope will need to make preflighted requests. A preflighted request will send a request using the OPTIONS method in advance, which serves to query the **Server** whether the actual request is safe to send. This is more secure as certain requests may have implications on the user data,

For example: a `DELETE` Method

#### Request
```
DELETE /delete_method HTTP/1.1
Host: localhost:3003
Connection: keep-alive
Accept: application/json, text/javascript, */*; q=0.01
Origin: http://localhost:3001
User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/61.0.3163.100 Safari/537.36
Content-Type: application/json
Referer: http://localhost:3001/
```

#### Response

```
HTTP/1.1 200 OK
Content-Type: text/html;charset=utf-8
Access-Control-Allow-Origin: *
Access-Control-Allow-Methods: DELETE
Access-Control-Allow-Headers: Content-Type
Content-Length: 32
X-Xss-Protection: 1; mode=block
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Server: WEBrick/1.3.1 (Ruby/2.4.1/2017-03-22)
Date: Wed, 20 Dec 2017 07:17:03 GMT
Connection: Keep-Alive
```
If the **Server** allows the request, by adding the header `Access-Control-Allow-Methods = "DELETE"`, it will then return a response that indicates that the **Client** is allowed to make such a request, for a certain period of time without issuing another preflight request until it expires, as indicated in the `Access-Control-Max-Age` header, after which a new preflight request needs to be issued. The **Client** can then send the actual `DELETE` request over.

## Conclusion
CORS is a useful tool to relax the same-origin policy, by allowing 3rd party servers to selectively allow cross-origin requests access. However, it must be implemented on the server side, hence why is was not the solution to my previous problem as the 3rd party server I was contacting did not implement CORS. If you're interested in how to circumvent this though, you can use your own server as an intermediary to make the request to the 3rd party server, as sending a request to your own server does not violate the same-origin policy.