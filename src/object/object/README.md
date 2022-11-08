我们可以通过使用带有可选 属性列表 的花括号 `{…}` 来创建对象。一个属性就是一个键值对（“key: value”），其中键（`key`）是一个字符串（也叫做属性名），值（`value`）可以是任何值。

我们可以把对象想象成一个带有签名文件的文件柜。每一条数据都基于键（key）存储在文件中。这样我们就可以很容易根据文件名（也就是“键”）查找文件或添加/删除文件了。(类似`Hash Map`)

![img](https://zh.javascript.info/article/object/object.svg)

我们可以用下面两种语法中的任一种来创建一个空的对象（“空柜子”）：

```js
let user = new Object(); // “构造函数” 的语法
let user = {}; // “字面量” 的语法 (常用)
```

![img](https://zh.javascript.info/article/object/object-user-empty.svg)

通常，我们用花括号。这种方式我们叫做 **字面量**。

## 文本和属性

我们可以在创建对象的时候，立即将一些属性以键值对的形式放到 `{...}` 中。

```javascript
let user = {
  // 一个对象
  name: "John", // 键 "name"，值 "John"
  age: 30, // 键 "age"，值 30
};
```

生成的 user 对象可以被想象为一个放置着两个标记有 “name” 和 “age” 的文件的柜子。

![img](https://zh.javascript.info/article/object/object-user.svg)

可以使用点符号访问属性值：

```javascript
// 读取文件的属性：
alert(user.name); // John
alert(user.age); // 30
```

属性的值可以是任意类型，让我们加个布尔类型：

```javascript
user.isAdmin = true;
```

![img](https://zh.javascript.info/article/object/object-user-isadmin.svg)

我们可以用 `delete` 操作符移除属性：

```javascript
delete user.age;
```

> 注: 在`ts`中`delete`操作符仅可作用域可选参数, 后续(如果有)在`ts`篇中会详细介绍

![img](https://zh.javascript.info/article/object/object-user-delete.svg)

我们也可以用多字词语来作为属性名，但必须给它们加上引号：

```javascript
let user = {
  name: "John",
  age: 30,
  "likes birds": true, // 多词属性名必须加引号
};
```

![img](https://zh.javascript.info/article/object/object-user-props.svg)

## 方括号访问值

对于多词属性, 点操作符就不能用了:

```javascript
// 这将提示有语法错误
user.likes birds = true
```

`JavaScript` 理解不了。它认为我们在处理 `user.likes`，然后在遇到意外的 `birds` 时给出了语法错误。

点符号要求 `key` 是有效的变量标识符。这意味着：不包含空格，不以数字开头，也不包含特殊字符（允许使用 `$` 和 `_`）。

有另一种方法，就是使用方括号，可用于任何字符串：

```javascript
let user = {};

// 设置
user["likes birds"] = true;

// 读取
alert(user["likes birds"]); // true

// 删除
delete user["likes birds"];
```

**方括号同样提供了一种可以通过任意表达式来获取属性名的方式 —— 与文本字符串不同 —— 例如下面的变量：**

```javascript
let key = "likes birds";

// 跟 user["likes birds"] = true; 一样
user[key] = true;
```

## 计算属性

当创建一个对象时，我们可以在对象字面量中使用方括号。这叫做 计算属性。

```javascript
let fruit = prompt("Which fruit to buy?", "apple");

let bag = {
  [fruit]: 5, // 属性名是从 fruit 变量中得到的
};

alert(bag.apple); // 5 如果 fruit="apple"
```

计算属性的含义很简单：`[fruit]` 含义是属性名应该从 fruit 变量中获取。

所以，如果一个用户输入 "apple"，bag 将变为 `{apple: 5}`。

## 属性值简写

```javascript
let name = "John";
let user = {
  name,
  age: 30,
};
```

等价于:

```javascript
let name = "John";
let user = {
  name: name,
  age: 30,
};
```

## 属性存在值测试

相比于其他语言，JavaScript 的对象有一个需要注意的特性：能够被访问任何属性。即使属性**不存在**也不会报错！

读取不存在的属性只会得到 `undefined`。所以我们可以很容易地判断一个属性是否存在：

```javascript
let user = {};

alert(user.noSuchProperty === undefined); // true 意思是没有这个属性
```

这里还有一个特别的，检查属性是否存在的操作符 "in"。

语法是：

```javascript
"key" in object;
```

例如：

```javascript
let user = { name: "John", age: 30 };

alert("age" in user); // true，user.age 存在
alert("blabla" in user); // false，user.blabla 不存在。
```

请注意，in 的左边必须是 属性名。通常是一个带引号的字符串。

如果我们省略引号，就意味着左边是一个**变量**，它应该包含要判断的实际属性名。例如：

```js
let user = { age: 30 };

let key = "age";
alert(key in user); // true，属性 "age" 存在
```

`in`操作符存在的理由为: 当存储值为 `undefined` 时, 通过 `object.key` 判断会出错:

```js
let obj = {
  test: undefined,
};

alert(obj.test); // 显示 undefined，所以属性不存在？

alert("test" in obj); // true，属性存在！
```

## `for...in`循环

为了遍历一个对象的所有键（`key`），可以使用一个特殊形式的循环：`for..in`。这跟我们在前面学到的 for(;;) 循环是完全不一样的东西。

```js
for (key in object) {
  // 对此对象属性中的每个键执行的代码
}
```

例如，让我们列出 `user` 所有的属性：

```js
let user = {
  name: "John",
  age: 30,
  isAdmin: true,
};

for (let key in user) {
  // keys
  alert(key); // name, age, isAdmin
  // 属性键的值
  alert(user[key]); // John, 30, true
}
```

