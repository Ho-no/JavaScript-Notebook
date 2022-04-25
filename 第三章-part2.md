# String 类型

字符串可以用双引号 (")，单引号 (')，反引号 (`)标示。

字符串的长度可以通过其属性 length 获取。

ECMAScript 中的字符串是不可变的，一旦创建，其值就不能被改变。要修改某个变量中的字符串，必须先销毁原来的字符串，然后将包含新值的另一个字符串保存到该变量。


## toString() 与 String()

toString() 方法可见于数值、布尔值、对象和字符串。在对数值调用这个方法时，toString() 方法可以接受一个参数用以指定表示的进制。需要注意的是 null 和 undefined 没有 toString() 方法。

如果不确定一个值是否是 null 或者 undefined，可以使用 String()。String() 函数遵从以下规则：
- 如果值有 toString() 方法，调用并返回（不传参数）。
- 如果是 null 返回 "null"，如果是 undefined 返回 "undefined"。

## 模板字面量

使用反引号可以定义模板字面量。与单双引号定义的字面量不同，模板字面量保留换行字符，可以跨行定义字符串。

模板字面量可以使用 ${} 在字符串中插入变量，任何插入的变量都会使用 toString() 强制转型为字符串。（错误：插入 undefined 以及 null 值表现正常，使用的应该是 String()。）

模板字面量支持定义标签函数。标签函数本身是一个常规函数，但将模板字面量作为参数传入后，函数接受到的参数是原始字符串数组和模板字面量中每个表达式的求值结果。

因为表达式参数的数量是可变的，应该使用剩余操作符将他们收集到一个数组中 (strings, ...expressions)。

String.raw 函数接收一个模板字面量，将其中的原始内容不转义直接输出。

特殊的是 '\n' 可以不转义，实际的换行则不行。

标签函数的第一个参数，即字符串数组，存在 raw 属性，可以取得每个字符数组中每个字符串的原始内容构成的数组。


# Symbol 类型

Symbol 是 ES6 新增的数据类型。符号是原始值，且符号实例是唯一的、不可变的。

## 符号的基本用法

符号需要使用 Symbol() 函数初始化。typeof 对符号返回 symbol。

调用 Symbol() 函数时，可以传入一个字符串参数作为符号的描述，但这个字符串参数与符号定义或标识无关。

    
`console.log(Symbol('foo') == Symbol('foo')) // false;`

因此，如果用 Symbol 实例用作对象的属性，可以保证它不会覆盖已有的对象属性。

Symbol() 函数不能像 Boolean, String 或 Number 那样搭配 new 关键字作为构造函数使用。不能用这种方法创建 Symbol 类型的包装对象（参见 5.3 原始值包装类型）。

## 使用全局符号注册表

如果运行时的不同部分需要共享和重用符号实例，那么可以用一个字符串作为键，在全局符号注册表中创建并重用符号。

Symbol.for() 方法对每个字符串键都执行幂等操作。第一次使用某个字符串调用时，它会检查全局运行时注册表，发现不存在对应的符号时，会生成一个新符号实例并添加到注册表中。后续使用相同字符串的调用同样会检查注册表，发现存在与该字符串对应的符号，然后就会返回该符号实例。

`console.log(Symbol.for('foo') == Symbol.for('foo')) // true`

即使采用相同的符号描述，在全局注册表中定义的符号跟使用 Symbol() 定义的符号也并不等同

`console.log(Symbol.for('foo') == Symbol('foo')) // false`

全局注册表中的符号必须使用使用字符串键来创建。任何传入的值都会被转换为字符串。

`console.log(Symbol.for()) // Symbol(undefined)`

可以使用 Symbol.keyFor() 来查询全局注册表中的字符串键。这个方法接收符号，返回该全局符号对应的字符串键。如果查询的不是全局符号，返回 undefined。如果传入的不是符号，抛出 TypeError。

## 使用符号作为属性

凡是可以使用字符串或数值作为属性的地方，都可以使用符号。这就包括了对象字面属性和 Object.defineProperty() / Object.defineProperties() 定义的属性（参见 8.1）。

    let s = Symbol()

    let o = {
        [s1] : 'val'
    }

Object.getOwnPropertyNames() 接收一个对象，返回对象实例的常规属性数组。Object.getOwnPropertySymbols() 返回对象实例的符号属性数组。Reflect.ownKeys() 会返回两种类型的键。Object.getOwnPropertyDescriptors() 会返回一个对象，对象中包含常规和符号属性的描述。

## 常用内置符号

ES6 引入了一些内置符号用以暴露语言内部行为。这些内置符号最重要的用途之一是重新定义它们，从而改变原生结构的行为。比如，我们知道 for-of 循环会在相关对象上使用 Symbol.iterator 属性，那么就可以通过在自定义对象上重新定义Symbol.iterator 的值，来改变 for-of 在迭代该对象时的行为。

## Symbol.asyncIterator

这个符号由 for-await-of 语句使用（ES9，参见附录 A）。

## Symbol.hasInstance

这个符号由 instanceof 操作符使用。

    function Foo() {}
    let f = new Foo()
    console.log(f instanceof Foo)
    //等价于
    console.log(Foo[Symbol.hasInstance](f))

这个属性定义在 Function 的原型上，因此默认在所有函数和类上都可以调用。可以在继承的类上通过静态方法重新定义这个函数。

    class Foo {
        static [Symbol.hasInstance]() {
            return false;
        }
    }

    let f = new Foo();
    console.log(f instanceof Foo) // false;

## Symbol.isConcatSpreadable

这个符号作为一个属性表示一个布尔值，如果是 true，则该对象应该用 Array.prototype.concat() 打平其数组元素（参见 6.2.11）。

打平意为将数组中的元素摊开。如果不打平，concat 会将整个数组作为一个对象进行追加。

    let initial = ['foo']
    let array = ['bar']; 
    console.log(array[Symbol.isConcatSpreadable]);  // undefined 
    console.log(initial.concat(array));             // ['foo', 'bar'] 
    array[Symbol.isConcatSpreadable] = false; 
    console.log(initial.concat(array));             // ['foo', Array(1)]

## Symbol.iterator

这个符号作为一个属性表示一个方法，该方法返回对象默认的迭代器。这个符号由 for-of 语句使用。

    class Emitter {
        constructor(max) {
            this.max = max;
            this.idx = 0;
        }

        *[Symbol.iterator]() {
            while (this.idx < this.max) {
                yield this.idx++;
            }
        }
    }

    function count() {
        let emitter = new Emitter(5);

        for (let n of emitter) {
            console.log(n);
        }
    }

    count();
    // 0
    // 1
    // 2
    // 3
    // 4

参见第七章。

## Symbol.match

这个符号由 String.prototype.match() 方法使用。String.prototype.match() 方法会使用以 Symbol.match 为键的函数来对正则表达式求值。RegExp 的原型链上有该函数的定义，因此所有 RegExp 实例默认是这个 String 方法的有效参数。

    class StringMatcher { 
        constructor(str) { 
            this.str = str; 
        } 
        
        [Symbol.match](target) { // target 是调用 match 方法的字符串实例
            return target.includes(this.str); 
        } 
    } 
    
    console.log('foobar'.match(new StringMatcher('foo'))); // true 
    console.log('barbaz'.match(new StringMatcher('qux'))); // false


## Symbol.replace

这个符号由 String.prototype.replace() 方法使用。Symbol.replace 函数接收两个参数，即调用 replace() 方法的字符串实例和替换字符串。



## Symbol.search

这个符号由 String.prototype.search() 方法使用。该方法返回字符串中匹配正则表达式的索引。

## Symbol.species

这个符号作为一个属性表示“一个函数值，该函数作为创建派生对象的构造函数”。这个属性在内置类型中最常用，用于对内置类型实例方法的返回值暴露实例化派生对象的方法。（不知所云）

    class MyArray extends Array {
        static get [Symbol.species]() { // static get 创建一个伪属性
            return Array;
        }
    }

    let a = new MyArray(1, 2, 3);
    let b = a.map(x => x * x);
    console.log(b instanceof MyArray); // false
    console.log(b instanceof Array);   // true

## Symbol.split

这个符号由 String.prototype.split() 方法使用。

    class StringSplitter {
        constructor(str) {
            this.str = str;
        }

        [Symbol.split](target) {
            return target.split(this.str);
        }
    }

    console.log('foobar'.split(new StringSplitter('o'))); // ['f', 'bar']

## Symbol.toPrimitive

这个符号由 ToPrimitive 抽象操作使用（参见 3.5）。ToPrimitive 操作会把一个对象转换为原始值。

在 Symbol.toPrimitive 属性(用作函数值)的帮助下，一个对象可被转换为原始值。该函数被调用时，会被传递一个字符串参数 hint ，表示要转换到的原始值的预期类型。 hint 参数的取值是 "number"、"string" 和 "default" 中的任意一个。

## Symbol.toStringTag

这个符号作为一个属性表示对象的默认字符串描述，该方法返回对象的字符串标签。这个符号由 Object.prototype.toString() 方法使用。

## Symbol.unscopables

with语句扩展一个语句的作用域链。设置这个符号并让其映射对应属性的键值为 true，就可以阻止该属性出现在 with 环境绑定中。

不推荐使用 with，它可能是混淆错误和兼容性问题的根源。因此也不推荐使用 Symbol.unscopables。