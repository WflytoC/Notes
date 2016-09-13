`Gulp.js` 是一个自动化构建工具,开发者可以使用它在项目开发过程中自动执行常见任务。

####@angular模块

* `common`
* `compiler`
* `compiler-cli`
* `core`
* `forms`
* `http`
* `platform-browser`
* `platform-browser-dynamic`
* `router`
* `tsc-wrapped`
* `upgrade`


####组件

```
import {  Component } from '@angular/core';

@Component( {
    selector: 'app',
    template: '<p>Hello,World</p>'
})

export class AppComponent {
    constructor() {}
}
```

`AppComponent`是应用的入口点，当然，它是组件。组件是由`@Component`修饰的类，通过可视化模版来控制`DOM`元素。只要遇到匹配的选择器元素(`selector element`)，angular2就会将组件模版放到`DOM`中。

####模板语法

* `Interpolation` -- `{{}}`

		实现单向数据绑定(数据从数据源流向模板)，`{{}}`里的内容叫模板表达式。不要在模板表达式中编写改变组件的东西。

* `Square bracket syntax` -- `[]`

		绑定HTML元素的属性
* `Event Binding` -- `( )`

		绑定事件，当绑定HTML元素事件属性时，需要省略掉`on`
* `Two-way Data Binding` -- `[[ngModel]]`

		该语法只适用以下的HTML原生元素：input、textbox、select，你也可以监听模型的变化，通过绑定(ngModelChange)事件
		
####Built-in Directives内建指令

指令名前要加`*`

* `ngIf`：

		<ul *ngIf="isVisible"></ul>
		//如果表达式，这里是isVisible，的值为false，该元素不是隐藏，而是移除。
* `ngFor`

		基本语法： *ngFor="let user of users"
		*ngFor提供了一些到处值：
		index -- 当前循环的索引；
		first -- 元素是否是第一个；
		last -- 元素是否是最后一个；
		even -- 元素索引是否是偶数；odd -- 元素索引是否是奇数
* `ngSwitch`

		<div class="wrapper" [ngSwitch] = "user.favoriteAnimal">
			<div *ngSwitchWhen="dog">
			
			</div>
			<div *ngSwitchWhen="cat">
			
			</div>
			<div *ngSwitchDefault>
			
			</div>
		</div>
		
		满足表达式的HTML块会插入到DOM中。例如，如果user.favoriteAnimal === "dog"，那么第一个块就会插入到DOM中。
		
		
* `ngClass`和`ngStyle`


		可以使用[class.active]="isActive()"和[style.width.px]="widthToSet()"来为元素添加单个class或样式。但是，需要动态添加、移除、改变多个class或样式时，这种语法就会很乱。angular2提供了ngClass和ngStyle。
		
		<span [ngClass]="{online: user.status === 'online',offline: user.status == 'offline'}"></span>
		
		span [ngStyle]="{'background': 'red','color': 'black'}"></span>

####Services

`Servie`负责处理数据获取和服务器通信，而组件负责展示数据。

创建`user.service.ts`:

		import { Injectable } from '@angular/core';
		@Injectable()
		export class UserService {
		
			private users = [
			
			];
			
			get() {
				
				return this.users;
			}
		
		}

创建好服务后，如何在组件中使用呢？
		
		//导入UserService类
		import { UserService } from './user.Service';
		
		//为UserService注册provider
		providers: [UserService]
		
		//定义service属性
		constructor(private _userService: UserService) {}

####Basic Routing

首先进行配置：`在index.html`中

		<html>
			<head>
				<base href="/">

在main.ts中注册`ROUTER_PROVIDERS`:

		import {ROUTER_PROVIDERS} from '@angular/router';
		bootstrap(AppComponent,[ROUTER_PROVIDER]);


