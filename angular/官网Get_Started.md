前提：安装`Node.js`和`npm`

####创建和配置工程

* 创建工程文件夹

* 创建配置文件

		package.json：声明工程的npm包依赖项
		
		tsconfig.json：定义TypeScript如何编译器产生JavaScript
		
		typings.json：为TypeScript编译器不识别的库提供额外的定义文件
		
		systemjs.config.js：为模块加载器提供发现应用模块位置的信息
		
* 安装包

		npm install
		

####创建应用

首先在工程目录中创建文件夹`app`，应用的代码基本放置在里面。

使用`NSModules`来将功能块构成angular应用。每个angular应用至少有一个模块，root模块。

`app/app.module.ts`，这是应用的入口点

```
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

import { AppComponent }   from './app.component';

@NgModule({
    imports: [ BrowserModule ],
    declarations: [AppComponent],
    bootstrap: [ AppComponent ]
})

export class AppModule { }
```

####创建组件，将其添加到应用中

每个angular应用至少有一个组件：根组件，叫做`AppComponent`。组件是angular应用的基本构建快。一个组件通过它关联的模板来控制一个视图。

`app/app.component.ts`

```
import { Component } from '@angular/core';
@Component({
  selector: 'my-app',
  template: '<h1>My First Angular 2 App</h1>'
})
export class AppComponent { }
```

组件的结构：

* `import`声明：让你定义的组件能够获得angular核心的`@Compnent`装饰器函数。
* `@Compnent`装饰器将元数据与`AppComponent`组件类关联
* 组件类通过模板来控制视图的外观与行为。在该例子中，不需要任何应用逻辑，所以组件类是空的。

####开启应用

需要告诉angular来开启应用

`app/main.ts`

```
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app.module';
const platform = platformBrowserDynamic();
platform.bootstrapModule(AppModule);

```

上面的代码初始化了应用运行的平台，然后使用平台来加载`AppModule`

####定义网页来加载应用

在工程根目录，创建`index.html`：

```
<html>
  <head>
    <title>Angular 2 QuickStart</title>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="styles.css">
    <!-- 1. Load libraries -->
     <!-- Polyfill(s) for older browsers -->
    <script src="node_modules/core-js/client/shim.min.js"></script>
    <script src="node_modules/zone.js/dist/zone.js"></script>
    <script src="node_modules/reflect-metadata/Reflect.js"></script>
    <script src="node_modules/systemjs/dist/system.src.js"></script>
    <!-- 2. Configure SystemJS -->
    <script src="systemjs.config.js"></script>
    <script>
      System.import('app').catch(function(err){ console.error(err); });
    </script>
  </head>
  <!-- 3. Display the application -->
  <body>
    <my-app>Loading...</my-app>
  </body>
</html>

```

在工程根目录创建`styles.css`

```
/* Master Styles */
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2, h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
```

####构建和运行应用

打开终端，运行`npm start`，该命令运行两个并行node进程：

* 处于观察模式的TypeScrpt编译器
* 静态服务器`lite-server`，在浏览器中加载`index.html`，当应用文件改变时，就会刷新浏览器。












