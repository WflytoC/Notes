给`AppComponent`添加两个属性,`title`属性表示应用名，`hero`属性表示英雄名。

`app.component.ts`

```
export class AppComponent {
  title = 'Tour of Heroes';
  hero = 'Windstorm';
}
```

现在更新`@Component`装饰器中的模板，将数据绑定到组件类的属性上。

```
template: '<h1>{{title}}</h1><h2>{{hero}} details!</h2>'
```

`{{ }}`告诉应用从组件类中读取`title`和`hero`属性，然后渲染。这是单向数据绑定中的插值。

####Hero object

这里，`hero`只是一个名字，下面来将`hero`从字符串转化为类。创建`Hero`类，将其放在`app.component.ts`文件的顶部，在`import`声明下面。

```
export class Hero {
    id: number;
    name: string;
}
```

重构一下组件的`hero`属性，让其成为`Hero`类型，然后初始化：

```
hero: Hero = {
        id: 1,
        name: 'Windstorm'
};
```

下面来改变模板中的数据绑定：

```
template: '<h1>{{ title }}</h1><h2>{{ hero.name }} details!</h2>'
```

添加更多的HTML元素：
```
template: '<h1>{{ title }}</h1><h2>{{ hero.name }} details!</h2><div><label>id:</label>{{hero.id}}</div><div><label>name: </label>{{hero.name}}</div>'
```

多行模板字符串：改变字符串模板的单引号，使用``符号。

```
template: `
        <h1>{{title}}</h1>
        <h2>{{hero.name}}<h2>
        <div><label>id: </label>{{hero.id}}</div>
        <div><label>name: </label>{{hero.name}}</div>
    `
```

我们想要能够编辑hero名字，使用<label>与<input>来重构：

```
template: `
        <h1>{{title}}</h1>
        <h2>{{hero.name}}<h2>
        <div><label>id: </label>{{hero.id}}</div>
        <div>
            <label>name: </label>
            <input value="{{hero.name}}" placeholder="name">
        </div>
    `
```

我们注意到hero的名字出现在<input>输入框中，但是，我们改变名字时，变化并没有映射到<h2>中。对<input>使用单向绑定并不会获得预期效果。

####双向绑定

对form inputs使用双向绑定之前，需要在angular模块中导入FormsModule包。我们需要将其添加到`NgModule`装饰器的`imports`数组中，这个数组包含了应用需要的外部模块。

`app.modules.ts`

