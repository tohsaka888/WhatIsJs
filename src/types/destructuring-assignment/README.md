# 解构赋值

## 数组解构

这是一个将数组解构到变量中的例子：

```js
// 我们有一个存放了名字和姓氏的数组
let arr = ["John", "Smith"];

// 解构赋值
// 设置 firstName = arr[0]
// 以及 surname = arr[1]
let [firstName, surname] = arr;

alert(firstName); // John
alert(surname); // Smith
```

我们可以使用这些变量而非原来的数组项了。

当与 `split` 函数（或其他返回值为数组的函数）结合使用时，看起来更优雅：

```javascript
let [firstName, surname] = "John Smith".split(" ");
alert(firstName); // John
alert(surname); // Smith
```

### 默认值

如果数组比左边的变量列表短，这里不会出现报错。缺少对应值的变量都会被赋 `undefined`

```javascript
let [firstName, surname] = [];

alert(firstName); // undefined
alert(surname); // undefined
```

如果我们想要一个“默认”值给未赋值的变量，我们可以使用 `=` 来提供：

```javascript
// 默认值
let [name = "Guest", surname = "Anonymous"] = ["Julius"];

alert(name); // Julius（来自数组的值）
alert(surname); // Anonymous（默认值被使用了）
```

## 对象解构

解构赋值同样适用于对象。

基本语法是:

```js
let {var1, var2} = {var1:…, var2:…}
```

变量的顺序并不重要，下面这个代码也是等价的：

```javascript
// 改变 let {...} 中元素的顺序
let { height, width, title } = { title: "Menu", height: 200, width: 100 };
```

如果我们想把一个属性赋值给另一个名字的变量，比如把 `options.width` 属性赋值给名为 `w` 的变量，那么我们可以使用冒号来设置变量名称：

```javascript
let options = {
  title: "Menu",
  width: 100,
  height: 200,
};

// { sourceProperty: targetVariable }
let { width: w, height: h, title } = options;

// width -> w
// height -> h
// title -> title

alert(title); // Menu
alert(w); // 100
alert(h); // 200
```

对于可能缺失的属性，我们可以使用 `"="` 设置默认值，如下所示：

```javascript
let options = {
  title: "Menu",
};

let { width = 100, height = 200, title } = options;

alert(title); // Menu
alert(width); // 100
alert(height); // 200
```

## 嵌套解构

```javascript
let options = {
  size: {
    width: 100,
    height: 200,
  },
  items: ["Cake", "Donut"],
  extra: true,
};

// 为了清晰起见，解构赋值语句被写成多行的形式
let {
  size: {
    // 把 size 赋值到这里
    width,
    height,
  },
  items: [item1, item2], // 把 items 赋值到这里
  title = "Menu", // 在对象中不存在（使用默认值）
} = options;

alert(title); // Menu
alert(width); // 100
alert(height); // 200
alert(item1); // Cake
alert(item2); // Donut
```

对象 `options` 的所有属性，除了 `extra` 属性在等号左侧不存在，都被赋值给了对应的变量：

![img](https://zh.javascript.info/article/destructuring-assignment/destructuring-complex.svg)

最终，我们得到了 `width、height、item1、item2` 和具有默认值的 `title` 变量。

注意，`size` 和 `items` 没有对应的变量，因为我们取的是它们的内容。
