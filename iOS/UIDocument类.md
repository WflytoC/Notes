`UIDocument`：

#### 子类化`UIDocument`类

`UIDocument`是一个抽象类，它不可以直接实例化。应用必须要创建`UIDocument`的子类，至少要重写两个方法：
 
 * `func contentsForType(typeName: String) throws -> AnyObject`：当数据被写入文件或文档中时，这个方法就会被`UIDocument`子类的实例调用。该方法负责收集被写入的数据，然后以`NSData`或`NSFileWrapper`对象返回。
 
 * `func loadFromContents(contents: AnyObject, ofType typeName: String?) throws`：当数据从文件或文档中被读取时，这个方法就会被`UIDocument`子类的实例调用。这个方法负责加载从文件中读取出的数据到应用的数据模型中。
 
 
####实现应用的数据结构

