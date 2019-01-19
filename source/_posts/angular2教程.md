---
title: angular2教程
date: 2018-03-13 22:22:40
tags:
---

# Angular简介
- AngularJS：
	+ AngularJS自2012年发布，首次提出双向绑定。
	+ AngularJS 是一个为动态WEB应用设计的结构框架。它能让你使用HTML作为模板语言，通过扩展HTML的语法，让你能更清楚、简洁地构建你的应用组件。它的创新点在于，利用 数据绑定 和 依赖注入，它使你不用再写大量的代码了。这些全都是通过浏览器端的Javascript实现，这也使得它能够完美地和任何服务器端技术结合。
	+ AngularJS是为了克服HTML在构建应用上的不足而设计的。HTML是一门很好的为静态文本展示设计的声明式语言，但要构建WEB应用的话它就显得乏力了。所以我做了一些工作（你也可以觉得是小花招）来让浏览器做我想要的事。
	+ 不足之处：
		* 1、饱受诟病的性能问题：通过检查进行数据更新，当数据不断增加时，检查的效率就不断降低。页面加载速度也会变慢。
		* 2、落后于当前web发展理念(如组件式的开发)
		* 3、专注于web开发，对手机端的支持不是太友好：由于angularJS是09年诞生的，因此并没有考虑到手机端的适配，首先是性能问题，手机平台的硬件资远远比不上电脑平台。（IONIC专门做app的开发框架）
- Angular2：
	+ 2016年angular2发布
	+ 目标：原生移动支持：IOS+Android
	+ 新特性：
		* 1、移除了 controller +$scope的设计，改用组件式开发。（更容易理解和上手）
		* 2、性能更好（渲染更快，变化检测效率更高）
			- AngularJS可以很明显的看到应用启动时的页面加载过程。
			- Angular2优化：
				+ 启动开始，绑定所有组件
				+ 渲染还未开始
				+ 一个页面在服务器被渲染后，然后发送到客户端
				+ 这时Angular对其解析，然后把解析后的页面注入到DOM中，这样就避免出现闪烁。
		* 3、优先为移动应用设计（Angular Mobile Toolkit ）
		* 4、更加贴合未来的标准（如es6/7、WebComponent）
		* 5. 依赖注入改进：
			- AngularJS 1.x多重依赖注入
			- Angular2改进：
				+ 只有一种依赖注入机制：在构造函数中通过类型注入。
				+ constructor(keyUtils: KeyboardUtils){
				+ this.keyUtils = keyUtils;}
				+ 只有一种机制，那么它将变得更加容易学习。同时这种依赖注入器是类似层级结构，在不同层次的组件树，有可能实现对相同类型的不同实现。
				+ 如果一个组件没有定义依赖，它会代理给上层注入器查找依赖，依次往上。这让 Angular 2 提供原生的懒加载成为可能
	+ 无缝升级方案：
		* UpgradeAdapter适配器：由于angularJS并没有上下兼用angular2代码，所以推出了Adapter适配器，用于将angular2的新特性加载到angularJS的模板中。这样的一种实现方式既没有对原有的代码进行破坏性的影响，通话也能及时使用angular2的新特性。
	+ 采用单向数据流

# angular-cli安装配置
- 安装ts:(因为angular是typescript写的，所以要先安装)  npm install -g typescript typings
- 老版本：npm install -g angular-cli
- 新版本：npm install -g @angular/cli
	+ 最好用cnpm装，因为npm需要支持很多依赖，如python、vs等
	+ npm install -g cnpm
	+ cnpm install -g angular-cli
- 安装完后：
	+ npm config list查看具体的环境配置、目录
	+ 简单测试：
		* ng --version
		* ng help
		* ng new project1
		* ng serve
- 新建工程
	+ webstorm打开后，新建angular-cli，这个是代表angular2工程，angular-js代表angular工程
	+ 配置angular-cli的安装路径
	+ 常见问题：http://blog.csdn.net/pointer_v/article/details/55096197

# Angular的三大核心概念
1. 组件
	- 
2. 模块NgModule
	- 动态异步加载js
	- Router和NgModule实现异步路由
3. 路由
	- 静态路由
	- 异步路由

# Angular架构
- 组件化
- 依赖注入
- 数据绑定

# UI库
- ng2-bootstrap
- PrimeNG
- Angular-Material2
- ionic

# NiceFish
- 简介：
- 

