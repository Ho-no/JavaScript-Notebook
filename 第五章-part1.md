# Date

ECMAScript 的 Date 类型将日期保存为自1970 年 1 月 1 日午夜（零时）至今所经过的毫秒数。

Date.parse() 方法接受一个表示日期的字符串：
- “月/日/年”，如"5/23/2019"；
- “月名 日, 年”，如"May 23, 2019"；
- “周几 月名 日 年 时:分:秒 时区”，如"Tue May 23 2019 00:00:00 GMT-0700"；
- ISO 8601 扩展格式“YYYY-MM-DDTHH:mm:ss.sssZ”，如 2019-05-23T00:00:00

如果传给 Date.parse() 的字符串不能被解析，则返回 NaN。

如果直接把字符传给 Date 构造函数，那么 Date 会调用 Date.parse() 来解析。

Date.UTC() 方法接受数字参数的年、月（0-11）、日（1-31）、时（0-23）、分、秒和毫秒。

    // GMT 时间 2000 年 1 月 1 日零点
    let y2k = new Date(Date.UTC(2000, 0)); 
    // GMT 时间 2005 年 5 月 5 日下午 5 点 55 分 55 秒
    let allFives = new Date(Date.UTC(2005, 4, 5, 17, 55, 55));

与 Date.parse()一样，Date.UTC()也会被 Date 构造函数隐式调用，但有一个区别：这种情况下创建的是本地日期，不是 GMT 日期。

Date.now() 方法返回当前时间的毫秒数。

## 继承的方法

toLocaleString() 方法返回与浏览器运行的本地环境一致的日期和时间。12小时制，不包含时区信息。

toString()方法通常返回带时区信息的日期和时间，而时间也是以 24 小时制（0~23）表示的。

valueOf() 方法返回日期的毫秒表示。因此，操作符（如大于号小于号）可以直接使用它的返回值。

## 日期格式化方法。

- toDateString()显示日期中的周几、月、日、年。
-  toTimeString()显示日期中的时、分、秒和时区。
-  toLocaleDateString()显示包含时区的日期中的周几、月、日、年。
-  toLocaleTimeString()显示包含时区的日期中的时、分、秒。
-  toUTCString()显示完整的 UTC 日期。

以上发放的具体输出与浏览器实现有关。

## 日期/时间组件方法

Date 类型有 get 和 set 方法。

# RegExp

ECMAScript 通过 RegExp 类型支持正则表达式。正则表达式使用类似 Perl 的简洁语法来创建

    let expression = /pattern/flags; 

pattern 是正则表达式，flags 是正则表达式的标志。

下面给出表示匹配模式的标志：

- g: 全局匹配，即在整个字符串中搜索。
- i: 忽略大小写。
- m: 多行匹配，查找到行末时会继续查找。
- y: 粘附模式，表示只查找从 lastIndex 开始及之后的字符串。 
- u: Unicode 匹配
- s: dotAll 模式，即 . 匹配所有字符，包括换行符。
    
```
    /at/g //匹配字符串中所有的 at
    /[bc]at/i //匹配字符串中所有的 bat 或 cat，忽略大小写
    /.at/gi //匹配字符串中所有以 at 结尾的三字符组合，忽略大小写
```

元字符可以用 \ 转义。

RegExp 可以直接用字面量创建，也可以向构造函数传入两个表示 pattern 和 flag 的字符串来创建。需要注意的是，字符串表示正则表达式的转义时，需要二次转义。

此外，创建 RegExp 也可以基于已有的实例。

    const re1 = /cat/g; 
    console.log(re1); // "/cat/g" 

    const re2 = new RegExp(re1); 
    console.log(re2); // "/cat/g"

    const re3 = new RegExp(re1, "i"); 
    console.log(re3); // "/cat/i" 

## RegExp 实例属性

- global: 布尔值，表示是否设置 g 标记。
- ignoreCase: 布尔值，表示是否设置 i 标记。
- unicode: 布尔值，表示是否设置 u 标记。
- sticky: 布尔值，表示是否设置 y 标记。
- multiline: 布尔值，表示是否设置 m 标记。
- dotAll: 布尔值，表示是否设置 s 标记。
- lastIndex: 整数，表示下一次匹配的起始位置。
- source: 字符串，以字面量格式表示正则表达式的模式串（没有开头和结尾的斜杠）。
- flags: 字符串，以字面量格式表示正则表达式的标志（没有开头和结尾的斜杠）。
  
## RegExp 实例方法

RegExp 实例的主要方法是 exec()；这个方法接受一个字符串，返回一个包含匹配信息的数组，如果没有匹配，返回 null。

返回的数组虽然是 Array 实例，但包含两个额外的属性：index 和 input。index 是字符串中模式匹配的起始位置，input 是要查找的字符串。