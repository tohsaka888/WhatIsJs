# Object.keys，values，entries

- `Object.keys(obj)` —— 返回一个包含该对象所有的键的数组。
- `Object.values(obj)` —— 返回一个包含该对象所有的值的数组。
- `Object.entries(obj)` —— 返回一个包含该对象所有 `[key, value]` 键值对的数组。

但是请注意区别（比如说跟 map 的区别）:

- 第一个区别是，对于对象我们使用的调用语法是 `Object.keys(obj)`，而不是 `obj.keys()`。
- 第二个区别是 `Object.*` 方法返回的是“真正的”数组对象，而不只是一个可迭代对象。这主要是历史原因。
