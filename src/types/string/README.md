# 字符串

## 特殊字符

我们仍然可以通过使用“换行符（newline character）”，以支持使用单引号和双引号来创建跨行字符串。换行符写作 `\n`，用来表示换行：

```javascript
let guestList = "Guests:\n * John\n * Pete\n * Mary";

alert(guestList); // 一个多行的客人列表
```

所有的特殊字符都以反斜杠字符 `\` 开始。它也被称为“转义字符”。

```javascript
alert("I'm the Walrus!"); // I'm the Walrus!
```

当然，只有与外部闭合引号相同的引号才需要转义。因此，作为一个更优雅的解决方案，我们可以改用双引号或者反引号：

```js
alert(`I'm the Walrus!`); // I'm the Walrus!
```

但是如果我们需要在字符串中显示一个实际的反斜杠 \ 应该怎么做？

我们可以这样做，只需要将其书写两次 \\：

```javascript
alert(`The backslash: \\`); // The backslash: \
```

## 访问字符

要获取在 pos 位置的一个字符，可以使用方括号 `[pos]` 或者调用 `str.charAt(pos)` 方法。第一个字符从零位置开始：

```javascript
let str = `Hello`;

// 第一个字符
alert(str[0]); // H
alert(str.charAt(0)); // H

// 最后一个字符
alert(str[str.length - 1]); // o
```

方括号是获取字符的一种现代化方法，而 `charAt` 是历史原因才存在的。

它们之间的唯一区别是，如果没有找到字符，[] 返回 undefined，而 charAt 返回一个空字符串：

```js
let str = `Hello`;

alert(str[1000]); // undefined
alert(str.charAt(1000)); // ''（空字符串）
```

## 字符串是不可变的

在 JavaScript 中，字符串不可更改。改变字符是不可能的。
我们证明一下为什么不可能：

```javascript
let str = "Hi";

str[0] = "h"; // error
alert(str[0]); // 无法运行
```

通常的解决方法是创建一个新的字符串，并将其分配给 str 而不是以前的字符串。

例如：

```javascript
let str = "Hi";

str = "h" + str[1]; // 替换字符串

alert(str); // hi
```

> 字符串不可更改但是可以赋值

```javascript
let s1 = "123";
let s2 = "456";
s2 = s1;
console.log(s2); // "123"
```
