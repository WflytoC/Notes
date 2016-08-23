
####知识点

* `WebDAV(Web-based Distributed Authoring and Versioning)`是一种基于 `HTTP 1.1`协议的通信协议。它扩展了HTTP 1.1，在GET、POST、HEAD等几个HTTP标准方法以外添加了一些新的方法，使应用程序可直接对`Web Server`直接读写，并支持写文件锁定(`Locking`)及解锁(`Unlock`)，还可以支持文件的版本控制。

* `Keep-Alive`功能使客户端到服务器端的连接持续有效，当出现对服务器的后继请求时，`Keep-Alive`功能避免了建立或者重新建立连接。市场上 的大部分Web服务器，包括`iPlanet`、`IIS`和`Apache`，都支持`HTTP Keep-Alive`。对于提供静态内容的网站来说，这个功能通常很有用。但是，对于负担较重的网站来说，这里存在另外一个问题：虽然为客户保留打开的连 接有一定的好处，但它同样影响了性能，因为在处理暂停期间，本来可以释放的资源仍旧被占用。当Web服务器和应用服务器在同一台机器上运行时，Keep- Alive功能对资源利用的影响尤其突出。

－－－－－－－－－－－－－－－－－－－－－－－－－－－

`GCDWebServer`是基于GCD的HTTP1.1服务器，嵌入在OS X和iOS应用中。

拓展：

* `GCDWebUploader`：

		GCDWebUploader：GCDWebServer的子类，提供使用web浏览器上传、下载文件的接口。
* `GCDWebDAVServer`：

		GCDWebDAVServer：GCDWebServer的子类，实现了WebDAV服务器
		
		
不支持的方面：

* `Keep-alive`连接
* `HTTPS`


####搭建环境

[GCDWebServer的Github地址](https://github.com/swisspol/GCDWebServer)

* 将整个`GCDWebServer`子目录添加到Xcode工程中，如果想要使用`GCDWebDAVServer`或者`GDCWebUploader`，也要添加这些子目录；

* 链接到`libxml2`和`libz`：`Target > Build Phases > Link Binary With Libraries`

* 将`$(SDKROOT)/usr/include/libxml2`添加到`header search paths`中：`Target > Build Settings > HEADER_SEARCH_PATHS`

Swift的架接文件中导入头文件：

```
#import "GCDWebServer.h"
#import "GCDWebServerDataResponse.h"
```

####Hello,World

```

    func initWebServer() {
        
        let webServer = GCDWebServer()
        webServer.addDefaultHandlerForMethod("GET", requestClass: GCDWebServerRequest.self) { request in
            
            return GCDWebServerDataResponse(HTML: "<html><body><p>Hello,World</p></body></html>")
        }
        webServer.startWithPort(8080, bonjourName: "GCD Web Server")
        
        print("Visit \(webServer.serverURL) in your web browser")
    }
```

注意：开启服务器的视图控制器进入后台时，服务器会停止。


####iOS应用中基于web的上传

`GCDWebUploader`是`GCDWebServer`的子类，提供了随时可用的HTML5上传和下载的功能。允许用户在浏览器中使用界面操作iOS应用沙盒中的文件(包括上传、下载、删除、创建目录)

```
func initWebServer() {
        
        let webServer = GCDWebServer()
        webServer.addDefaultHandlerForMethod("GET", requestClass: GCDWebServerRequest.self) { request in
            
            return GCDWebServerDataResponse(HTML: "<html><body><p>Hello,World</p></body></html>")
        }
        webServer.startWithPort(8080, bonjourName: "GCD Web Server")
        
        print("Visit \(webServer.serverURL) in your web browser")
    }
```






