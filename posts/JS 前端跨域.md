#### overview

实现跨域的两种方式：客户端支持和服务端支持



[HTTP访问控制(CORS)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS) 



#### 服务端支持

原理：因为跨域了，要先option一下目标域名，拿到返回的header，用以判断是否允许当前域名发送跨域请求。

检测到Access-Control-Allow-Origin: * 之后就知道了 原来是允许的， 那么就开始POST/GET吧。。

OPTIONS请求方法的主要用途有两个：

1. 获取服务器支持的HTTP请求方法；也是黑客经常使用的方法。

2. 用来检查服务器的性能。例如：AJAX进行跨域请求时的预检，需要向另外一个域名的资源发送一个HTTP OPTIONS请求头，用以判断实际发送的请求是否安全。



PHP



    header('Access-Control-Allow-Origin: *');

    header('Access-Control-Allow-Methods: POST, GET, OPTIONS');

    header('Access-Control-Max-Age: 1000');

    if(array_key_exists('HTTP_ACCESS_CONTROL_REQUEST_HEADERS', $_SERVER)) {

        header('Access-Control-Allow-Headers: '

               . $_SERVER['HTTP_ACCESS_CONTROL_REQUEST_HEADERS']);

    } else {

        header('Access-Control-Allow-Headers: *');

    }



    if("OPTIONS" == $_SERVER['REQUEST_METHOD']) {

        exit(0);

    }



前端 JS

可以使用 jQuery 的 ajax 方法实现



原生 XMLHttpRequest 实现，注意可以通过 Chrome Devtools 观察网络请求，注意观察请求是否 POST 转化为 option 

[XMLHttpRequest changes POST to OPTION](http://stackoverflow.com/questions/8153832/xmlhttprequest-changes-post-to-option)



    let request = new window.XMLHttpRequest()

    request.open('POST', 'http://ip/api')

    request.setRequestHeader('Content-Type', 'text/plain; charset=UTF-8')

    request.send(data)

    request.addEventListener('readystatechange', function () {

      if (this.readyState === 4) {

        console.log(this.responseText)

      }

    })



注意事项：预请求转化条件 [预请求 MDN](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS#预请求) ,如果通过 XMLHttpRequest  mime 类型为 application/json ，后台服务器没有处理相应的 option 请求，可能会导致失败