# Angular引入css
1. 全局引入
2. bootstrap.css和ng-bootstrap.css区别：
	- 
3. 

# 常见问题
1. AngularJS设置img：ng-src与src区别
	- <img ng-src="{{url}}">
	- 加载图片过程：在angular加载页面完成之前，浏览器会试图去url地址加载图片；一旦加载完成之后，浏览器会找到具体的位置值，如logo.png,然后会加载正确的图片；
	- src：Angular的数据还没有返回，渲染dom时出错，所以直接读到的是空的url值
	- ng-src会让angular加载完成之后，才会得到正确的地址数据。
2. 相对路径问题：
	- 加/img/...
	- 而不是img/...
3. 

# Angular指令
- angular指令：扩展的HTML属性，带有ng-前缀
- angular完美支持数据库的CRUD(create、read、update、delete)应用程序，可以把实例对象想象成数据库中的记录
- ng-app:定义了angularJS应用程序的根元素
	+ 在网页加载完毕后会自动引导
- ng-init：定义初始值
- ng-model：绑定HTML元素到应用程序数据
	+ 提供类型验证number、email、required
	+ 提供状态invaid、dirty、touched、error
	+ 为html元素提供css类
	+ 绑定html元素到html表单
- ng-repeat：对于集合中的每个项会克隆一次html元素
- 创建自定义指令：
	+ 使用驼峰法来命名一个指令， runoobDirective, 但在使用它时需要以 - 分割, runoob-directive:
- 

# Typescript
1. Typescript简介
2. Javascript区别
	- JavaScript一种动态类型、弱类型、基于原型的客户端脚本语言，用来给HTML网页增加动态功能。
		+ 动态：在运行时确定数据类型。变量使用之前不需要类型声明，通常变量的类型是被赋值的那个值的类型。
		+ 弱类：计算时可以不同类型之间对使用者透明地隐式转换，即使类型不正确，也能通过隐式转换来得到正确的类型。
		+ 原型：新对象继承对象（作为模版），将自身的属性共享给新对象，模版对象称为原型。这样新对象实例化后不但可以享有自己创建时和运行时定义的属性，而且可以享有原型对象的属性。
		+ PS：新对象指函数，模版对象是实例对象，实例对象是不能继承原型的，函数才可以的。
3. TypeScript/Angular2调用原生JS的桥接文件的编写：https://www.jianshu.com/p/e87e596c3280

# 标准
1. JavaScript三个部分：
	- ECMAScript核心：规定了语言的组成部分：
		+ 语法、类型、语句、关键字、保留字、操作符、对象
	- DOM文档对象类型：
		+ 把整个页面映射为一个多层节点效果，开发人员可以借助DOM提供的API，轻松地删除、添加、替换或修改任何节点
		+ DOM也有级别，分为DOM1、DOM2、DOM3，扩展不少规范和新接口
	- BOM浏览器对象模型：
		+ 支持可以访问和操作浏览器窗口的浏览器对象模型，开发人员可以控制浏览器显示的页面以外的部分；
2. ES5
	- ECMAScript第五个版本(第四个版本由于过于复杂废弃了)
	- 支持特性：
		+ strict模式
		+ Array增加方法：every、some 、forEach、filter 、indexOf、lastIndexOf、isArray、map、reduce、reduceRight方法
		+ Object方法：Object.getPrototypeOf、Object.create、Object.getOwnPropertyNames、Object.defineProperty
3. ES6
	- ECMAScript6在保证向下兼容的前提下，提供大量新特性，目前浏览器兼容情况如下：
	- 特性：
		+ 块级作用域：关键字let、常量const
		+ 对象字面量的属性赋值简写property value shorthand
		+ 赋值解构：
				```
				let singer = { first: "Bob", last: "Dylan" };
				let { first: f, last: l } = singer; // 相当于 f = "Bob", l = "Dylan"
				let [all, year, month, day] =  /^(\d\d\d\d)-(\d\d)-(\d\d)$/.exec("2015-10-25");
				let [x, y] = [1, 2, 3]; // x = 1, y = 2
				```
		+ 函数参数 - 默认值、参数打包、 数组展开（Default 、Rest 、Spread）
		+ 箭头函数 Arrow functions
		+ 字符串模板 Template strings
		+ Iterators（迭代器）+ for..of
		+ Proxies:使用代理proxy监听对象的操作，然后做相应的事情
		+ Promises
	- 
