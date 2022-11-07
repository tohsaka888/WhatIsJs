## 对象方法

```javascript
let user = {
  name: "John",
  age: 30,
};

user.sayHi = function () {
  alert("Hello!");
};

user.sayHi(); // Hello!
```

这里我们使用函数表达式创建了一个函数，并将其指定给对象的 user.sayHi 属性。

随后我们像这样 `user.sayHi()` 调用它。

当然，我们也可以使用预先声明的函数作为方法，就像这样：

```javascript
let user = {
  // ...
};

// 首先，声明函数
function sayHi() {
  alert("Hello!");
}

// 然后将其作为一个方法添加
user.sayHi = sayHi;

user.sayHi(); // Hello!
```

## 方法简写

在对象字面量中，有一种更短的（声明）方法的语法：

```js
// 这些对象作用一样

user = {
  sayHi: function () {
    alert("Hello");
  },
};

// 方法简写看起来更好，对吧？
let user = {
  sayHi() {
    // 与 "sayHi: function(){...}" 一样
    alert("Hello");
  },
};
```

说实话，这种表示法还是有些不同。在对象继承方面有一些细微的差别（稍后将会介绍），但目前它们并不重要。在几乎所有的情况下，更短的语法是首选的。

## 方法中的 “this”

通常，对象方法需要访问对象中存储的信息才能完成其工作。

例如，`user.sayHi()` 中的代码可能需要用到 `user` 的 `name` 属性。

为了访问该对象，方法中可以使用 `this` 关键字。

**`this` 的值就是在点之前的这个对象，即调用该方法的对象。**

举个例子：

```js
let user = {
  name: "John",
  age: 30,

  sayHi() {
    // "this" 指的是“当前的对象”
    alert(this.name);
  },
};

user.sayHi(); // John
```

在这里 `user.sayHi()` 执行过程中，`this` 的值是 `user`。

技术上讲，也可以在不使用 `this` 的情况下，通过外部变量名来引用它：

```javascript
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert(user.name); // "user" 替代 "this"
  },
};
```

但这样的代码是不可靠的。如果我们决定将 `user` 复制给另一个变量，例如 `admin = user`，并赋另外的值给 `user` ，那么它将访问到错误的对象。

下面这个示例证实了这一点：

```javascript
let user = {
  name: "John",
  age: 30,

  sayHi() {
    alert(user.name); // 导致错误
  },
};

let admin = user;
user = null; // 重写让其更明显

admin.sayHi(); // TypeError: Cannot read property 'name' of null
```

## “this” 不受限制

在 `JavaScript` 中，`this` 关键字与其他大多数编程语言中的不同。`JavaScript` 中的 `this` 可以用于任何函数，即使它不是对象的方法。

下面这样的代码没有语法错误：

```javascript
let user = { name: "John" };
let admin = { name: "Admin" };

function sayHi() {
  alert(this.name);
}

// 在两个对象中使用相同的函数
user.f = sayHi;
admin.f = sayHi;

// 这两个调用有不同的 this 值
// 函数内部的 "this" 是“点符号前面”的那个对象
user.f(); // John（this == user）
admin.f(); // Admin（this == admin）

admin["f"](); // Admin（使用点符号或方括号语法来访问这个方法，都没有关系。）
```

这个规则很简单：如果 `obj.f()` 被调用了，则 `this` 在 `f` 函数调用期间是 `obj`。所以在上面的例子中 `this` 先是 `user`，之后是 `admin`。

## 箭头函数没有自己的 “this”

箭头函数有些特别：它们没有自己的 `this`。如果我们在这样的函数中引用 `this`，`this` 值取决于外部“正常的”函数。

举个例子，这里的 `arrow()` 使用的 `this` 来自于外部的 `user.sayHi()` 方法：

```javascript
let user = {
  firstName: "Ilya",
  sayHi() {
    let arrow = () => alert(this.firstName);
    arrow();
  },
};

user.sayHi(); // Ilya
```

> 这是箭头函数的一个特性，当我们并不想要一个独立的 this，反而想从外部上下文中获取时，它很有用。

## 总结

- 一个函数在声明时，可能就使用了 this，但是这个 this 只有在函数被调用时才会有值。
- 可以在对象之间复制函数。
- 以“方法”的语法调用函数时：`object.method()`，调用过程中的 `this` 值是 `object` (调用它的那个对象)。

请注意箭头函数有些特别：它们没有 `this`。在箭头函数内部访问到的 `this` 都是从外部获取的。

## 任务

### 在对象字面量中使用`this`

这里 `makeUser` 函数返回了一个对象。

访问 `ref` 的结果是什么？为什么？

```javascript
function makeUser() {
  return {
    name: "John",
    ref: this,
  };
}

let user = makeUser();

alert(user.ref.name); // 结果是什么？
```

结果是: 报错。

理由很简单: `let user = makeUser()`因为没有任何对象使用了`makeUser`方法,所以`makeUser`内`this`为`undefined`, 即`user.ref`为`undefined`。

`undefined.name`结果就报错了!

### 链式调用

有一个可以上下移动的 `ladder` 对象：

```js
let ladder = {
  step: 0,
  up() {
    this.step++;
  },
  down() {
    this.step--;
  },
  showStep: function () {
    // 显示当前的 step
    alert(this.step);
  },
};
```

现在，如果我们要按顺序执行几次调用，可以这样做：

```javascript
ladder.up();
ladder.up();
ladder.down();
ladder.showStep(); // 1
ladder.down();
ladder.showStep(); // 0
```

解决方案:

```javascript
let ladder = {
  step: 0,
  up() {
    this.step++;
    return this;
  },
  down() {
    this.step--;
    return this;
  },
  showStep: function () {
    // 显示当前的 step
    console.log(this.step);
    return this;
  },
};

ladder.up().up().down().showStep().down().showStep(); // 展示 1，然后 0

console.log(ladder);
```
