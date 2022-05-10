# 原始值包装类型

ES 提供了三种原始值包装类型：Boolean、Number、String。

原始值本身不应该有属性或方法。每当用户使用某个原始值的方法或属性时，后台会临时创建一个包装类型对象。

引用类型与原始值包装类型的主要区别在于对象的生命周期。自动创建的原始值包装对象则只存在于访问它的那行代码执行期间。这意味着不能在运行时给原始值添加属性和方法。

    let s1 = "some text"; 
    s1.color = "red"; 
    console.log(s1.color); // undefined

由于所有的对象都会转换为 true，所有原始值包装对象都会转换为布尔值 true。

Object 构造函数作为一个工厂方法，能够根据传入值的类型返回相应原始值包装类型的实例。

    let obj = new Object("some text"); 
    console.log(obj instanceof String); // true

## Boolean

Boolean 的实例会重写 valueOf()方法，返回一个原始值 true 或 false。toString()方法被调用时也会被覆盖，返回字符串"true"或"false"。

Boolean 的实例即使保存 false，也会被转换为 true。

    let falseObject = new Boolean(false); 
    let result = falseObject && true; 
    console.log(result); // true 

    let falseValue = false; 
    result = falseValue && true; 
    console.log(result); // false

## Number

toFixed() 方法接受一个数字作为参数指定保留的位数，返回一个字符串，表示该数字的四舍五入后的结果。

toExponential() 方法接受一个数字作为参数指定保留的位数，返回一个字符串，表示该数字的科学计数法表示。

toPrecision() 方法接受一个数字作为参数指定结果章数字的位数，返回一个字符串，表示该数字的精确表示。这个方法会根据情况选择调用 toFixed() 或 toExponential()。

## String

每个 String 对象都有一个 length 属性，表示字符串中字符的数量。

1. JavaScript 字符

    JavaScript 字符串由 16 位码元组成。

    charAt() 方法返回给定索引位置的字符。

    JavaScript 字符串使用了两种 Unicode 编码混合的策略：UCS-2 和 UTF-16。对于可以采用 16 位编码的字符（U+0000~U+FFFF），这两种编码实际上是一样的。

    charCodeAt() 方法返回给定索引位置的字符的码元值。