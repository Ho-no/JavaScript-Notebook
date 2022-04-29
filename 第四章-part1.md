# 原始值和引用值

上一章讨论了 6 种原始值：Undefined、Null、Boolean、Number、String 和 Symbol。保存原始值的变量是按值（by value）访问的，因为我们操作的就是存储在变量中的实际值。

引用值是保存在内存中的对象。与其他语言不同，JavaScript 不允许直接访问内存位置，因此也就不能直接操作对象所在的内存空间。在操作对象时，实际上操作的是对该对象的引用（reference）而非实际的对象本身。为此，保存引用值的变量是按引用（by reference）访问的。

在很多语言中，字符串是使用对象表示的，因此被认为是引用类型。ECMAScript 打破了这个惯例。

## 动态属性

对于引用值，可以随时添加、修改和删除属性及方法。

原始值不能有属性，尽管尝试给原始值添加属性不会报错。比如：
    
    let name = "Nicholas"; 
    name.age = 27; 
    console.log(name.age); // undefined 

但可以使用包装类型来添加属性：
    
    let name = new String("Nicholas"); 
    name.age = 27; 
    console.log(name.age); // 27

## 复制值

将一个原始值赋值给另一个变量，会复制该值的值，而不是引用。但把引用值赋值给另一个变量，则会复制该引用，两个变量实际上指向同一个变量。（指针）

## 传递参数

ES 中所有函数的参数都是值传递的。以下这段代码可以反映其与引用传递的区别：

    function setName(obj) { 
        obj.name = "Nicholas"; 
        obj = new Object(); 
        obj.name = "Greg"; 
    } 
    let person = new Object(); 
    setName(person); 
    console.log(person.name); // "Nicholas" 

ES 中函数的参数都是局部变量。

## 确定类型

ES 中停供了 instanceof 操作符，用来确定一个值是否是某个类的实例。如果变量是给定引用类型（参见第 8 章）的实例，则 instanceof 操作符返回 true。

使用 instanceof 检测原始值始终返回 false，因为它们不是对象。

## 执行上下文与作用域

