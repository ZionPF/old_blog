---
layout: post
title: "在Ubuntu上设定自己的Proxy Server"
description: ""
category: slides
tags : [Network Paper]
---

以后的文章，特别是技术类的，我会尽量用英文。。。主要是有些时候真心觉得中文感觉有点儿怪。。。

# Setting HTTP Proxy For Ubuntu Servers

Once you have a HTTP Proxy, it's quite easy to use for Ubuntu users, take apt-get for example, just type :

    export http_proxy="http://yourproxyserver.com:8080/"

Where "yourproxyserver.com" is your url/IP of your proxy server, and 8080 should be replaced by your real proxy port.

And you will be able to apt-get with your proxy. To make the change permanent, it's better to do this in /etc/environment:

    http_proxy="http://yourproxyserver.com:8080/"
    https_proxy="http://yourproxyserver.com:8080/"
    ftp_proxy="http://yourproxyserver.com:8080/"
    HTTP_PROXY="http://yourproxyserver.com:8080/"
    HTTPS_PROXY="http://yourproxyserver.com:8080/"
    FTP_PROXY="http://yourproxyserver.com:8080/"

Now that's perfect if you are using https link for your github, but what if you use git: types?

Not that hard, simply type the following command in your command line:

    git config --global http.proxy http://yourproxyserver.com:8080/

Now, if you have a cluster of Ubuntus like me, simply make the above commands a shell script, or you could just copy from here:


[lazy script for http proxy in ubuntu ](https://github.com/HolySparky/shells/blob/master/http_proxy_ubuntu.sh)

