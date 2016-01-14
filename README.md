# learning egret

## Index
1.[js如何调用ts?](#1)
2.[ts如何调用js?](#2)
3.[egret基础](#egret)
4.[Q&A](#qa)
5.[typescript基础](#ts)

-------------------------

<h2 id="1">js call ts</h2>

方法与ts调用ts一样。例如有ts方法：
```typescript
class SuperMan {
    public constructor() {
    }

    public say() {
        alert("hello, I'm Superman'");
    }
}

module Human {
    export class man {
        public constructor() {

        }

        public say() {
            alert("I'm a man'");
        }

        public static saying() {
            alert("I'm a man");
        }
    }
}
```
那么，js调用的方式如下：
```javascript
var superman = new SuperMan();
superman.say();

var man = new Human.man();
man.sya();

//调用静态方法
Human.man.saying();

```

--------------------------

<h2 id="2">ts call js</h2>

这里所说的特指js库文件（如：SFS2X_API_JS.js），由于 js 与 ts 在语法结构上的差异，在 ts 中不能直接调用 js 库的 API，所以TypeScript 团队提供了一套虚构声明语法，可以把现有代码的 API 用头文件的形式描述出来，扩展名为 d.ts（d.ts 命名会提醒编译器这种文件不需要编译）。更多关于[d.ts](http://www.typescriptlang.org/Handbook#writing-dts-files)

在egret环境下，想要调用js库，官方建议以第三方库的形式导入到egret项目中。下面以一个实例来说明。

1.假设有js库名为hello.js
```javascript
function say(msg) {
    alert(msg);
}
```
2. 创建hello.d.ts
```typescript
declare function say(msg: string): void;
```
3. 创建egret-lib项目,执行`egret create_lib hello`,进入hello/，可以看到下面文件：

- bin   存放编译后的文件
- libs  引用的其他类库
- src   源码
- package.json   配置文件

4.将刚才创建的 hello.js 和 hello.d.ts 复制到 hello/src/
5.编辑package.json，将源文件包含进去
```json
{
    "name": "egret",
    "version": "3.0.0",
    "modules": [
        {
            "name": "hello",
            "description": "hello",
            "files": [
                "hello.js",
                "hello.d.ts"
            ],
            "root": "src"
        }
    ]
}
```
6.编译，发布,`egret build`

编译成功后，在hello/bin/下面生成一个名为`hello/`的目录，这就是我们要的第三方库

7.测试

- 创建一个egret空项目`egret create demo --type empty`
- 编辑`egretProperties.json`导入hello库
    ```json
     "modules": [
        {
          "name": "egret"
        },
        {
          "name":"hello",
          "path":"../hello"
        }
      ]
    ```
- 执行`egret build -e`后会发现项目下libs/modules/多了个hello/的模块
- 添加测试代码
```typescript
public constructor() {
    super();
    say("hello world");
}
```
-------------------------------------

<h2 id="egret">egret基础</h2>

#### 屏幕适配
- EXACT_FIT
    不保持原始宽高比缩放应用程序内容，缩放后应用程序内容正好填满播放器视口

- FIXED_HEIGHT
    保持原始宽高比缩放应用程序内容，缩放后应用程序内容在水平和垂直方向都填满播放器视口，但只保持应用程序内容的原始高度不变，宽度可能会改变

- FIXED_WIDTH
    保持原始宽高比缩放应用程序内容，缩放后应用程序内容在水平和垂直方向都填满播放器视口，但只保持应用程序内容的原始宽度不变，高度可能会改变

- NO_BORDER
    保持原始宽高比缩放应用程序内容，缩放后应用程序内容的较窄方向填满播放器视口，另一个方向的两侧可能会超出播放器视口而被裁切

- NO_SCALE
    不缩放应用程序内容

- SHOW_ALL
    保持原始宽高比缩放应用程序内容，缩放后应用程序内容的较宽方向填满播放器视口，另一个方向的两侧可能会不够宽而留有黑边

-------------------------------------

<h2 id="qa">Q&A</h2>

+ 直接使用egret命令，提示找不到

    >
  可以使用sudo egret，但是每次都需要输入密码。有没有什么方法不用输入密码，不然经常用起来很麻烦。在磁盘根目录下找到 /usr/local/bin，这个是隐藏文件，找不到的可以先设置显示隐藏文件：defaults write com.apple.finder AppleShowAllFiles -bool true。右键查看“显示简介”，“共享与权限”属性中，把本机用户名添加上去，开放读与写权限就ok了。


--------------------------------------

<h2 id="ts">typescript基础</h2>

1.基本数据类型

+ Boolean
+ Number
+ String
+ Array
```typescript
var list:number[] = [1, 2, 3];
var list:Array<number>=[1, 2, 3];
```
+ Enum
```typescript
enum Color {Red, Green, Blue}
var c: Color = Color.Red;
alert(c);//默认下标从0开始，alert(0);

//可以手动指定下标
enum Color1 {Red = 1, Green, Blue}
var c1: Color1 = Color1.Green;
alert(c1);//alert(2)

//根据下标查找名称
enum Color2 {Red = 1, Green=2, Blue=4}
var c2: string = Color2[4];
alert(c2);//alert(Blue)
```
+ Any
```typescript
//不确定类型，退出编译检查
var notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean

//不确定数组元素类型
var anylist:any[] = [1, true, "free"];
anylist[1] = 100;
```
+ Void
```typescript
//空白
function warnUser(): void {
    alert(123);
}
```

2.类

+ 基本语法
```typescript
class Animal {
    animalName:string;

    constructor(name:string) {
        this.animalName = name;
    }

    sayHello() {
        alert(this.animalName + ": Hello");
    }
}

var tom = new Animal("Tom");
tom.sayHello();//alert(Tom:Hello)
```

+ 继承
```typescript
class Animal {
    animalName:string;

    constructor(name:string) {
        this.animalName = name;
    }

    sayHello() {
        alert(this.animalName + ": Hello");
    }
}

class Cat extends Animal {
    //重写sayHello方法
    sayHello() {
        alert(this.animalName + "(Cat)：" + "Hello");
    }
}

class Mouse extends Animal {
    sayHello() {
        alert(this.animalName + "(Mouse)：" + "Hello");
    }
}

var tom:Animal = new Cat("Tom");
tom.sayHello();//alert(Tom(Cat):Hello)
var jerry:Animal = new Mouse("Jerry");
jerry.sayHello();//alert(Jerry(Mouse):Hello)
```

+ 修饰符

当我们把animalName 改为private
```typescript
class Animal {
    private animalName:string;//默认是public

    constructor(name:string) {
        this.animalName = name;
    }
    //...
}

class Cat extends Animal {
    //重写sayHello方法
    sayHello() {
        alert(this.animalName + "(Cat)：" + "Hello");//Error 编译不通过
    }
}
```

+ get，set 访问器
```typescript
class Animal {
    private _animalName:string;//默认是public

    get animalName():string {
        return this._animalName;
    }

    set animalName(name:string):string {
        this._animalName = name;
    }

    //...
}
```

+ 静态属性
```typescript
//静态属性
class Table {
    static width = 100;
    static height = 200;
}

var width = Table.width;
alert(width);//alert(100)
```

3.接口

+ 基本语法
```typescript
interface ICar {
    color:string;
}

class Bus implements ICar {
    color:string;
    constructor() {
        this.color = "Blue";
    }
}

var bus = new Bus();
alert(bus.color);
```

+ 继承接口
```typescript
//继承接口
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}
```

+ 可选属性
```typescript
interface ICar {
    color:string;
    safetyDevice?:any;//实现类无需实现
}

function MoveCar(car:ICar){
    if(car.safetyDevice)
    {
        alert("The car is safe");
    }
    else
    {
        alert("The car is not safe");
    }
}
```

4.模块（Modules）

作用：
1.防止命名空间冲突；
2.将一个功能模块很容易的划分到不同文件中，更容易维护；

+ 基本语法
```typescript
module MyDemo {
    export interface IDemo {

    }

    export class Demo implements IDemo {

    }
}
```

+ 别名
```typescript
module Shapes {
    export module Polygons {
        export class Triangle { }
        export class Square { }
    }
}

import polygons = Shapes.Polygons;
var sq = new polygons.Square(); // 类似于 'new Shapes.Polygons.Square()'
```

5.函数（Function）

+ 基本语法
```typescript
function add(x:number, y:number):number {
    return x + y;
}
// or
var myAdd = function (x:number, y:number):number {
    return x + y;
};
```

+ 完整的函数类型
```typescript
var myAdd:(x:number, y:number)=>number =
    function (x:number, y:number):number {
        return x + y;
    };
```

为了增强可读性，给参数x、y具有实际的意义，可以这样写
```typescript
var myAdd:(baseValue:number, increment:number)=>number =
    function (x:number, y:number):number {
        return x + y;
    };
```

第二部分number 是一个返回类型，如果无需返回类型，请使用 'void'
第三部分的function 参数类型，根据上下文类型进行推断，可以省略
```typescript
var myAdd:(baseValue:number, increment:number)=>number =
    function (x, y) {
        return x + y;
    };
```

+ 可选参数
```typescript
function buildName(firstName:string, lastName?:string) {
    if (lastName)
        return firstName + " " + lastName;
    else return firstName;
}
var result1 = buildName("Bob");
```

+ 默认参数
```typescript
function buildNameDefaultValue(firstName: string, lastName = "Smith") {
        return firstName + " " + lastName;
}
var result1 = buildNameDefaultValue("Bob");
```

+ 可变参数

例如在C#中，方法参数定义使用param int[],调用方法时，就可以传递多个int类型的参数
在TypeScript中
```typescript
function buildNameRest(firstName:string, ...restOfName:string[]) {
    return firstName + " " + restOfName.join(" ");
}

var employeeName = buildNameRest("Joseph", "Samuel", "Lucas", "MacKinzie")
```

+ Lambads 和this关键字
```typescript
var people={
    name:["张三","李四","王五","赵六"],
    getName:function(){
        return function(){
            var i=Math.floor(Math.random()*4);
            return {
                n:this.name[i]
            }
        }
    }
}

var pname=people.getName();
alert("名字："+pname().n);
调用发现getName中的this关键字指向的是getName,访问不到外部的name属性
所以我们修改为：

var people = {
    name: ["张三", "李四", "王五", "赵六"],
    getName: function () {
        return  ()=> {
            var i = Math.floor(Math.random() * 4);
            return {
                n: this.name[i]
            }
        }
    }
}

var pname = people.getName();
alert("名字：" + pname().n);
```

+ 重载
```typescript
function student(name:string):string;
function student(age:number):number;
function student(numberorage:any):any {
    if (numberorage && typeof (numberorage) == "string")
        alert("姓名");
    else
        alert("年龄");
}
student("Tom");//alert("姓名")
student(15);//alert("年龄")
```

6.泛型

+ 基本语法
```typescript
function identity<T>(arg: T): T {
    return arg;
}

//数组泛型
function identity<T>(arg: T[]): T[] {
    console.log(arg.length);
}
```

+ 泛型类型（通用的函数类型）
```typescript
function identity<T>(arg:T):T {
    return arg;
}
var myIdentity:<T>(arg:T)=>T = identity;//T也可使用其他字母表示
//也可以这么写
//var myIdentity:{<T>(arg:T): T} = identity;
```

+ 接口泛型
```typescript
interface GenericIdentityFn {
    <T>(arg:T): T;
}

function identity<T>(arg:T):T {
    return arg;
}

var myIdentity:GenericIdentityFn = identity;
```

+ 泛型类
```typescript
class GenericNumber<T> {
    zeroValue:T;
    add:(x:T, y:T) => T;
}

var myGenericNumber = new GenericNumber<number>();
myGenericNumber.zeroValue = 0;
myGenericNumber.add = function (x, y) {
    return x + y;
};
```

+ 泛型约束
```typescript
interface Lengthwise {
    length: number;
}

function loggingIdentity<T extends Lengthwise>(arg:T):T {
    console.log(arg.length);
    return arg;
}

loggingIdentity(3);//error
loggingIdentity({length: 10, value: 3});  //只要类型包含length属性即可
```

+ 泛型类约束
```typescript
class Findable<T>
{
    //...
}
function find<T>(n: T, s: Findable<T>) {
    // ...
}
```

7.合并

+ 合并接口
```typescript
interface Box {
    height: number;
    width: number;
}

interface Box {
    scale: number;
}

var box: Box = {height: 5, width: 6, scale: 10};
```

+ 合并模块
```typescript
module Animals {
    exportclass Zebra { }
}

module Animals {
    exportinterface Legged { numberOfLegs: number; }
    exportclass Dog { }
}

//相当于
module Animals {
    exportinterface Legged { numberOfLegs: number; }

    exportclass Zebra { }
    exportclass Dog { }
}
```

+ 合并模块和类
```typescript
class Album {
    label:Album.AlbumLabel;
}
module Album {
    export class AlbumLabel {
    }
}
```

+ 合并模块和函数
```typescript
function buildLabel(name:string):string {
    return buildLabel.prefix + name + buildLabel.suffix;
}

module buildLabel {
    export var suffix = "";
    export var prefix = "Hello, ";
}

alert(buildLabel("Sam Smith"));
```typescript

+ 合并模块与枚举
```typescript
enum Color {
    red = 1,
    green = 2,
    blue = 4
}

module Color {
    export function mixColor(colorName:string) {
        if (colorName == "yellow") {
            return Color.red + Color.green;
        }
        else if (colorName == "white") {
            return Color.red + Color.green + Color.blue;
        }
        else if (colorName == "magenta") {
            return Color.red + Color.blue;
        }
        else if (colorName == "cyan") {
            return Color.green + Color.blue;
        }
    }
}
```