```
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule }   from '@angular/forms';
import { AppComponent }  from './app.component';
@NgModule({
  imports: [
    BrowserModule,
    FormsModule
  ],
  declarations: [
    AppComponent
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

我们来更新下模板，使用`ngModel`内建指令来进行双向绑定：

```
<input [(ngModel)] ="hero.name" placeholder="name">
```

####Master/Detail

在`app.component.ts`创建包含十个hero的数组：

```
const HEROES: Hero[] = [
  { id: 11, name: 'Mr. Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
```

在AppComponent组件类中创建公有属性heroes，目的是为了绑定数据。

```
heroes = HEROES;
```

我们没有定义heroes类型，但是TypeScript可以从HEROES数组中推断出它的类型。

我们的组件拥有heroes，让我们在模板中创建一个无序列表来展示它们。在标题下面、hero详情上面插入：

```
<h2>My Heroes</h2>
<ul class="heroes">
  <li>
    <!-- each hero goes here -->
  </li>
</ul>
```

我们需要将组件类中的`heroes`数组绑定到模板上，首先为`<li>`标签添加内建指令`*ngFor`

```
<li *ngFor="let hero of heroes">
```

`"let hero of heroes"`意思是：从`heroes`数组中取出每个`hero`，将其存储在本地的`hero`变量中。


现在，我们在<li>标签之间插入一些内容，使用hero模板变量来展示heroes的属性。

```
<li *ngFor="let hero of heroes">
  <span class="badge">{{hero.id}}</span> {{hero.name}}
</li>

```


让我们为组件添加一些样式，通过对`@Component`装饰器设置`styles`属性。

```
styles: [`
  .selected {
    background-color: #CFD8DC !important;
    color: white;
  }
  .heroes {
    margin: 0 0 2em 0;
    list-style-type: none;
    padding: 0;
    width: 15em;
  }
  .heroes li {
    cursor: pointer;
    position: relative;
    left: 0;
    background-color: #EEE;
    margin: .5em;
    padding: .3em 0;
    height: 1.6em;
    border-radius: 4px;
  }
  .heroes li.selected:hover {
    background-color: #BBD8DC !important;
    color: white;
  }
  .heroes li:hover {
    color: #607D8B;
    background-color: #DDD;
    left: .1em;
  }
  .heroes .text {
    position: relative;
    top: -3px;
  }
  .heroes .badge {
    display: inline-block;
    font-size: small;
    color: white;
    padding: 0.8em 0.7em 0 0.7em;
    background-color: #607D8B;
    line-height: 1em;
    position: relative;
    left: -1px;
    top: -4px;
    height: 1.8em;
    margin-right: .8em;
    border-radius: 4px 0 0 4px;
  }
`]
```

展示heroes的模板整体：

```
<h2>My Heroes</h2>
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
```

####选择一个Hero

修改`<li>`：将angular事件绑定到它的点击事件上。

```
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
        <span class="badge">{{hero.id}}</span> {{hero.name}}
</li>
```

关注一下事件绑定：

```
(click)="onSelect(hero)"
```

`()`表示`<li>`元素的`click`事件，`=`右边的表达式调用`AppComponent`的方法`onSelect()`，该方法将模板输入变量hero作为参数。

向组件中添加`onSelect`方法，该方法将组件的`selectedHero`设置为用户点击的hero。

使用selectedHero属性来取代之前的hero属性：

```
selectedHero: Hero;

```

添加`onSelect`方法：

```
onSelect(hero: Hero): void {
  this.selectedHero = hero;
}
```

完成上面的步骤后，当加载应用时，我们会看到一系列hero，但是没有一个hero被选中，`selectedHero`是`undefined`。浏览器控制台会报下面的错误：

```
EXCEPTION: TypeError: Cannot read property 'name' of undefined in [null]
```

解决这个问题的方法是：如果没有hero被选中的话，我们让hero详情移除DOM中。使用<div>来包装模板中的hero详情内容，然后添加ngIf内建指令，将它的值设置为组件类的selectedHero属性。

```
<div *ngIf="selectedHero">
  <h2>{{selectedHero.name}} details!</h2>
  <div><label>id: </label>{{selectedHero.id}}</div>
  <div>
    <label>name: </label>
    <input [(ngModel)]="selectedHero.name" placeholder="name"/>
  </div>
</div>
```

angular中属性绑定(`property binding`)的语法是`[]`,例如：

```
[class.selected]="hero === selectedHero"
```

####多个组件

现在，我们的hero列表和hero详情在同一文件的相同组件中，下面来分离hero详情组件，添加新的文件`hero-detail.component.ts`，创建组件`HeroDetailComponent`。

```
import { Component, Input } from '@angular/core';

@Component({
  selector: 'my-hero-detail',
})
export class HeroDetailComponent {
}
```

上述代码从angular中导入`Component`和`Input`装饰器。接着将该组件导入到AppComponent中，创建对应的`<my-hero-detail>`元素。

`HeroDetailComponent`必须要知道展示哪个hero，谁会告诉它，就是父组件，AppComponent.

更新AppComponent模板，这样，它就会将`selectedHero`属性绑定到`HeroDetailComponent`的hero属性上。

```
<my-hero-detail [hero]="selectedHero"></my-hero-detail>
```

注意：hero属性是属性绑定的target，我们将target属性声明为input属性。使用@Input来标注hero属性。

```
@Input()
  hero: Hero;
```

来到AppModule中，应用的根模块，让它来使用HeroDetailComponent，首先导入：

```
import { HeroDetailComponent } from './hero-detail.component';
```

接着，我们将`HeroDetailCOmponent`添加到`NgModule`装饰器的`declarations`数组中，这个数组包含了所有的`component`，`pipe`，我们创建的`directive`。

```
@NgModule({
  imports: [
    BrowserModule,
    FormsModule
  ],
  declarations: [
    AppComponent,
    HeroDetailComponent
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

####Service

创建`hero.service.ts`

```
import { Injectable } from '@angular/core';
@Injectable()
export class HeroService {

}
```

导入angular的`Injectable()`函数，然后将其用作`@Injectable`装饰器。添加`getHeroes`方法：

```
@Injectable() 
export class HeroService {
	getHeroes(): void {
	
	}
}
```

创建新的文件`mock-heroes.ts`，内容如下：

```
import {Hero} from './hero';

export const HEROES: Hero[] = [
  { id: 11, name: 'Mr. Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
```

上述代码中，导出HEROES常量，以便在其它地方导入它。

回到HeroService，导入数据HEROES

```
import { Injectable } from '@angular/core';
import { Hero } from './hero';
import { HEROES } from './mock-heroes';
@Injectable()
export class HeroService {

    getHeroes(): Hero[] {
        return HEROES;
    }
}
```

组件该如何获取一个运行时的`HeroService`实例呢？方法是注入HeroService。

* 添加构造器(同时会定义一个私有的属性)

		    constructor(private heroServie: HeroService) {
    	}
* 将`HeroService`添加到组件的`providers`元数据中

		在@Component调用中添加下面的providers数组属性。
		providers: [HeroService]


不要将复杂的逻辑代码放在constructor构造器中，如果我们实现angular的ngOnInit生命周期接口，angular会调用。angular提供了许多接口来处理组件生命周期的关键时刻。一创建时刻、发生变化时刻、最终的销毁时刻。每个接口都有一个方法，组件实现方法时，angular就会在适当的时候调用它。

```
import { OnInit } from '@angular/core';
export class AppComponent implements OnInit { 
	ngOnInit():void {
        
    }
}
```

HeroService会马上返回heroes数据，它的getHeroes是同步的，为了不让它阻塞UI线程，我们必须要使用异步的技术。我们会使用`Promise`。

我们要求异步的服务来执行某些工作，会给它一个回调函数。下面使用返回Promise的getHeroes方法来更新HeroService。

```
getHeroes(): Promise<Hero[]> {
        return Promise.resolve(HEROES);
 }
```

改变HeroService后，现在我们需要将this.heroes设置为Promise，而不是hero数组。

```
ngOnInit():void {
        this.heroServie.getHeroes().then(heroes => this.heroes = heroes);
    }
```

####添加路由


angular路由是外部的、可选的模块，叫做`RouterModule`，路由结合自多个服务(`RouterModule`)，多个指令(`RouterOutlet` `RouterLink` `RouterLinkActive`),和一个配置(`Routes`)。

* 添加基本标签

		打开index.html，在<head>部分的顶部添加<base href="/">
		
		<head>
  			<base href="/">
* 配置路由

		
		为应用的路由创建配置文件，Routes告诉路由展示哪个视图(当用户点击链接或在浏览器地址栏粘贴URL)，我们来定义第一个路由，其作为heroes组件的路由。
		
		import { ModuleWithProviders }  from '@angular/core';
		import { Routes, RouterModule } from '@angular/router';

		import { HeroesComponent }      from './heroes.component';

		const appRoutes: Routes = [
  		{
    		path: 'heroes',
   	 		component: HeroesComponent
  		}
		];
		
		
		上面的路由包含下面部分：
		
		path：搭配浏览器地址栏URL的路由的路径；
		component：当导航到这个路由时，路由应该创建的组件
		
		最后导出使用`RouterModule.forRoot`方法(参数是路由数组)初始化的常量`routing`。该方法返回配置好的路由模块，稍后我们将其添加到根模块中，AppModule。
		
		将创建好的路由添加到根NgModule中：
		
		import { routing } from './app.routing';

		@NgModule({
    		imports: [ BrowserModule,FormsModule,routing ],

		})
* `Router Outlet`

		如果粘贴路径`/heroes`到浏览器地址栏中，路由会将其搭配到heroes路由上，展示`HeroesComponent`组件。我们必须在模板的底部添加<router-outlet>元素来达到这样的效果。RouterOutlet是RouterModule提供的一个指令，当我们导航时，路由会马上在<router-router>下面展示每个组件。
* `Router Links`

		我们添加一个锚点标签到模板中，用户点击时，会导航到HeroesComponent。
		
		template: `
        <h1>{{title}}</h1>
        <a routerLink="/heroes">Heroes</a>
        <router-outlet></router-outlet>
        `
* 添加Dashboard

		只有存在多个视图时，路由才会有意义。创建组件`DashboardComponent`：
		
		来到app.routing.ts，指导它导航到dashboard。导入dashboard组件，将下面的路由定义添加到Routes数组中。
		
		{
        path: 'dashboard',
        component: DashboardComponent
    	}
    	
    	导入并将DashboardComponent添加到根NgModule的declarations中。
    	
    	declarations: [AppComponent, DashboardComponent,HeroDetailComponent,HeroesComponent]
    	
    	我们希望应用一启动时就显示dashboard，可以使用重定向来实现这样的效果，将下面的代码添加到路由定义的数组中。
    	
    	{
        path: '',
        redirectTo: '/dashboard',
        pathMatch: 'full'
    	}
    	
    	最后，将dashboard导航链接添加到模板中
    	
    	template: `
        <h1>{{title}}</h1>
        <nav>
            <a routerLink="/dashboard">Dashboard</a>
            <a routerLink="/heroes">Heroes</a>
        </nav>
        <router-outlet></router-outlet>
        `
* Dashboard Top Heroes

		使用templateUrl属性来取代template元数据，该属性指向一个新的模板文件。




##HTTP

####提供HTTP服务

`HTTPModule`不是核心的Angular模块，它是angular获取网络的可选途径。它作为分开的额外模块`@angular/http`存在，放在angular的npm包中。

注册HTPP服务：我们的应用依赖于angular的`http`服务，来自`@angular/http`库的`HTTPModule`为完整的HTTP服务保存了`provider`。

我们应当要在应用的任何地方都可以获取这些服务，所以要注册这些服务：将`HttpModule`添加到`AppModule`的`imports`列表中。在`AppModule`中，我们渲染了应用和它的`AppComponent`。

####模拟web API

我们推荐在根`AppModule`的`providers`中注册全局的服务，我们将要让HTTP客户端从服务中获取与保存数据，该服务是`in-memory web API`。







































