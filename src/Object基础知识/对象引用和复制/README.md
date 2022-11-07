对象与原始类型的根本区别之一是，对象是“通过引用”存储和复制的，而原始类型：字符串、数字、布尔值等 —— 总是“作为一个整体”复制。

让我们从原始类型开始，例如一个字符串。

这里我们将 message 复制到 phrase：

```js
let message = "Hello!";
let phrase = message;
```

结果我们就有了两个独立的变量，每个都存储着字符串 `"Hello!"`。

![img](https://zh.javascript.info/article/object-copy/variable-copy-value.svg)

但是，对象不是这样的。

**赋值了对象的变量存储的不是对象本身，而是该对象“在内存中的地址” —— 换句话说就是对该对象的“引用”。**

> 引用的概念和`c`语言中指针的概念类似, 但是引用**不是**指针

让我们看一个这样的变量的例子：

```js
let user = {
  name: "John",
};
```

![img](https://zh.javascript.info/article/object-copy/variable-contains-reference.svg)

该对象被存储在内存中的某个位置（在图片的右侧），而变量 `user`（在左侧）保存的是对其的“引用”。

我们可以将一个对象变量（例如 `user`）想象成一张写有对象的地址的纸。

当我们对对象执行操作时，例如获取一个属性 `user.name`，`JavaScript` 引擎会查看该地址中的内容，并在实际对象上执行操作。

**当一个对象变量被复制 —— 引用被复制，而该对象自身并没有被复制。**

例如:

```js
let user = { name: "John" };

let admin = user; // 复制引用
```

![img](https://zh.javascript.info/article/object-copy/variable-copy-reference.svg)

正如你所看到的，这里仍然只有一个对象，但现在有两个引用它的变量。

我们可以通过其中任意一个变量来访问该对象并修改它的内容：

```js
let user = { name: "John" };

let admin = user;

admin.name = "Pete"; // 通过 "admin" 引用来修改

alert(user.name); // 'Pete'，修改能通过 "user" 引用看到
```

## 通过引用来比较

仅当两个对象为同一对象时，两者才相等。

例如，这里 `a` 和 `b` 两个变量都引用同一个对象，所以它们相等：

```js
let a = {};
let b = a; // 复制引用

alert(a == b); // true，都引用同一对象
alert(a === b); // true
```

而这里两个独立的对象则并不相等，即使它们看起来很像（都为空）：

```js
let a = {};
let b = {}; // 两个独立的对象

alert(a == b); // false
```

## 克隆与合并

那么，拷贝一个对象变量会又创建一个对相同对象的引用。

但是，如果我们想要复制一个对象，那该怎么做呢？

我们可以创建一个新对象，通过遍历已有对象的属性，并在**原始类型值**的层面复制它们，以实现对已有对象结构的复制。

```js
let user = {
  name: "John",
  age: 30,
};

let clone = {}; // 新的空对象

// 将 user 中所有的属性拷贝到其中
for (let key in user) {
  clone[key] = user[key];
}

// 现在 clone 是带有相同内容的完全独立的对象
clone.name = "Pete"; // 改变了其中的数据

alert(user.name); // 原来的对象中的 name 属性依然是 John
```

我们也可以使用 `Object.assign` 方法来达成同样的效果。

```js
Object.assign(dest, [src1, src2, src3...])
```

- 第一个参数 dest 是指目标对象。
- 更后面的参数 src1, ..., srcN（可按需传递多个参数）是源对象。
- 该方法将所有源对象的属性拷贝到目标对象 dest 中。换句话说，从第二个开始的所有参数的属性都被拷贝到第一个参数的对象中。
- 调用结果返回 dest。

例如，我们可以用它来合并多个对象：

```js
let user = { name: "John" };

let permissions1 = { canView: true };
let permissions2 = { canEdit: true };

// 将 permissions1 和 permissions2 中的所有属性都拷贝到 user 中
Object.assign(user, permissions1, permissions2);

// 现在 user = { name: "John", canView: true, canEdit: true }
```

如果被拷贝的属性的属性名已经存在，那么它会被覆盖：

```js
let user = { name: "John" };

Object.assign(user, { name: "Pete" });

alert(user.name); // 现在 user = { name: "Pete" }
```

我们也可以用 `Object.assign` 代替 `for..in` 循环来进行简单克隆：

```js
let user = {
  name: "John",
  age: 30,
};

let clone = Object.assign({}, user);
```

它将 user 中的所有属性拷贝到了一个空对象中，并返回这个新的对象。

还有其他克隆对象的方法，例如使用 `spread` 语法 `clone = {...user}`，在后面的章节中我们会讲到。

> 以上提到到`clone`方法都是浅拷贝(只能处理单层的)

## 深层克隆

到现在为止，我们都假设 `user` 的所有属性均为原始类型。但属性可以是对其他对象的引用。

```js
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50,
  },
};

alert(user.sizes.height); // 182
```

现在这样拷贝 `clone.sizes = user.sizes` 已经不足够了，因为 `user.sizes` 是个对象，它会以引用形式被拷贝。因此 `clone` 和 `user` 会共用一个 `size`:

```js
let user = {
  name: "John",
  sizes: {
    height: 182,
    width: 50,
  },
};

let clone = Object.assign({}, user);

alert(user.sizes === clone.sizes); // true，同一个对象

// user 和 clone 分享同一个 sizes
user.sizes.width++; // 通过其中一个改变属性值
alert(clone.sizes.width); // 51，能从另外一个获取到变更后的结果
```

为了解决这个问题，并让 `user` 和 `clone` 成为两个真正独立的对象，我们应该使用一个拷贝循环来检查 `user[key]` 的每个值，如果它是一个对象，那么也复制它的结构。这就是所谓的“深拷贝”。

## 总结

- 对象通过引用被赋值和拷贝。换句话说，一个变量存储的不是“对象的值”，而是一个对值的“引用”（内存地址）。因此，拷贝此类变量或将其作为函数参数传递时，所拷贝的是引用，而不是对象本身。
- 所有通过被拷贝的引用的操作（如添加、删除属性）都作用在同一个对象上。
- 为了创建“真正的拷贝”（一个克隆），我们可以使用 `Object.assign` 来做所谓的“浅拷贝”（嵌套对象被通过引用进行拷贝）或者使用“深拷贝”函数，例如 `_.cloneDeep(obj)`。
