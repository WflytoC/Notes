过滤器(`filter`)用于拦截请求，处理ServletRequest或ServletResponse，也可用来打印，加密、解密，session跟踪，图片转换等等。

过滤器可以配置用来拦截一个或多个资源，过滤器可以通过注解或`deployment descriptor`来配置。如果多个过滤器适用于相同的资源，则调用的顺序非常重要的，这种情况下需要`deployment descriptor`。

用在`filter`中的接口包括：`Filter`、`FilterConfig`和`FilterChain`。过滤器类必须要实现`javax.servlet.Filter`接口，该接口暴露三个生命周期方法：`init`，`doFilter`和`destroy`。

```
void init(FilterConfig filterConfig);
```

当过滤器放入服务器中时，`init`方法就会被servlet容器调用。每次与filter相关联的资源被调用时，`Filter`实例的`doFilter`方法就会被servlet容器调用。

```
void doFilter(ServletRequest request,ServletResponse response,FilterChain filterChain)
```

`doFilter`方法实现的最后一行代码应当是调用`FilterChain`的`doChain`方法。

```
filterChain.doFilter(request,response)
```

一个资源可能关联多个过滤器(或链式过滤器)，`FilterChain.doFilter()`通常会造成链中的下一个过滤器被调用。对链中的最后一个过滤器调用`FilterChain.doFilter`会造成资源自身被调用。

写完一个过滤器类后，必须要配置过滤器类。

* 决定过滤器拦截哪些资源
* 搭建初始化值来传递给过滤器的init方法
* 给过滤器名称

`FilterConfig`接口可以让你通过它的`getServletContext`方法来获取ServletContext,通过它的getFilterName()方法来获取过滤器的名称。

如果想要获取传递给过滤器的初始化值，有两种方法：

```
Enumeration<String> getInitParameterNames() //返回过滤器参数名
```

```
String getInitParameter(String parameterName)
```

有两种方式来配置过滤器，可以使用WebFilter注解类型或者在`deployment descriptor`中注册。

```
package com.ccmobile.filter;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.Date;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import javax.servlet.annotation.WebInitParam;
import javax.servlet.http.HttpServletRequest;

import com.sun.org.apache.bcel.internal.generic.NEW;

@WebFilter(filterName = "LoggingFilter",urlPatterns = { "/*" },
			initParams = {
					@WebInitParam(name = "logFileName",value = "log.txt"),
					@WebInitParam(name = "prefix",value = "URI: ")
			}
		)
public class LoggingFilter implements Filter{
	
	private PrintWriter logger;
	private String prefix;
	
	@Override
	public void init(FilterConfig filterConfig) throws ServletException {
		prefix = filterConfig.getInitParameter("prefix");
		String logFileName = filterConfig.getInitParameter("logFileName");
		String appPath = filterConfig.getServletContext().getRealPath("/");
		System.out.println("logFileName = " + logFileName);
		try {
			logger = new PrintWriter(new File(appPath,logFileName));
		}catch (FileNotFoundException e) {
			e.printStackTrace();
			throw new ServletException(e.getMessage());
		}
	}
	
	@Override
	public void destroy() {
		System.out.println("destroy filter");
		if (logger != null) {
			logger.close();
		}
	}
	
	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain filterChain)
			throws IOException, ServletException {
		System.out.println("LoggingFilter.doFilter");
		HttpServletRequest httpServletRequest = (HttpServletRequest)request;
		logger.println( new Date() + " " + prefix + httpServletRequest.getRequestURI());
		logger.flush();
		filterChain.doFilter(request, response);
		
	}
}
```


图片保护过滤器防止图片直接被下载(在浏览器地址栏中直接输入图片URL)。过滤器的工作原理是检查`referer`这个HTTP头的值，null值表示当前请求没有referrer。
```
package com.ccmobile.filter;

import java.io.IOException;

import javax.servlet.Filter;
import javax.servlet.FilterChain;
import javax.servlet.FilterConfig;
import javax.servlet.ServletException;
import javax.servlet.ServletRequest;
import javax.servlet.ServletResponse;
import javax.servlet.annotation.WebFilter;
import javax.servlet.http.HttpServletRequest;

@WebFilter(filterName = "ImageProtectorFilter",urlPatterns = { "*.png","*.jpg" })
public class ImageProtectorFilter implements Filter{
	
	@Override
	public void init(FilterConfig filterConfig) throws ServletException {

	}
	
	@Override
	public void destroy() {

	}
	
	@Override
	public void doFilter(ServletRequest request, ServletResponse response, FilterChain filterChain)
			throws IOException, ServletException {
		System.out.println("ImageProtectorFilter");
		HttpServletRequest httpServletRequest = (HttpServletRequest)request;
		String referer = httpServletRequest.getHeader("referer");
		if (referer != null) {
			filterChain.doFilter(request, response);
		} else {
			throw new ServletException("Image not available");
		}
	}
}
```