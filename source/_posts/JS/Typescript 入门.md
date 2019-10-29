title: TypeScript Get Started
date: 2019/10/29
tags: 
	- React Native
	- TypeScript
---

最近刚开始入门typescript ，这是我总结的一些typescript 入门基础知识，内面知识涵盖了一些基本的概念，这里先发出去，毕竟坚持写博客的第一步就是先发出去，然后再慢慢优化，有第一步才会有接下来的第二三四步。

![mac and money](https://raw.githubusercontent.com/WymanY/PicBed/master/img/mac_money.jpg)
<!-- more -->


#### 什么是TypeScript
TypeScript 是 JavaScript 的一个超集，主要提供了类型系统和对 ES6 的支持，它由 Microsoft 开发，代码开源于 [GitHub](https://github.com/microsoft/TypeScript) 上。
引用官网的定义为：
> TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. Any browser. Any host. Any OS. Open source.

翻译成中文如下：
> TypeScript 是 JavaScript 的类型的超集，它可以编译成纯 JavaScript。编译出来的 JavaScript 可以运行在任何浏览器上。TypeScript 编译工具可以运行在任何服务器和任何系统上。TypeScript 是开源的。

#### Typescript入门教程
[Typescript](https://www.tslang.cn/samples/index.html)的官方文档地址，这里充分描述了如何入门typescript,包括可以在playground中练习typescript的语法,目前typescript已经开发到3.7Beta版本了，这个网站显示是3.6.1，所以还是推荐在[TS官方github](https://github.com/microsoft/TypeScript)查看具体相关的信息。当然入门教程我还是推荐**xcatliu**的[TypeScript 入门教程](https://github.com/xcatliu/typescript-tutorial),因为官方手册讲解比较晦涩难懂(~~PS:有时可能是因为中文网站翻译的问题~~），还经常在前面的章节就会涉及到后面的内容。而**TypeScript 入门教程**于从 JavaScript 程序员的角度总结思考，循序渐进的理解 TypeScript，但是讲解的都比较基础，如果想要深入，可以在入门之后再仔细阅读官方文档。

#### TypeScript的优势
**TypeScript 增加了代码的可读性和可维护性**
* 类型系统实际上是最好的文档，大部分的函数看看类型的定义就可以知道如何使用了
* 可以在编译阶段就发现大部分错误，这总比在运行时候出错好
* 增强了编辑器和 IDE 的功能，包括代码补全、接口提示、跳转到定义、重构等

#### TypeScript 的缺点
* 有一定的学习成本，需要理解接口（Interfaces）、泛型（Generics）、类（Classes）、枚举类型（Enums）等前端工程师可能不是很熟悉的概念
* 短期可能会增加一些开发成本，毕竟要多写一些类型的定义，不过对于一个需要长期维护的项目，TypeScript 能够减少其维护成本
* 集成到构建流程需要一些工作量
* 可能和一些库结合的不是很完美

#### TypeScript 一些基本的语法概念

* 为JS对象创建的基本类型
布尔值、数值、字符串、null、undefined 以及 ES6 中的新类型 Symbol。
所以我们在声明变量的时候可以通过如下的方式声明：

```js
let isDone: boolean = false;//bool
let decLiteral: number = 6;//number
let myName: string = 'Tom';//string
let u: undefined = undefined;//undefined
let n: null = null;//null
//javaScript中没有空值的概念，void 在ts 中表示没有任何返回值的函数
function alertName(): void {
    alert('My name is Tom');
}

let fibonacci: number[] = [1, 1, 2, 3, 5];//数组一般为基本类型后加上[]，表示一组同类行的数据。

```

其中类型推论就是我们可以不用明确的制定变量的类型，编译器会根据右边的值自动推论变量的类型。这在swift 中也存在类似的语法。

* 元组
数组合并了相同类型的对象，而元组（Tuple）合并了不同类型的对象。

```js
let tom: [string, number] = ['Tom', 25];
let tom: [string, number];
tom[0] = 'Tom';
tom[1] = 25;
```

* 枚举
枚举（Enum）类型用于取值被限定在一定范围内的场景，比如一周只能有七天，颜色限定为红绿蓝等。

```js
enum Days {Sun, Mon, Tue, Wed, Thu, Fri, Sat};
enum Days {Sun = 7, Mon, Tue, Wed, Thu, Fri, Sat = <any>"S"};
```

* 泛型
泛型（Generics）是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

```js
function swap<T, U>(tuple: [T, U]): [U, T] {
    return [tuple[1], tuple[0]];
}
```
在函数内部使用泛型变量的时候，由于事先不知道它是哪种类型，所以不能随意的操作它的属性或方法，可以约束是某一种类型的泛型，这样我们就可以在使用泛型相应的方法。

```js
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg: T): T {
    console.log(arg.length);
    return arg;
}
```

* 类
typeScript 与ES6的最主要区别就是为类增加了访问控制。TypeScript 可以使用三种访问修饰符（Access Modifiers），分别是 public、private 和 protected。并且添加了只读属性关键字readonly

```js
class Animal {
    readonly name;
    public constructor(name) {
        this.name = name;
    }
}

let a = new Animal('Jack');
console.log(a.name); // Jack
a.name = 'Tom';
```
* 接口类型

TypeScript 中的接口是一个非常灵活的概念，除了可用于对类的一部分行为进行抽象以外，也常用于对「对象的形状（Shape）」进行描述。

```js
interface Person {
    name: string;
    age: number;
2// age?:number;
}

let tom: Person = {
    name: 'Tom',
    age: 25
};
定义的变量比接口少了一些属性是不允许的,多一些属性也是不允许的，
当然如果我们想要少一些属性在变量是option的时候是可以的。
```

接口也类似协议，可以由类来遵守实现，从而对类进行约

```js
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}

class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}

```
* 字符串字面量类型
字符串字面量类型用来约束取值只能是某几个字符串中的一个

```js
type EventNames = 'click' | 'scroll' | 'mousemove';
function handleEvent(ele: Element, event: EventNames) {
    // do something
}

handleEvent(document.getElementById('hello'), 'scroll');  // 没问题
handleEvent(document.getElementById('world'), 'dbclick'); // 报错，event 不能为 'dbclick'
```

* 声明合并
如果定义了两个相同名字的函数、接口或类，那么它们会合并成一个类型
如下是接口的合并，合并还有命名空间的合并和类的合并，和命名空间与类和函数和枚举类型合并。我的理解是合并有点类似OC中的分类，或者类扩展等，可以给已有的类增加属性方法。
```js
interface Alarm {
    price: number;
}
interface Alarm {
    weight: number;
}
```


* 联合类型

```javascript

    let myFavoriteNumber: string | number;
    myFavoriteNumber = 'seven';
    myFavoriteNumber = 7;
    
    //注意我们在使用联合类型变量的时候如果想访问联合变量的属性，只能访问两种类型共同拥有的属性。
    myFavoriteNumber.length//Error
    myFavoriteNumber.toString//OK
    
```

* 交叉类型
交叉类型是将多个类型合并为一个类型。 这让我们可以把现有的多种类型叠加到一起成为一种类型，它包含了所需的所有类型的特性。 例如， Person & Serializable & Loggable同时是 Person 和 Serializable 和 Loggable。 就是说这个类型的对象同时拥有了这三种类型的成员。
们大多是在混入（mixins）或其它不适合典型面向对象模型的地方看到交叉类型的使用。 （在JavaScript里发生这种情况的场合很多！） 下面是如何创建混入的一个简单例子

```js
function extend<T, U>(first: T, second: U): T & U {
    let result = <T & U>{};
    for (let 341224198907270533 in first) {
        (<any>result)[341224198907270533] = (<any>first)[341224198907270533];
    }
    for (let 341224198907270533 in second) {
        if (!result.hasOwnProperty(341224198907270533)) {
            (<any>result)[341224198907270533] = (<any>second)[341224198907270533];
        }
    }
    return result;
}

class Person {
    constructor(public name: string) { }
}
interface Loggable {
    log(): void;
}
class ConsoleLogger implements Loggable {
    log() {
        // ...
    }
}
var jim = extend(new Person("Jim"), new ConsoleLogger());
var n = jim.name;
jim.log();
```


#### Typescript 开发的一些知名项目
**IDE - VSCode**
基于TypeScript + Nodejs + Electron开发的IDE. Github上star: 2万+
**Framework - Angular2**
基于TypeScript + RxJS + ZoneJS的Framework. Github上star: 2万+Angular从Angular2开始采用TypeScript来开发，强类型对框架的稳定性提供不少支持。微软Azure的页面就是用Angular写的

**UI - ant-design**
基于TypeScript + React的UI界面库. Github上star: 1万+
ant-design是由国内阿里旗下的蚂蚁金服的团队用TypeScript开发的一款企业级React UI库，已经应用到金服和其他阿里旗下产品当中。

**library - ui-router**
基于TypeScript + Angular的UI router库. Github上star: 1万+

ui-router的目的是提供一个管理UI跳转的库，基于状态机维护了一个层级的状态树，这个库对于单页应用来说非常有用。

**library - RxJS**
这个库现在出到5代，之前是用JavaScript开发，5代开始采用TypeScript开发。 Github上star: 5千+,前身4是由JavaScript开发，Github已经有超过1万的star。RxJS是基于流的概念，提供了一系列神奇的函数工具集，使用它们可以合并、创建、过滤这些流

**tool - tslint**
做JavaScript开发的有ESLint来规范代码，而TypeScript则可以用TSLint。 Github上star: 1千+，它能够规范我们的代码风格，提高代码质量和可维护性。

awesome-typeScript-projects这个项目里包含了绝大多数用typeScript开发的项目，包含应用，UI FrameWork,学习资源，推荐的editor，插件等等。地址如下：https://github.com/brookshi/awesome-typescript-projects

<hr>
<!-- more -->
To do:
* [ ] 如何为一个第三方的javascript库编写lib.d.ts
* [ ] 如何将我们的项目转化为支持typescript
