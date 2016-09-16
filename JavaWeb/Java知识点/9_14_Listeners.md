`Servlet API`携带用于事件驱动的编程的事件类和监听器。所有的事件类都继承自java.util.Event，监听器可以在三个不同的级别获取：`ServletContext`，`HttPSession`和`ServletRequest`。

用于创建监听器的监听器接口属于`javax.servlet`和`javax.servlet.http`包：

 * `javax.servlet.ServletContextListener`：响应`servlet context`生命周期事件的监听器。其中一个方法是在`servlet context`刚创建的时候调用，另一个方法是在`servlet context`关闭之前调用。
 
 * `javax.servlet.ServletContextAttributeListener`：监听`servlet context`的属性被添加，移除或取代。

 * `javax.servlet.http.HttpSessionListener`：用来响应HttpSession的创建、超时、失效
 
  * `javax.servlet.http.HttpSessionAttributeListener`：当session属性被添加、移除或取代的时候，该监听器会被调用
 
 *  `javax.servlet.http.HttpSessionActivationListener`：当HttpSession被激活或钝化的时候，该监听器会被调用
 
 * `javax.servlet.http.HttpSessionBindingListener`：如果一个类的实例会被作为HttpSession属性存储，可以实现这个接口。
 * `javax.servlet.ServletRequestListener`：该接口会响应ServletRequest的创建和移除
 
 * `javax.servlet.ServletRequestAttributedListener`：当ServletRequest的属性被添加、移除或取代时，该监听器的方法就会被调用。
 
 * `javax.servlet.AsyncListener`：用来处理异步操作的监听器

 有两种方式来注册监听器，以便被`servlet container`识别。第一个是使用`WebListener`注解类型：
 
 ```
 @WebListener
 public class ListenerClass implements ListenerInterface {

}
 ```
 
 第二种是在`deployment descriptor`中使用listener元素注册监听器：
 
 ```
 <listener>
 	<listener-class> ... </listener-class>
 </listener>
 ```
 
 