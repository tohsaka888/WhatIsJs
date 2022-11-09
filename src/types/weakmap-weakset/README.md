# WeakMap and WeakSet（弱映射和弱集合）

我们从前面的 [垃圾回收](/src/object/garbage-collection/) 章节中知道，`JavaScript` 引擎在值“可达”和可能被使用时会将其保持在内存中。

```javascript
let john = { name: "John" };

// 该对象能被访问，john 是它的引用

// 覆盖引用
john = null;

// 该对象将会被从内存中清除
```

通常，当对象、数组之类的数据结构在内存中时，它们的子元素，如对象的属性、数组的元素都被认为是可达的。

例如，如果把一个对象放入到数组中，那么只要这个数组存在，那么这个对象也就存在，即使没有其他对该对象的引用。

就像这样:

```javascript
let john = { name: "John" };

let array = [john]; // 数组存储了john的引用

john = null; // 覆盖引用

// 前面由 john 所引用的那个对象被存储在了 array 中
// 所以它不会被垃圾回收机制回收
// 我们可以通过 array[0] 获取到它
```

> 注: 数组存储对象存储的是对象的引用

```js
let a = { val: 1 }; // a是{val: 1}这个对象的引用
let arr = [a]; // 可以理解为存储的是{val : 1}这个对象的引用而不是存储了a

a.val = 2;
console.log(arr); // [{a: 2}]
```

但是当我们覆盖了这个`a`引用

```javascript
let a = { val: 1 }; // a是{val: 1}这个对象的引用
let arr = [a]; // 可以理解为存储的是{val : 1}这个对象的引用而不是存储了a

a = { b: 2 }; // 这时a->{ b: 2 }
console.log(arr, a); // [{a: 1}] { b: 2 }
```

## WeakMap

`WeakMap` 和 `Map` 的第一个不同点就是，`WeakMap` 的键**必须是对象**，不能是原始值：

```js
let weakMap = new WeakMap();

let obj = {};

weakMap.set(obj, "ok"); // 正常工作（以对象作为键）

// 不能使用字符串作为键
weakMap.set("test", "Whoops"); // Error，因为 "test" 不是一个对象
```

现在，如果我们在 `weakMap` 中使用一个对象作为键，并且没有其他对这个对象的引用 —— 该对象将会被从内存（和 map）中自动清除。

```javascript
let john = { name: "John" };

let weakMap = new WeakMap();
weakMap.set(john, "...");

john = null; // 覆盖引用

// john 被从内存中删除了！
```

与上面常规的 `Map` 的例子相比，现在如果 `john` 仅仅是作为 `WeakMap` 的键而存在 —— 它将会被从 `map`（和内存）中自动删除。

`WeakMap` 不支持迭代以及 keys()，values() 和 entries() 方法。所以没有办法获取 `WeakMap` 的所有键或值。

## WeakSet

`WeakSet` 的表现类似：

- 与 `Set` 类似，但是我们只能向 `WeakSet` 添加**对象**（而不能是原始值）。
- 对象只有在其它某个（些）地方能被访问的时候，才能留在 `WeakSet` 中。
- 跟 `Set` 一样，`WeakSet` 支持 add，has 和 delete 方法，但不支持 `size` 和 `keys()`，并且不可迭代。
