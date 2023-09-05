# 流程控制

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [流程控制](#流程控制)
  - [语句块](#语句块)
    - [使用括号表示语句块](#使用括号表示语句块)
  - [条件语句](#条件语句)
    - [`如果` 语句](#如果-语句)
      - [多行格式](#多行格式)
      - [省略 `else` 语句](#省略-else-语句)
      - [多个条件语句组合](#多个条件语句组合)
      - [条件表达当中的局部变量](#条件表达当中的局部变量)
    - [`条件` 语句](#条件-语句)
      - [条件表达式里的模式匹配(::移动)](#条件表达式里的模式匹配移动)
      - [条件语句的局部语句](#条件语句的局部语句)
    - [`选择` 语句（::不支持，使用模式匹配替代）](#选择-语句不支持使用模式匹配替代)
  - [语句块和跳转语句](#语句块和跳转语句)
  - [循环语句](#循环语句)
    - ["重复 让" 语句](#重复-让-语句)
      - [`设有 让` 的返回值](#设有-让-的返回值)
    - ["设有 取自" 语句](#设有-取自-语句)
      - [设置过滤条件](#设置过滤条件)
      - [组合遍历多个序列](#组合遍历多个序列)
    - [指定次数循环(::TODO 删除)](#指定次数循环todo-删除)
    - [映射语句](#映射语句)
      - [映射时设置条件](#映射时设置条件)
      - [组合映射多个序列](#组合映射多个序列)
    - [序列类型当中带有循环性质的方法](#序列类型当中带有循环性质的方法)

<!-- /code_chunk_output -->

## 语句块

使用 `开始...以上`（`begin...end`） 包围起来的一条或多条语句被称之为语句块，一般语句块被用于：

1. 将一段语句打包起来作为一个整体，要么整体被执行，要么整体不执行。下面章节提到的条件语句、循环语句的各个组成部分其实都隐含者语句块。
2. 有时候某些结构只允许一行语句，但想写上多行语句，则可以使用语句块把它们打包起来。
3. 用于限制局部变量的有效范围。

语句块的最后一个语句或者表达式的值会作为语句块的返回值。

示例：

```js
let v = begin
        let i = 123
        let j = 456
    end
```

赋值语句会返回它本身的值，所以 `let j = 456` 会返回 `456`，然后因为它是语句块的最后一句，所以语句块的值也是 `456`，最后变量 `v` 接受语句块的返回值，所以它的值也是 `456`。

语句块可以嵌套，而且返回值会一层一层地往外传。

示例：

```js
let v =
    begin
        begin
            begin
                123
            end
        end
    end
```

最后变量 `v` 的值为 `123`。

### 使用括号表示语句块

可以使用一对圆括号 "(...)" 表示语句块，它跟 `开始...以上` 的作用是一样的。

示例：

```js
let v = (
        let i = 123
        let j = 456
    )
```

它的意义跟上面的第一个示例完全一样。不过括号一般用在同一行需要区分运算优先级的情况。

示例：

```js
let v = (a + b) * c
let p = (a :or b) :and c
```

而 `开始...以上` 格式一般用于多行语句。

注意如果一对括号里面的表达式或者语句之间有逗号，则该对括号表示一个元组而不是一个语句块。

示例：

```js
let v = (
    let i = 123,
    let j = 456
)
```

变量 `v` 的值将会是一个元组，其值为 `(123, 456)`。特别地，如果在括号内的末尾处有一个逗号，将会表示单独一个元素的元组。

示例：

```js
let a = (123)
let b = (123,)
```

变量 `a` 的值是一个整数 `123`，而变量 `b` 是一个只有一个成员的元组。

## 条件语句

### `如果` 语句

`如果`（`if`） 条件语句的语法：

* `如果 ... 那么 ... 否则 ...`
* `if ... then ... else ...`

语句有 3 个表达式：

1. 第一个表达式是一个值为 `逻辑`（`Boolean`） 的条件表达式；
2. 如果条件表达式的值为 true，则返回第二个表达式的值；
3. 如果条件表达式的值为 false，则返回第三个表达式的值。

示例：

```js
让 x = 90
让 a = 如果 x >= 60 那么 "及格" 否则 "不及格"
```

需要注意：

* 条件表达式只接受逻辑类型的返回值；
* `那么` 和 `否则` 两个表达式的返回值的数据类型必须一致；
* 如果忽略条件语句的返回值（即条件语句的返回值不赋值给一个变量），则对 `那么` 和 `否则` 表达式返回值的数据类型不作要求。

#### 多行格式

`如果` 语句可以写成多行格式，可以在 `那么`、`否则` 后面换行，然后最后加上 `以上` 表示语句块结束。另外，在条件表达式后面也是可以换行的。下列都是合法的多行格式：

```js
如果 ...
那么 ...
否则 ...
```

```js
如果 ...
那么
    ...
否则
    ...
以上
```

```js
如果 ... 那么
    ...
否则
    ...
以上
```

```js
if ...
then ...
else ...
```

```js
if ...
then
    ...
else
    ...
end
```

```js
if ... then
    ...
else
    ...
end
```

#### 省略 `else` 语句

如果忽略条件语句的返回值，也可以省略 `else` 语句，示例：

```js
如果 x > 90 那么
    书写行 ("优秀")
以上
```

#### 多个条件语句组合

在 `否则` 后面，也可以追加另一个 `如果` 语句，比如：

```js
让 x = 89
如果 x >= 90 那么
    书写行 ("优秀")
否则 如果 x >= 80 那么
    书写行 ("好")
否则 如果 x >= 60 那么
    书写行 ("良")
否则
    书写行 ("加油")
以上
```

```js
let x = 89
if x >= 90 then
    writeLine ("Excellent")
else if x >= 80 then
    writeLine ("Good")
else if x >= 60 then
    writeLine ("Fair")
else
    writeLine ("Poor")
end
```

#### 条件表达当中的局部变量

条件表达式可以允许一小段局部的语句，只需在表达式后面加上 `其中`（`where`） 关键字即可。

示例：

```js
let x = 89
if x >= 90 and isEven where
    let isEven = x % 2 == 0 then
    ...
end
```

多条局部语句可以使用逗号分隔。

### `条件` 语句

当有多个条件时，除了可以使用多个 `如果` 语句组合，还能使用 `条件` 语句。

`条件` 语句的语法：

```js
条件
    情况 条件表达式1: 语句1
    情况 条件表达式2: 语句2
    ...
    默认: 语句N
以上
```

```js
condition
    case expr1: statement1
    case expr2: statement2
    ...
    default: statementN
end
```

其中最后一个 `默认` 语句是可选的。

跟 `如果` 语句一样，条件语句也能返回值。示例：

```js
让 x = 70
让 s = 条件
    情况 x>=90: "优"
    情况 x>=80: "好"
    情况 x>=60: "良"
    默认: "加油"
以上
```

```js
let x = 70
let s = condition
    case x>=90: "Excellent"
    case x>=80: "Good"
    case x>=60: "Fair"
    default: "Poor"
end
```

#### 条件表达式里的模式匹配(::移动)

条件表达式除了可以是一个返回逻辑数值的表达式，还可以是一个模式匹配表达式 `让...匹配`。

示例：

```js
let x = (70, 80)
let s = condition
    case let (a,b) match x:
        `got two numbers, {a} and {b}`
    end
```

因为 `让...匹配` 表达式返回一个 `逻辑型`（`Boolean`） 数值，所以该表达式还可以进一步跟其他条件表达组合。

示例：

```js
let x = (70, 80)
let s = condition
    case let (a,b) match x :and a >= 60:
        `got two numbers, {a} and {b}`
    end
```

#### 条件语句的局部语句

有时需要在条件的 `情况`（`case`） 里使用某些计算后的值，但这些值对于条件语句之外没有用处，这时可以使用 `其中`（`where`）关键字在条件里定义及计算局部变量。示例：

```js
让 x = 70
让 s = 条件
    其中 让 isEven = x `余` 2 == 0,
         让 isOdd = x `余` 2 != 0
    情况 x >= 90 :并且 isEven: "优.偶数"
    情况 x >= 90 :并且 isOdd: "优.奇数"
    ...
以上
```

```js
let x = 70
let s = condition
    where let isEven = x `rem` 2 == 0,
         let isOdd = x `rem` 2 != 0
    case x >= 90 :并且 isEven: "Excellent.Even"
    case x >= 90 :并且 isOdd: "Excellent.Odd"
    ...
end
```

`其中` 关键字后面加上一个或多个变量的声明和赋值，多个变量之间使用逗号 "," 分隔，或者使用 `begin...end` 关键字包围起来。

`其中` 关键字也可以写在 `情况` 的后面，表示其声明的变量仅仅在当前情况下使用，比如上例可以写成：

```js
让 x = 70
让 s = 条件
    情况 x >= 90 :并且 isEven
        其中 让 isEven = x `余` 2 == 0
        :"优.偶数"
    情况 x >= 90 :并且 isOdd
        其中 让 isOdd = x `余` 2 != 0
        :"优.奇数"
    ...
以上
```

```js
let x = 70
let s = condition
    case x >= 90 :并且 isEven
        where let isEven = x `rem` 2 == 0
        : "Excellent.Even"
    case x >= 90 :并且 isOdd
        where let isOdd = x `rem` 2 != 0
        : "Excellent.Odd"
    ...
end
```

- - -
(::移动)

需要注意，`其中` 部分的代码在条件判断之前，所以如果某条件分支里既包含模式匹配表达式，又包含 `其中` 语句，那么条件语句里是可以使用 `其中` 语句定义的局部变量，但 `其中` 语句里不能使用模式表达式创建的变量。

示例：

```js
条件
    情况 让 (id, name) 匹配 User :并且
        id >= 100 :并且  # 没问题, 变量 `id` 来自 `让...匹配` 表达式
        isEven           # 没问题, 变量 `isEven` 来自 `其中` 语句
        其中 开始
            让 isEven = id % 2 == 0
            # 错误，变量 `id` 还不存在，因为 `其中` 语句
            # 在其他所有条件表达式之前执行。
        以上:
        ...
end
```

```js
condition
    case let (id, name) match User :and
        id >= 100 :and  # OK, the variable `id` comes from the `let...match` expression
        isEven          # OK, the variable `isEven` comes from the `where` statement
        where begin
            let isEven = id % 2 == 0
            # ERROR, becuase the variable `id` does not exist yet
            # this `where` statement is executed before all other
            # conditional expressions
        end:
        ...
end
```

### `选择` 语句（::不支持，使用模式匹配替代）

如果需要一个值与多个候选值比较，可以使用 `选择`（`switch`）语句。

`选择` 语句的语法：

```js
选择 variable
    情况 value1: ...
    情况 value2: ...
    情况 value3: ...
    默认: ...
以上
```

```js
switch variable
    case value1: ...
    case value2: ...
    case value3: ...
    default: ...
end
```

选择语句跟条件语句不同的地方在于，选择语句仅仅让指定的变量的值跟每个情况里的字面量或者常量进行相等比较。

`情况` 后面可以跟一个或多个字面量或者常量，只需用逗号分隔即可。

示例：

```js
让 d = 1
选择 d
    情况 1,2,3,4,5:
        书写行("工作日")
    情况 6,7:
        书写行("假日")
以上
```

```js
let d = 1
switch d
    case 1,2,3,4,5:
        writeLine("Working days")
    case 6,7:
        writeLine("Holiday")
end
```

跟 `如果`，`条件` 语句一样，`选择` 语句也是可以返回值的，示例：

```js
让 d = 2
让 s = 选择 d
    情况 1,2,3,4,5: "工作日"
    情况 6,7: "假日"
以上
书写行(s)
```

```js
let d = 2
let s = switch d
    case 1,2,3,4,5: "Working days"
    case 6,7: "Holiday"
end
writeLine(s)
```

跟其他编程语言的 `switch` 语句不一样，XiaoXuan 的 `情况` 后面不需要 `break` 关键字。

## 语句块和跳转语句

`块`（`block`） 和 `回到`（`recur`） 语句是一个运行环境内部使用的语句。

`块` 语句用于包围一组语句，跟匿名函数类似，语句块允许设置一个参数并设置其初始值，但跟匿名函数不同的是语句块无法被调用，它仅仅是用于标记一组语句。

`回到` 语句用于携带一个数值重新回到语句块的起点，语句块的参数被赋予新值，并重新执行一次。`回到` 语句后面的语句将会被忽略（也就是永远不会被执行）。

`块` 语句和 `回到` 语句是运行环境实现循环结构的方法。

`回到` 语句还能携带一组数值返回到所在函数的函数，这组数值将会成为函数的新参数，再执行一次函数主体。运行环境使用 `回到` 语句实现尾部调用优化，当函数体的最后一句是调用函数自身（或者该语句后面只有 `如果` 语句的 `以上` 语句），则该调用语句会被语法解析器替换成 `回到` 语句。

> 在用户代码里无法使用这两个语句。

`开始...以上`（`begin...end`）语句实际上是空参数的 `块` 语句的语法糖，两者作用一样，只是 `开始...以上` 是用户代码。

## 循环语句

### "重复 让" 语句

`重复 让`（`for/loop let`）是一种循环语句。

类似 Java 的 "for" 语句，Java 的 "for" 语句由一个或多个变量及其初始值、一个条件表达式、让变量变化的语句以及循环主体共 4 个部分组成，一个典型的 `循环 让` 语句则由一个变量及初始值、一个条件语句和一个让循环语句再次执行的语句共 3 个部分组成。

示例：

```js
重复 让 i = 0 如果 i < 100 那么
    ...
    书写行 (i)
    ...
    循环 i+1
以上
```

```js
for let i = 0 {
    if i < 100 then
        ...
        next i+1
    end
}
```

第二种写法：

```js
for let i = 0 if i < 100 then
    ...
    writeLine(i)
    ...
    recur i+1
end
```

上面语句的作用等同以下的 JavaScript 语句：

```JavaScript
for(let i=0; i<100; i=i+1) {
    console.log(i)
}
```

for let 后面可以有多个变量，比如：

```js
for let i=0, j=0 {
    if i < 10 then
        ...
        next i+1, j+2
    end
}
```

需要注意，for let 后面有多少个变量，则 next 后面就必须有相同数量的值。

for 表达式也有返回值，默认返回空元 `()`

```js
for let i = 0 {
    if i < 100 then
        ...
        next i+1
    else
        i
    end
}
```

在 `设有 让` 语句当中：

* `让 i = 0` 是循环的变量名称及初始值，`设有 让` 语句只能设置一个变量（如果需要多个同时变化的变量，可以使用一个有多个成员的元祖作为变量）；
* `如果 i < 100 那么 ... 以上` 是循环的条件语句（同时也是循环的主体），如果条件表达式返回 `真` 值，则执行循环主体，否则结束循环。
* `重做 i+1` 用于赋予循环变量新值，以及跳到循环开始的位置并再次执行循环的主体。
* 如果 `重做` 语句后面还有其他语句，这些代码将会被忽略。

`设有 让` 语句的原理实际上是 `块` 和 `回到` 语句的语法糖。`设有 让` 语句创建了一个有一个参数的 `块` 并赋予了初始值，而 `重做` 语句则是 `回到` 语句。

有时循环语句也可以使用匿名函数实现，如上例的等效代码如下：

```js
01  让 f = 匿名函数 (整数 i)
02      如果 i < 100 那么
03          ...
04          书写行 (i)
05          ...
06          f (i+1)
07      以上
08  以上
09
10  f (0)
```

显然，`设有 让` 语句更简洁明了。

`设有 让` 语句后面也可以换行写条件语句，上面的示例代码将第一行换行之后得：

```js
设有 让 i = 0
    如果 i < 100 那么
        ...
        书写行 (i)
        ...
        重做 i+1
    以上
以上
```

```js
for let i = a
    if i < 100 then
        ...
        writeLine(i)
        ...
        redo i+1
    end
end
```

可见 `设有 让` 语句是一个语句块和一个条件语句的结合体，其中的条件语句还能省略，不过一个典型的循环语句一般有这些部分：

1. `设有 让` 关键字；
2. 定义变量及赋予初始值；
3. 一个条件语句；
4. `重做` 关键字。

需注意：

* 要在在一定的条件下停止调用 `重做` 关键字，否则会陷入死循环。
* 如果漏写了 `重做` 关键字语句，循环体仍然会被执行一次（如果条件表达式的值为 `真` 的话）；
* `重做`（`redo`）是关键字，不是函数名称，后面直接跟着变量的新值，不需要加括号（不过即使加上括号也没错）。如果变量是元组，则需要加上括号（此时括号里面有逗号，表示元组）。
* `设有 让` 循环语句不支持 `continue` 和 `break` 等关键字。只要不调用 `重做` 则相当于 `break`，至于 `continue` 语句可以使用一个大范围的 `如果` 条件语句代替。

下面演示如果通过元组实现在循环体里使用多个变量：

```js
for let (x,y) = (0, 0) if x > 10 :and y < 100
    ...
    writeLine (y)
    ...
    redo (x+1, y+1)
end
```

#### `设有 让` 的返回值

因为 `设有 让` 循环语句是一个语句块，所以也是可以有返回值的。

示例：

```js
让 总和 =
    设有 让 (i, s) = (1, 0)
        如果 i > 100 那么
            s
        否则
            让 t = s + i
            重做 (i+1, t)
        以上
    以上
```

```js
let sum =
    for let (i, s) = (1, 0)
        if i > 100 then
            s
        else
            let t = s + i
            redo (i+1, t)
        end
    end
```

上例代码实现从整数 1 累加到 100，然后将总和值（即变量 `s` 的值）。

### "设有 取自" 语句

`设有 取自`（`for in`）用于遍历一个列表或者实现了 `序列` 特性的数据到一个指定的变量，相当于 Java 语言的 "for each" 语句。

示例：

```js
设有 i 取自 [1..10]
    书写行 (i)
以上

// 或者
设有 i 取自 [1..10] 书写行 (i) // ::不一定支持
```

```js
for i in [1..10]
    writeLine (i)
end

// or
for i in [1..10] writeLine (i) // ::不一定支持
```

`设有 取自` 语句也是 `块` 和 `返回` 语句的语法糖。原理是使用模式 `[x,...xs]` 从一个 "类列表" （即 `取自` 关键字后面的数值）获取第一个元素及余下的元素，然后让第一个元素的值作为变量执行 `设有 让` 循环体，当循环体执行完成后，判断类列表是否仍然有剩余元素，有则再次使用模式 `[x,...xs]` 取第一个元素并重复这个过程，没有则结束语句。

上面的示例代码等效下面的代码：

```js
让 c = [1..10]
块 (c)
    如果 非(是空的(c)) 那么
        让 [i, ...rest] = c
        # 循环体开始
        书写行(i)
        # 循环体结束
        返回 rest
    以上
以上
```

```js
let c = [1..10]
block (c)
    if not(isEmpty(c)) then
        let [i, ...rest] = c
        # loop body start
        writeLine(i)
        # loop body end
        recur rest
    end
end
```

需要注意的是：

* `设有 取自` 语句无返回值，如果想遍历一个列表且根据一定的关系返回值，可以使用 `映射`（`map`） 或者 `折叠`（`fold`） 等函数。
* 在 `设有 取自` 语句不支持 `continue` 和 `break` 等关键字。

#### 设置过滤条件

```js
for i in [1..10]
    filter i :mod: 2 == 0
    writeLine (i)
end

// or
for (i in [1..10]) filter (i :mod: 2 == 0) writeLine (i) // ::不一定支持
```

#### 组合遍历多个序列

示例：

```js
for i in [1..10], j in [0.1,0.2,..1.0]
    writeLine (`{i},{j}`)
end

for i in [1..10], j in [0.1,0.2,..1.0]
    filter (i :mod: 2 == 0) :and: (j >= 0.5)
    writeLine (`{i},{j}`)
end

```

### 指定次数循环(::TODO 删除)

因为循环指定次数比较常见，所以还有一种重复指定次数的循环语句 `重复` 语句。

示例：

```js
重复 10
    书写行 ("你好")
以上
```

```js
repeat 10
    writeLine ("Hello")
end
```

`重复` 关键字后面加一个自然数，表示循环的次数。在循环主体里可以使用特殊变量 `%` 获取当前循环的次数。

`重复` 语句实际上是 `设有 取自` 语句的语法糖，上面的语句相当于：

```js
设有 % 取自 [1..10]
    书写行 ("你好")
以上
```

`重复` 语句无返回值，不支持 `continue` 和 `break` 等关键字。

### 映射语句

用于遍历一个序列的每个元素，然后返回一个列表。

```js
let a = map i in [1..10]
            i * 2
        end

// or
let a = map i in [1..10] i * 2 // ::不一定支持
```

#### 映射时设置条件

```js
let a = map i in [1..10]
    filter i :mod: 2 == 0
    i * 2
end

// or
let a = map i in [1..10] filter (i :mod: 2 == 0) i* 2 // ::不一定支持
```

#### 组合映射多个序列

```js
let a = map i in [1..10], j in [0.1,0.2,..1.0]
            (i, j)
        end
```

返回 `[(1,0.1), (1,0.2), .., (2,0.1), (2,0.2), .., (10,0.9), (10,1.0)]`

```js
let a1 = map i in [1..10], j in [0.1,0.2..1.0]
            filter i :mod: 2 == 0
            (i, j)
        end

let a2 = [1..10].map(i=>
            [0.1,0.2,..1.0].map(j=>
                (i,j)
            )
        ).filter((i,j) => if i :mod: 2 == 0)
```

上面 2 句等效。

### 序列类型当中带有循环性质的方法

比如 `map`，`fold`，`zip` 等方法，详细见《集合》一章。