# Macro 宏

用于生成源代码的代码，宏的展开（解析执行）在编译器的 `预处理` 阶段。

宏的格式 `$name(...)`，其中的括号也可以是花括号和中括号，比如 `$name[...]` 和 `$name{...}`

## join 宏

```js
$join{

}
```