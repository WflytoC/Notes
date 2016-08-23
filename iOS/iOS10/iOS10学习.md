####Cocoa Touch framework

Cocoa使用Objective-C，提供了特性：

* category
* protocol
* delegation
* notification
* memory management
* Key–value coding
* key–value observing



Swift3与Swift2相比，基本的语法发生了很大的变化，导致许多标准库和Cocoa方法的命名发生了变化；新的Foundation框架从某些类型名中移除了`NS`前缀。


---------------------------

####Swift3基础

* `一条语句`可以分为`多行`书写

* 对象，可以向其发送消息(message)，消息，就是一条指令。

* Swift有三种对象类型：`class`、`struct`、`enum`。

* 模块是由多个文件构成，这些文件能够自动互相使用；如果模块间不使用`import`，则无法互相使用。

* 在`文件顶层`(非类中)声明的变量是全局变量，所有的代码都可以访问它。

* 在`文件顶层`(非类中)声明的函数是全局函数，所有的代码都可以访问它。

* 可执行的代码不可以放在`文件顶层`

* 命名空间：

		class Manny {
		
			class Klass {}
		}
		其中，Manny是命名空间的名称，通过Manny.Klass来访问Klass
* 模块：顶层的命名空间是模块；默认，应用是一个模块，因此是命名空间，命名空间的名字就是应用名。


####函数

* 参数列表：
* 函数签名：
* 



