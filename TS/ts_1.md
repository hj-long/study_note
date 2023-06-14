1. 通俗讲述什么是TS

    ts是一门语言，这门语言和js有点类似

    ts是js的“类型”超集

2. 为什么要用TS

    别把问题放在运行后，把问题放在开发时（在开发就能通过类型检测发现问题）

    TS: 强类型语言

3. 环境搭建

    [环境搭建教程](https://www.xuexiluxian.cn/blog/detail/8b8e67922a714696820cd7cefc0526ab)

4. 基本使用 

    编译：``tsc helloworld.ts``, 会在同一文件夹下生成对应的 js 文件

    监听编译：``tsc -w helloworld.ts``， 实时生成 js 

    生成 ts.config.json 文件：``tsc --init``


5. 数组约束写法

- 2.1 数组每一个成员必须是xx类型

    let arr1: number[] = [1, 2, 3, 4];

    let arr2: string[] = ['a', 'b', 'c'];

    let arr3:Array<number> = [2, 3,4];


 - 数组中可能存在多个类型

    let arr4: (string | number)[] = [1，'b'];  //这个不看位置
    
    let arr5: [number,string] = [1,'b'];    //这个是严格按照位置排序的


6. void : 函数无返回值


7. 对象约束写法


8. 函数约束写法

- 函数参数约束和返回值约束

    ```
    function add( a:number, b:number): number {
        return a + b
    }

    add(1, 2)  // 3

    ```
    // 上面的函数 add 的参数限定为number类型，返回值限定为 number 类型，如果 是 void ,则表示没有返回值

- 可选参数，只需要在参数类型前面加个 ？ 即可，例如：``b?: number``，表示参数 b 可能不存在，不仅函数可以，其他如对象数组等也可以使用可选

- 默认值

    ```
    function add( a:number, b:number = 10): number {
        return a + b
    }

    add(1)  // 11

    ```

- 箭头函数约束

    ```
    let func1: (p1:number, p2:string) => string = (a:number, b:string):string => {
        return a + b;
    }

    ```

    其中，``(p1:number, p2:string) => string`` 表示箭头函数约束，``(a:number, b:string):string``,表示该函数的参数、返回值约束，有点繁琐~


9. interface 接口

    - 自定义的一个数据结构

    - 使用：

        ```
        interface Base {
            name: string;
            age: number;
        }

        let obj:Base = {
            name: 'ss',
            age: 5
        }
        ```

    ***注意*** 
        
        1. 首字母要大写，一般是 I 开头，如 Idata、Idemo

        2. 写完每个属性后面以;结尾

    - 继承 extends，相当于累加

        ```
        interface IBase {
            name: string;
            age: number;
        }

        interface Ichild extends IBase {
            data: {
                a: number;
                b: string;
            }
        }

        let obj:Ichild = {
            name: 'ss',
            age: 5,
            data: {
                a: 1,
                b: 's'
            }
        }

        ```


10. 类 

- 用法

```
    class Person {
        name: string;
        age: number;
        constructor(name: string, age: number) {
            this.name = name;
            this.age = age;
        };
        sayName(): string {
            return `我是${this.name},今年${this.age}岁`
        }
    }

    let ming = new Person('小明', 18)

    console.log(ming.sayName())

```
    
在上面的 person 类中，name、age 是类本身的属性约束，在 constructor 中有参数的约束，方法 sayName 有返回值约束

- 修饰符

    readonly：只读

        ```
        readonly name: string;

        ```

        只要在属性前面加上只读修饰符，那么这个属性就不能被修改

    public：公开的，在任何地方都可以访问

    protected：受保护的，只能在当前类和当前类的子类内部使用

        就是说，只能通过当前类及继承此类的子类中的内部方法通过this.xx能够访问到，如果使用实例.xx是访问不了的，

    private：私有的，当前类的内部使用

        只能在当前类的内部通过 this.xx 使用

- 抽象类: abstract
    
    1. 不完成具体的功能

    2. 抽象类不能new

    3. 抽象类可以继承，如果要继承，就必须实现该类的抽象方法

    ```
    abstract class Person {
        abstract sayName(): void;
        abstract run():void;
    }

    class Child extends Person {
        sayName(): void {
            console.log('say name')
        }
        run(): void {
            console.log('run')
        }
    }
    ```

    4. 使用场景： 例如封装一个链接数据库的约束类，这个类不实现具体功能，但是需要链接每一款数据库和其中的函数操作

    ```
    abstract class Db{
        abstract connection():void;
        abstract auth():void;
    }

    class mySql extends Db{
        connection(){

        }
        auth(){
        
        }
    }
    new mySql();

    ```

- 类的约束：implements, 可以累加

    ```
    interface Ip1 {
        name: string;
    }

    interface Ip2 {
        sayName():string;
    }

    class Person implements Ip1, Ip2 {
        name: string;
        sayName(): string {
            return this.name
        }
    }

    ```

11. 泛型

    - 功能跟形参差不多：是指在定义函数、接口或类的时候，不预先指定具体的类型，而在使用的时候再指定类型的一种特性。

    - 函数泛型(在使用的时候再去指定类型)

        ```
        function fun1<T>( name:T ): T{
            return name;
        }
        fun1<string>('123');
        fun1<number>(123);

        ```

    - 使用多个泛型

        ```
        function func1<T, U>(a:T, b:U){
            console.log(a, b)
        } 

        func1<number, string>(1, '2')

        ```

    - 结合 class

        ```
        class Person<T>{
            userName:T
            constructor( name:T ){
                this.userName = name;
            }
        }
        let person = new Person<string>('张三');

        ```

12. xxx.d.ts 全局生命文件：里面写的 interface 可以在项目中某一个文件直接使用


13. ts 中的装饰器

- 装饰器:就是一个方法，可以注入到类、方法、属性参数上来“扩展"类、属性、方法、参
数的功能。

- 类装饰器（无法传参数）

    ```
    function demo1( target:any ) {
        console.log('类装饰器')
    }
    @demo1
    class Person {}

    let p = new Person()  // 输出："类装饰器"

    ```
    装饰器函数的 target 参数为要装饰的类对象，可以通过 target.prototype.xx = 'xxxx' 为类添加属性

- 装饰器工厂（可以传参数）

    ```
    function demo2( option:any ) {
        console.log('装饰器参数：',option)
        return function( target:any ) {
            console.log('装饰器 内部')
        }
    }
    @demo2('dd')
    class Person {}

    let p = new Person()  //输出："装饰器参数：dd   装饰器 内部"
    ```

    option 为传递的参数，在装饰器内部返回的函数为类装饰器函数，其中的target为被装饰的类对象

- 装饰器组合

    ```
    function demo1( target:any ) {
        console.log('装饰器1')
    }
    function demo2( option:any ) {
        console.log('装饰器2',option)
        return function( target:any ) {
            console.log('装饰器2 内部')
        }
    }
    function demo3( option:any ) {
        console.log('装饰器3', option)
        return function( target:any ) {
            console.log('装饰器3 内部')
        }
    }
    function demo4( target:any ) {
        console.log('装饰器4')

    }

    @demo1
    @demo2('参数2')
    @demo3({name:'参数3'})
    @demo4
    class Person {}

    let p = new Person()

    ```
    执行顺序为：

    ```
    装饰器2 参数2
    装饰器3 { name: '参数3' }
    装饰器4
    装饰器3 内部
    装饰器2 内部
    装饰器1
    ```

    ***注意***：结合起来一起使用的时候, 会先 ``从上至下`` 的执行所有的 ``装饰器工厂``, 拿到所有真正的装饰器, 然后再 ``从下至上`` 的执行所有的 ``装饰器``

- 属性装饰器

    ```
    function attrDemo(target: any, attr: any) {
        target[attr] = 'bb';
    }
    
    class Person {
        @attrDemo
        name: string = 'jj';
    }
    
    let p = new Person();
    console.log(p.name); // 输出 'bb'
    ```
    target 是类本身，attr 是类的属性名

- 方法装饰器

    ```
    function test( target: any, propertyKey: string, descriptor: PropertyDescriptor ) {
        console.log( '执行方法' );
    }

    class Person {
        @test
        sayName() {
            console.log( 'say name...' );
        }
    }

    let p = new Person();
    p.sayName();

    ```
    第一个参数target： 对于静态方法来说是类的构造函数，对于实例方法则是类的原型对象

    第二个参数propertyKey： 是方法的名称

    第三个参数descriptor： 是被装饰的方法的属性描述符，包含方法的属性值和方法的可枚举、可写、可配置等特性。




