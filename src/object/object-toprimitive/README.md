## 转换规则

1. 不会转换为布尔值。所有的对象在布尔上下文（context）中均为 true，就这么简单。只有字符串和数字转换。
2. 数字转换发生在对象相减或应用数学函数时。例如，Date 对象（将在 `日期和时间` 一章中介绍）可以相减，`date1 - date2` 的结果是两个日期之间的差值。
3. 至于字符串转换 —— 通常发生在我们像 alert(obj) 这样输出一个对象和类似的上下文中。

## hint

`JavaScript` 是如何决定应用哪种转换的？

类型转换在各种情况下有三种变体。它们被称为 “hint”，在 规范 所述：

**`"string"`**

对象到字符串的转换，当我们对期望一个字符串的对象执行操作时，如 “alert”：

```javascript
// 输出
alert(obj);

// 将对象作为属性键
anotherObj[obj] = 123;
```

**`number`**

对象到数字的转换，例如当我们进行数学运算时：

```javascript
// 显式转换
let num = Number(obj);

// 数学运算（除了二元加法）
let n = +obj; // 一元加法
let delta = date1 - date2;

// 小于/大于的比较
let greater = user1 > user2;
```

**`default`**

在少数情况下发生，当运算符“不确定”期望值的类型时。

例如，二元加法 + 可用于字符串（连接），也可以用于数字（相加）。因此，当二元加法得到对象类型的参数时，它将依据 `"default"` hint 来对其进行转换。

```javascript
// 二元加法使用默认 hint
let total = obj1 + obj2;

// obj == number 使用默认 hint
if (user == 1) { ... };
```

像 `<` 和 `>` 这样的小于/大于比较运算符，也可以同时用于字符串和数字。不过，它们使用 `“number”` hint，而不是 `“default”`。这是历史原因。

**为了进行转换，JavaScript 尝试查找并调用三个对象方法：**

1. 调用 `obj[Symbol.toPrimitive](hint)` —— 带有 `symbol` 键 `Symbol`.`toPrimitive`（系统 `symbol`）的方法，如果这个方法存在的话
2. 否则，如果 `hint` 是 `"string"` —— 尝试调用 `obj.toString()` 或 `obj.valueOf()`，无论哪个存在。
3. 否则，如果 `hint` 是 `"number"` 或 `"default"` —— 尝试调用 `obj.valueOf()` 或 `obj.toString()`，无论哪个存在。

## Symbol.toPrimitive

我们从第一个方法开始。有一个名为 `Symbol.toPrimitive` 的内建 `symbol`，它被用来给转换方法命名，像这样：

```js
let user = {
  name: "John",
  money: 1000,
  // hint方法
  [Symbol.toPrimitive](hint) {
    alert(`hint: ${hint}`);
    return hint == "string" ? `{name: "${this.name}"}` : this.money;
  },
};

// 转换演示：
alert(user); // hint: string -> {name: "John"}
alert(+user); // hint: number -> 1000
alert(user + 500); // hint: default -> 1500
```

从代码中我们可以看到，根据转换的不同，`user` 变成一个自描述字符串或者一个金额。`user[Symbol.toPrimitive]` 方法处理了所有的转换情况。

## `toString/valueOf`

如果没有 `Symbol.toPrimitive`，那么 `JavaScript` 将尝试寻找 `toString` 和 `valueOf` 方法：

- 对于 `"string"` hint：调用 `toString` 方法，如果它不存在，则调用 `valueOf` 方法（因此，对于字符串转换，优先调用 `toString`）。
- 对于其他 `hint`：调用 `valueOf` 方法，如果它不存在，则调用 `toString` 方法（因此，对于数学运算，优先调用 `valueOf` 方法）。

`toString` 和 `valueOf` 方法很早己有了。它们不是 `symbol`（那时候还没有 symbol 这个概念），而是“常规的”字符串命名的方法。它们提供了一种可选的“老派”的实现转换的方法。

默认情况下，普通对象具有 `toString` 和 `valueOf` 方法：

`toString` 方法返回一个字符串 `"[object Object]"`。
`valueOf` 方法返回对象自身。

> 注: toString 方法常被用来判断元素类型, 详情见[准确的类型判断](https://github.com/tohsaka888/WhatIsJs/tree/master/src/JavaScript%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B#%E5%87%86%E7%A1%AE%E7%9A%84%E7%B1%BB%E5%9E%8B%E5%88%A4%E6%96%AD-objectprototypetostringcall)

下面是一个示例：

```javascript
let user = { name: "John" };

alert(user); // [object Object]
alert(user.valueOf() === user); // true
```

所以，如果我们尝试将一个对象当做字符串来使用，例如在 `alert` 中，那么在默认情况下我们会看到 `[object Object]`。

这里提到的默认的 `valueOf` 只是为了完整起见，以避免混淆。正如你看到的，它返回对象本身，因此被忽略。别问我为什么，这是历史原因。所以我们可以假设它根本就不存在。

有的时候, 我们不需要默认的`toString`方法, 相反的我们希望调用`toString`方法时发生一些别的事...比如: 转化为`JSON`字符串。

```javascript
const user = {
  name: "admin",
  pass: 123456,
  toString() {
    return JSON.stringify(this);
  },
};

console.log(user.toString()); // {"name":"admin","pass":123456}
```

## 总结

对象到原始值的转换，是由许多期望以原始值作为值的内建函数和运算符自动调用的。

这里有三种类型`（hint）`：

- "string"（对于 alert 和其他需要字符串的操作）
- "number"（对于数学运算）
- "default"（少数运算符，通常对象以和 "number" 相同的方式实现 "default" 转换）

规范明确描述了哪个运算符使用哪个 `hint`。

转换算法是：

1. 调用 `obj[Symbol.toPrimitive](hint)` 如果这个方法
2. 否则，如果 `hint` 是 `"string"`, 尝试调用 `obj.toString()` 或 `obj.valueOf()`，无论哪个存在。(先调用`toString()`)
3. 否则，如果 `hint` 是 `"number"` 或者 `"default"`, 尝试调用 `obj.valueOf()` 或 `obj.toString()`，无论哪个存在。(先调用`valueOf()`)

在实际使用中，通常只实现 `obj.toString()` 作为字符串转换的“全能”方法就足够了，该方法应该返回对象的“人类可读”表示，用于日志记录或调试。(当然你也可以改写)
