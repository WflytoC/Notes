知识点：

* 静态库形式：`.a和.framework`

* 动态库形式：`.dylib和.framework`

* 系统的`.framework`是动态库，我们自己建立的`.framework`是静态库

####使用Realm要求：

 
```
 
 	* iOS 8 or later, OS X 10.9 or later & WatchKit
 	
 	* Xcode 7.3 or later required.

 
```

####安装

1. 将`RealmSwift.framework`和`Realm.framework`拷贝到`Embedded Binaries`中(选择`主工程`和`单元测试工程`)


2. 在应用工程的`Build Phases`下，创建`Run Script RealmSwift`，并将下面代码拷贝：

```
	
	bash "${BUILT_PRODUCTS_DIR}/${FRAMEWORKS_FOLDER_PATH}/Realm.framework/strip-frameworks.sh"
```