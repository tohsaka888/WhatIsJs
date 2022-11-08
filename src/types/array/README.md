# 数组

## 使用 “at” 获取最后一个元素

假设我们想要数组的最后一个元素。

一些编程语言允许我们使用负数索引来实现这一点，例如 `fruits[-1]`。

但在 `JavaScript` 中这行不通。结果将是 `undefined`，因为方括号中的索引是被按照其字面意思处理的。

我们可以显式地计算最后一个元素的索引，然后访问它：`fruits[fruits.length - 1]`。

```javascript
let fruits = ["Apple", "Orange", "Plum"];

alert(fruits[fruits.length - 1]); // Plum
```

有点麻烦，不是吗？我们需要写两次变量名。

幸运的是，这里有一个更简短的语法 `fruits.at(-1)`：

```javascript
let fruits = ["Apple", "Orange", "Plum"];

// 与 fruits[fruits.length-1] 相同
alert(fruits.at(-1)); // Plum
```

换句话说，`arr.at(i)`：

如果 `i >= 0`，则与 `arr[i]` 完全相同。
对于 `i` 为负数的情况，它则从**数组的尾部向前数**。

## `pop/push`, `shift/unshift` 方法

`队列（queue）`是最常见的使用数组的方法之一。在计算机科学中，这表示支持两个操作的一个有序元素的集合：

- `push` 在末端添加一个元素.
- `shift` 取出队列首端的一个元素，整个队列往前移，这样原先排第二的元素现在排在了第一。

![img](https://zh.javascript.info/article/array/queue.svg)

这两种操作数组都支持。

队列的应用在实践中经常会碰到。例如需要在屏幕上显示消息队列。

数组还有另一个用例，就是数据结构 栈。

它支持两种操作：

`push` 在末端添加一个元素.
`pop` 从末端取出一个元素.

所以新元素的添加和取出都是从“末端”开始的。

栈通常被被形容成一叠卡片：要么在最上面添加卡片，要么从最上面拿走卡片：

![img](https://zh.javascript.info/article/array/stack.svg)

对于栈来说，最后放进去的内容是最先接收的，也叫做 `LIFO（Last-In-First-Out）`，即后进先出法则。而与队列相对应的叫做 `FIFO（First-In-First-Out）`，即先进先出。

`JavaScript` 中的数组既可以用作队列，也可以用作栈。它们允许你从首端/末端来添加/删除元素。

这在计算机科学中，允许这样的操作的数据结构被称为 `双端队列（deque）`。

## 不要使用 == 比较数组

`JavaScript` 中的数组与其它一些编程语言的不同，不应该使用 `==` 运算符比较 `JavaScript` 中的数组。

该运算符不会对数组进行特殊处理，它会像处理任意对象那样处理数组。

让我们回顾一下规则：

- 仅当两个对象引用的是同一个对象时，它们才相等 ==。
- 如果 `==` 左右两个参数之中有一个参数是对象，另一个参数是原始类型，那么该对象将会被转换为原始类型，转换规则如 [对象—原始值转换](/src/object/object-toprimitive/) 一章所述。
- `null` 和 `undefined` 相等 `==`，且各自不等于任何其他的值。

严格比较 `===` 更简单，因为它不会进行类型转换。

所以，如果我们使用 `==` 来比较数组，除非我们比较的是两个引用同一数组的变量，否则它们永远不相等。

> 数组本质上也是 Object

```js
alert([] == []); // false
alert([0] == [0]); // false
```

从技术上讲，这些数组是不同的对象。所以它们不相等。`==` 运算符不会进行逐项比较。

与原始类型的比较也可能会产生看似很奇怪的结果：

```javascript
alert(0 == []); // true

alert("0" == []); // false
```

在这里的两个例子中，我们将原始类型和数组对象进行比较。因此，数组 `[]` 被转换为原始类型以进行比较，被转换成了一个空字符串 `''`。

## 任务

### 数组被拷贝了吗?

```javascript
let fruits = ["Apples", "Pear", "Orange"];

// 在“副本”里 push 了一个新的值
let shoppingCart = fruits;
shoppingCart.push("Banana");

// fruits 里面是什么？
alert(fruits.length); // ?
```

结果是`4`:

```js
let fruits = ["Apples", "Pear", "Orange"];

let shoppingCart = fruits;

shoppingCart.push("Banana");

alert(fruits.length); // 4
```

这是因为**数组是对象**。所以 `shoppingCart` 和 `fruits` 是同一数组的引用。
