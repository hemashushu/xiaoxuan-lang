# 函数

## 函数的调用

* 调用函数的语法

  `函数名称 (参数值1, 参数值2, ... , 参数值N)`
  `functionName (value1, value2, ... , valueN)`

  示例：

  `加 (1, 2)`
  `行输出 ("你好")`

  `add (1, 2)`
  `writeLine ("Hello")`

* 调用函数并获取返回值

  `让 变量名 = 函数名称 (参数值1, 参数值2, ..., 参数值N)`
  `let variableName = functionName (value1, value2, ..., valueN)`

  示例：

  `让 a = 平方根 (4)`
  `让 b = 加 (1, 2)`

  `let a = sqrt (4)`
  `let b = add (1, 2)`

* 按参数名称调用函数

  一般调用函数时，需要把各个参数值按顺序（位置）传入函数，当一个函数的参数较多，或者包含有可省参数时，则可以考虑使用按参数名称传入参数值：

  `函数名称 (参数名称1=参数值1, 参数名称2=参数值2, ..., 参数名称N=参数值N)`
  `functionName (param1=value1, param2=value2, ..., paramN=valueN)

  可以混合按参数位置和按参数名称两种传参方式，不过必须先传完所有的位置参数值，然后才可以开始传名称参数值：

  `函数名称 (参数值1, ..., 参数值N, 参数名称a=参数值a, ..., 参数名称z=参数值z)`
  `functionName (value1, ..., valueN, paramA=valueA, ..., paramZ=参数值Z)`

  示例：

  `绘图.直线 (x1=0, y1=0, x2=100, y2=100, 颜色=颜色常数.红色, 线条宽度=3)`
  `绘图.直线 (0, 0, 100, 100, 颜色=颜色常数.红色, 线条宽度=3)`

  `draw.line (x1=0, y1=0, x2=100, y2=100, color=ColorConstant.Red, lineWidth=3)`
  `draw.line (0, 0, 100, 100, color=ColorConstant.Red, lineWidth=3)`

* 调用一个没有参数的函数（即过程）时，需要在函数名称后面加上空元（即一对空括号）：

  `函数名称 ()`
  `functionName ()`

* 函数也可以嵌套调用，即：将一个函数的返回值作为另一个函数的参数值。严格来说任何有返回值的表达式或者语句块，都可以作为参数值传给一个函数：

  `函数1 (参数值1, 函数2 (参数值a, 参数值z), 任意有返回值的表达式, ...)`
  `function1 (value1, function2 (valueA, valueZ), any_expression, ...)`

  示例：

  `让 弦 = 平方根 (平方 (3), 平方 (4))`
  `让 弦 = 平方根 (3 ^ 2 + 4 ^ 2)`

  `let dist = sqrt (square (3), square (4))`
  `let dist = sqrt (3 ^ 2 + 4 ^ 2)`

  * 如果要避免多层函数嵌套调用，可以先把表达式的值求出来并赋值给变量，然后再传入函数。
  * 在语法上，允许省略函数名称和参数列表之间的空格，允许省略参数值之间的空格，不过为了规范起见尽量保留这些空格。

* 参数列表换行

  当参数列表过长时，可以换行书写，但参数列表的第一个括号（即开始括号）必须保留在函数名称后面，示例：

  ```
  01  让 标准差 =
  02      平方根 (
  03          加 (
  04              (x1-μ)^2,
  05              (x2-μ)^2
  06          )
  07      )
  ```

  ```
  01  let std_deviation =
  02      sqrt (
  03          add (
  04              (x1-μ)^2,
  05              (x2-μ)^2
  06          )
  07      )
  ```

  注意上面示例的第 02 和 03 行的末尾的左括号 "(" 必须跟在函数名称后面。

### 特性当中的函数的点号调用方式

如果某个数据类型（包括结构体和联合体）实现了某个特性（trait），则可以在这个数据类型的实例上使用点号（"."）来调用类当中的函数。

比如有一个 "可显示"（"Display"）特性，其中有一个 "转为字符串"（"toString"） 的函数。现有结构体 "用户" 实现了这个特性，则有两种方式调用 "转为字符串" 函数：

```
01  让 a = 可显示<用户>.转为字符串 (用户甲)
02  让 b = 用户甲.转为字符串 ()
```

```
01  let a = Display<User>.toString (userA)
02  let b = userA.toString ()
```

上面代码的 01 行是标准的函数调用方式，02 行是使用了特性函数点号的调用方式。实际上点号调用方式是一个语法糖，即：

`数据实例.函数名 (参数值1, 参数值2, ... 参数值N)`
`dataInstance.functionName (value1, value2, ... valueN)`

是下面语句的语法糖：

`特性名称.函数名 (数据实例, 参数值1, 参数值2, ... 参数值N)`
`TraitName.functionName (dataInstance, value1, value2, ... valueN)`

需注意的是仅当类函数的第一个参数为数据实例的类型时才能使用点号调用方式。

第二种比较接近一般面向对象编程语言的对象实例的方法（method）调用风格和习惯，例如：

```
// Java
User userA = ...;
int id = userA.id;  // 访问对象的属性
String text = userA.toString() // 访问对象的方法

// XiaoXuan
let userA = ...
let id = userA.id  // 访问结构体的成员
let text = userA.toString() // 调用类 "Display" 的 "toString()" 方法
```

在可以的情况下推荐使用第二种调用方式。

### 函数的链式调用方式

如果一个函数返回值的数值类型符合另一个函数的第 1 个参数的数据类型，则可以将 2 个函数的调用使用 "->" 符号连续调用。比如有如下 3 个函数：

```
函数 整数 加 (整数 i, 整数 i) = ...
函数 整数 平方 (整数 i) = ...
函数 显示 (整数 i) = ...
```

```
function Int add (Int i, Int i) = ...
function Int square (Int i) = ...
function show (Int i) = ...
```

现在想 "求 (3 + 4) 的平方，并把结果输出到标准输出"，一般的函数调用方式如下：

```
让 i = 加 (3, 4)
让 j = 平方 (i)
显示 (j)
```

```
let i = add (3, 4)
let j = square (i)
show (j)
```

或者写成一行：

`显示 (平方 (加 (3, 4)))`
`show (square (add (3, 4)))`

如果使用链式调用，则代码可以写成：

`加 (3, 4) -> 平方 () -> 显示 ()`
`add (3, 4) -> square () -> show ()`

使用链式调用函数比 "多行独立调用" 或者 "嵌套调用" 的方式更加直观。另外，如果链式调用当中后面的函数只有 1 个参数，则可以省略参数列表的括号，即上例可以进一步简化为：

`加 (3, 4) -> 平方 -> 显示`
`add (3, 4) -> square -> show`

链式调用的结合方向是从左到右的。

### 函数的组合

如果一个函数的返回值的数据类型符合另一个函数的第一个参数（有且只有一个参数）的数据类型，则可以使用函数组合符号（`&`）将两个或多个函数组合起来形成一个新的函数，例如：

`让 a = 负 (平方根 (加 (2, 3)))`
`let a = neg (sqrt (add (2, 3)))`

可以写成：

```
让 我的组合函数 = 负 & 平方根 & 加
让 a = 我的组合函数 (2, 3)
```

```
let myCombFunc = neg & sqrt & add
let a = myCombFunc (2, 3)
```

当然也可以把函数的组合、函数的调用一次过进行：

`让 a = 负 & 平方根 & 加 (2, 3)`
`let a = neg & sqrt & add (2, 3)`

函数组合的结合方向是从右到左的，即先从右侧的函数开始计算，然依次计算到最左侧的函数。

函数的组合只能用于只有一个参数的函数，如果一个函数有多个参数，可以尝试使用部分调用（需带有柯里化的函数才行）后再组合，比如：

```
让 加十分 = 加 (10)  // "加 (10)" 的返回值是一个部分函数： fn (b) = 10 + b
让 我的组合函数 = 平方根 & 加十分 & 平方
让 a = 我的组合函数 (60)
```

```
let addTen = add (10)  // "add (10)" return a partial function： fn (b) = 10 + b
let myCombFunc = sqrt & addTen & square
让 a = myCombFunc (60)
```

也可以写成一行：

`让 a = 平方根 & 加 (10) & 平方 (30)`
`let a = sqrt & add (10) & square (30)`

### 使用 `调用` 函数调用函数（未定）

使用 `调用` 函数调用一个函数的格式是：`调用 (函数名, 参数元组)`

示例：

```
// 函数的一般调用方式
让 s1 = 测试 (123, "Hello")

// 使用 `调用` 语句来调用
让 args = (123, "Hello")
让 s2 = 调用 (测试, args)
```

```
// normal call
lets1 = test (123, "Hello")

// call by 'call' statement
let args = (123, "Hello")
let s2 = call (test, args)
```

每个函数内部都有一个名为 "_参数值表_"（"_arguments_"） 的元组，其中记录者该函数被调用时所传入的所有参数值。

```
函数 浮点数 测试 (整数 a, 整数 b)
  让 和 = 调用 (加, _参数值表_)
  让 差 = 调用 (减, _参数值表_)
  平方根 (和^2 + 差^2)
以上
```

```
function Float test (Int a, Int b)
  let a = call (add, _arguments_)
  let b = call (sub, _arguments_)
  sqrt (a^2 + b^2)
end
```

### 函数的中置调用方式

如果一个函数只有两个参数，则在调用时可以以中置格式调用，方法是使用反单引号（"`"）将函数名包围起来，然后将两个参数分别写在函数的左右两边。比如函数：

`让 a = 加 (11, 55)`
`let a = add (11, 55)`

可以写成：

```
让 a = 11 `加` 55
```

```
let a = 11 `add` 55
```

所有名称由**纯符号**构成的函数都使用中置格式调用，如函数 "+" 和 "*" （函数 "加" 和 "乘" 的别名）等等。

示例，下面 3 句的作用完全相同：

```
让 a = 加 (11, 55)
让 b = 11 `加` 55
让 c = 11 + 55
```

```
let a = add (11, 55)
let b = 11 `add` 55
let c = 11 + 55
```

XiaoXuan 语言里没有运算符，所有运算符实际上都是函数的别名。

### 优先级和结合性

当**多个中置函数连续调用**时，不同的中置函数可以有不同的优先级别（即执行的先后顺序），相同优先级别的又有 "从左到右" 和 "从右到左" 两种不同的结合方向。优先级和结合方向是由定义函数时定义的。

默认情况下，所以中置函数的优先级别一样，且都遵循 "从左到右" 的结合方向。比如：

`让 a = 1 + 8 - 3`
`let a = 1 + 8 - 3`

在求值时，首先会执行表达式 `1 + 8`，得出值 `9` 之后再执行表达式 `9 - 3`，最后得出结果 `6`。

有些函数有较高的优先级，比如：

`让 b = 1 + 2 * 3`
`let b = 1 + 2 * 3`

因为 `*` 函数定义的优先级别比 `+` 的要高，所以会先执行表达式 `2 * 3`，得出值 `6` 之后再执行表达式 `1 + 6`，最后得出结果为 `7`。

从右到左的结合方向的函数比较少，常见的有幂运算函数 `^`，比如：

`让 c = 4 ^ 3 ^ 2`
`let c = 4 ^ 3 ^ 2`

它会先执行表达式 `3 ^ 2`，得出值 `9` 之后再执行表达式 `4 ^ 9`。

### 内置的中置函数的优先级

部分内置的中置函数的优先级（从高到低排列）如下：

1. 幂运算：`^`（`幂`，`pow`），结合顺序为从右到左；
2. 算术运算乘除余：`*`（`乘`，`mul`）、`/`（`除`，`div`），`余`（`rem`）；
3. 算术运算加减：`+`（`加`，`add`）、`-`（`减`，`sub`)；
4. 位运算移位：`左移`（`lshift`）、`右移`（`rshift`）、`逻辑右移`（`lrshift`）；
5. 位运算与或：`位与`（`bit_and`）、`位或`（`bit_or`）。这个跟其他语言（如 C、Java）有所不同，其他语言通常位运算的优先级在相等比较之后。因为 XiaoXuan 语言的逻辑值不能转换为数字类型，且只能进行逻辑运算，又因为大小比较和相等比较运算的结果是逻辑值，所以 "位运算与或" 放在这两种运算之后则变得无意义，所以 XiaoXuan 把它的优先级调高；
6. 大小比较运算，集合元素查找：`>`（`大于`，`gt`）、`>=`（`大于等于`，`gte`）、`<`（`小于`，`lt`）、`<=`（`小于等于`，`lte`）、`存在于`（`in`）、`含有`（`contains`）；
7. 相等比较运算：`==`（`等于`，`eq`）、`!=`（`不等于`，`neq`）；
8. 逻辑与：`&&`（`并且`，`and`）
9. 逻辑或：`||`（`或者`，`or`）

参考: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence

XiaoXuan 里没有：`负` 运算符 `-`、位运算 `位反` 运算符 `~` 以及逻辑 `非` 运算符 `!`，只有相应的函数 `负`（`neg`），`位反`（`bit_inv`）和 `非`（`not`） 函数，示例：

```
// Java
int a = - (3 + 4);
bool b = !(true && false);
int c = ~0xff00aabb

// XiaoXuan
let a = neg (3 + 4)
let b = not (true && false)
let c = bit_inv (0xff00aabb)
```

### 表达式的执行顺序

表达式的执行顺序（优先级从高到低排列）为：

1. 分组及语句块，即一对括号包围起来的部分；
2. 成员访问（即点号）、列表的索引访问（即中括号加数字下标）、映射表项目访问（即点号、或者中括号加字符串），如 `user.name`，`books[1]`，`user.addr.city`，`books[1][2]`，结合顺序为从左到右。其中成员访问包括了：类函数、结构体成员、联合体成员、枚举成员、映射表项目等访问，父模块和子模块名称之间的点号也算式成员访问；
3. 函数的一般调用；
4. 类函数的点号调用（`.`）；
5. Result 和 Option 结构的解构（`?`）及默认值表达式；
6. 函数的组合（`&`），结合顺序为从右到左；；
7. 函数的中置格式调用；
8. 函数的链式调用（`->`）；
9. 赋值语句（`=`），结合顺序为从右到左；。

需要注意的是函数的一般调用方式要比函数的中置调用方式优先级要高，示例：

`让 a = 平方根 (9) + 平方根 (16)`
`let a = sqrt (9) + sqrt (16)`

在执行时会先计算两个 `平方根` 函数，得出值之后再计算 `+` 函数。

## 函数的定义

* 定义函数的语法

  使用 `函数`（`function`） 关键字定义一个函数：

  ```
  函数 返回值的数据类型 函数名称 (参数类型1 参数名称1, ..., 参数类型N 参数名称N)
      ...
      函数主体
      ...
  以上
  ```

  ```
  function DataType functionName (DataType1 param1, ..., DataTypeN paramN)
      ...
      function body
      ...
  end
  ```

  函数的主体是一个或多个语句或表达式，最后一个表达式的值将会作为函数的返回值。示例：

  ```
  函数 浮点数 两点距离 (整数 x1, 整数 y1, 整数 x2, 整数 y2)
      让 a = (x2 - x1) ^ 2
      让 b = (y2 - y1) ^ 2
      平方根 (a + b)
  以上
  ```

  ```
  function Float distance (Float x1, Float y1, Float x2, Float y2)
      let a = (x2 - x1) ^ 2
      let b = (y2 - y1) ^ 2
      sqrt (a + b)
  end
  ```

  最后一行的 `平方根` 函数的结果将会作为函数的返回值。当然也可以加上 `return` 关键字，在函数主体最后一行的 `return` 关键字实际上在运行时会直接忽略。

* 省略返回值数据类型

  定义函数时，如果省略了返回值的数据类型：

  * 普通定义的函数会自动认为返回值为 `Result<Unit>` 即 `Void`，`return` 关键字后面不加任何值会被认为是 `return void`。
  * 匿名函数会根据上下文环境自动推断，先查找外部所需要的数据类型，如果无从推导则由返回值来决定。

* 如果函数的主体是一个表达式，则可以把函数的定义写成一行：

  `函数 返回值的数据类型 函数名称 (参数类型1 参数名称1, ..., 参数类型n 参数名称n) = 表达式`
  `function DataType functionName (DataType1 param1, ..., DataTypeN paramN) = expression`

  示例：

  `函数 整数 加十分 (整数 a) = a + 10`
  `function Int addTen (Int a) = a + 10`

  表达式不一定要求在一行内写完，只要是一个合法的表达式均可， 比较长的表达式也是可以换行的，示例：

  ```
  函数 浮点数 距离 (整数 x1, 整数 y1, 整数 x2, 整数 y2) =
      平方根 (
          (x2 - x1) ^ 2 +
          (y2 - y1) ^ 2)
  ```

  ```
  function Float distance (Int x1, Int y1, Int x2, Int y2) =
      sqrt (
          (x2 - x1) ^ 2 +
          (y2 - y1) ^ 2)
  ```

  上例当中的 `=` 符号、`(` 符号， `+` 符号后面均表示语句尚未结束。

* XiaoXuan 不支持变长参数

* XiaoXuan 不支持指定参数默认值，不支持可省参数

### 函数别名

可以为一个函数设置另一个名称，使用 `别名`（`alias`） 关键字定义函数的别名，语法如下：

`别名 新函数名称 = 原函数名称`
`alias newName = originalFunctionName`

示例：

`别名 "+" = 加`
`alias "+" = add`

如果函数名称是包含有符号，需要将函数名称使用双引号包围起来。

* 别名对一个函数的所有重载均起效。即别名只跟函数的名称有关，跟函数的签名无关；
* 一般别名的定义跟原函数的定义在同一个作用域里。比如在某个模块里定义的函数，其别名也应该在这个模块里定义；在某个类里定义的函数，其别名也应该在该类里定义。

### 匿名函数

使用 `匿名函数`（`fn`） 关键字可以定义一个匿名函数，匿名函数没有名称，通常在一个函数内部定义及使用，或者作为参数传给另一个（高阶）函数。

定义匿名函数的语法：

```
让 变量名称 =
    匿名函数 返回值的数据类型 (参数类型1 参数名称1, ..., 参数类型N 参数名称N)
        ...
        函数主体
        ...
    以上
```

```
let variable =
    fn DataType (DataType1 param1, ..., DataTypeN paramN)
        ...
        function body
        ...
    end
```

跟标准函数的定义一样，如果函数的主体是一个表达式，则可以把匿名函数的定义写成一行：

`让 变量名称 = 匿名函数 返回值的数据类型 (参数类型1 参数名称1, ..., 参数类型N 参数名称N) = 表达式`
`let variable = fn DataType (DataType1 param1, ..., DataTypeN paramN) = expression`

示例：

```
让 乘加 = 匿名函数 整数 (整数 a, 整数 x, 整数 b) = a * x + b
让 a = 乘加 (2, 3, 4)
```

```
let mulAdd = fn Int (Int a, Int x, Int b) = a * x + b
let a = mulAdd (2, 3, 4)
```

有些编程语言（如 JavaScript）支持定义和调用同时进行，但 XiaoXuan 不支持这种语法，即上面的例子不能写成：

`让 a = (匿名函数 整数 (整数 a) = a * 2) (4)`
`let a = (fn Int (Int a) = a * 2) (4)`

如果一个匿名函数作为参数传给另一个函数，且这个参数的函数签名是确定的，即类型推导能推导出匿名函数的返回值的数据类型及各个参数的数据类型，则可以省略书写返回值的数据类型以及各参数的数据类型。

示例：

```
function pick(List<Int> items, F f) where
    F = Boolean <= (Int)
    ...
end

let numbers = [1, 2, 3, 4, 5]
let fav = pick(numbers, fn (i) = i > 3 )
```

上例中的 `fn (i) = i > 3` 匿名函数省略了返回值数据类型，也省略了参数的数据类型。

### 匿名函数捕获变量值（闭包）

如果一个匿名函数的主体引用了匿名函数之外的变量，则变量的值会被捕获，即使匿名函数离开了其被定义的作用域，该被捕获的值仍然有效。示例：

```XiaoXuan
函数 F 增长(整数 i) 其中
    F = 整数 <= (整数)

    返回 匿名函数 (整数 x) = i + x
以上

让 f1 = 增长 (10)  // 函数 f1 绑定了值为 10 的变量 i
让 f2 = 增长 (20)  // 函数 f2 绑定了值为 20 的变量 i

输出行 (f1 (2)) // 输出 12
输出行 (f2 (2)) // 输出 22
```

```XiaoXuan
function F inc(Int i) where
    F = Int <= (Int)

    return fn (x) i + x
end

let f1 = inc (10)  // `f1` binds the variable i with value 10
let f2 = inc (20)  // `f2` binds the variable i with value 10

writeLine (f1 (2)) // output 12
writeLine (f2 (2)) // output 22
```

在支持可变变量的编程语言里，f1 和 f2 里引用了一个共同的变量 i，假如变量 i 的值（或者 i 的成员的值）被 f1 改变了，则该改变也会反映到 f2 所绑定的变量 i 里，有时这种改变并不是预想的，因此引起错误。示例：

```JavaScript
function inc() {
    let fns = [];
    let step = 0;
    for(let i=0; i<10; i++) {
        step +=1;
        let fn = x => x + step;
        fns.push(fn);
    }

    // fns 里包含 10 个匿名函数，其绑定的 step 的值都是 10，
    // 而不是 1 到 10。

    for(let fn of fns) {
        console.log(fn(5)); // 都输出 15
    }

    // 下面是修正版
    let ffs = [];
    for(let i=0; i<10; i++) {
        let step = i; // 复制 i 值到变量 step
        let fn = x => x + i;
        ffs.push(fn);
    }

    for(let ff of ffs) {
        console.log(ff(5)); // 依次输出 5,6,7,8, ..., 15
    }
}
```

在 JavaScript 环境里，当匿名函数使用自身之外的变量时，变量（不是变量的值，而是变量本身，或者说标识符本身）会被该匿名函数所捕获，如果匿名函数被返回并被传递到其他地方，该捕获仍然存在，即该匿名函数能够一直访问到该变量的最新的值。

示例：

```JavaScript
function makeCounter() {
    let n = 1;
    return [
        () => n = n + 1,
        () => n = n - 1,
        () => n
    ]
}

// makeCounter 函数返回一个数组，数组有三个匿名函数，
// 一个递增，一个递减，一个返回当前值

let [inc, dec, get] = makeCounter();
inc();  // 返回 2
inc();  // 返回 3
get();  // 返回 3
dec();  // 返回 2
dec();  // 返回 1
```

在 JavaScript 环境里，每个函数被执行时都有两个指针，一个指向父词法环境（即调用者 caller 的词法环境） ，一个指向当前的词法环境（即 callee 自己的词法环境），匿名函数也不例外。当匿名函数被当作返回值跳出所在的函数时，函数尚未被执行，所以没有自己的词法环境。但因为匿名函数有对其所在的环境的引用（即存在上述的第一个指针），所以所在的函数的词法环境不会被回收，会一直被保持者，直到匿名函数被回收为止。

参考《javascript info》的 "作用域与闭包" 一章:
https://zh.javascript.info/closure#step-4-fan-hui-han-shu

XiaoXuan 的变量值是不可变的，因此在匿名函数里引用的外部变量时，仅仅引用了一份该变量的值（对于复制类型的值，则是复制），匿名函数被当作返回值跳出所在的函数之后，如果所在的函数已经执行完毕，则它的词法环境可以放心被回收，XiaoXuan 的垃圾回收效率会更高一些。

### 简化版匿名函数

如前两节所说，如果一个匿名函数在一个函数签名确定的上下文环境里（比如匿名函数作为一个参数传给另一个函数），那么匿名函数可以省略书写返回值数据类型以及参数的数据类型。XiaoXuan 还支持另一种更简化的形式：连参数列表也省略。语法是可以使用 `执行`（`do`）关键字定义，然后在匿名函数函数体里使用 `%1`, `%2`, `%3`, ... 来访问各个参数值，其中第一个参数值还能使用不带编号的 `%` 符号访问。

示例：

```
// 假设函数签名为： Int <= (Int)
匿名函数 Int (Int x) = x + 10 // 标准版匿名函数
匿名函数 (x) = x + 10         // 简化版匿名函数
执行 % + 10               // `执行` 版匿名函数

// 假设函数签名为：Int <= (Int, Int)
匿名函数 Int (Int x, Int y) = sqrt(x^2 + y^2) // 标准版匿名函数
匿名函数 (x, y) = sqrt(x^2 + y^2)             // 简化版匿名函数
执行 sqrt(%1^2 + %2^2)                    // `执行` 版匿名函数
```

```
// function signature: Int <= (Int)
fn Int (Int x) = x + 10 // standard anonymous function
fn (x) = x + 10         // simplified anonymous function
do % + 10               // `do` version of anonymous function

// function signature：Int <= (Int, Int)
fn Int (Int x, Int y) = sqrt(x^2 + y^2) // standard anonymous function
fn (x, y) = sqrt(x^2 + y^2)             // simplified anonymous function
do sqrt(%1^2 + %2^2)                    // `do` version of anonymous function
```

下面是一个实际的例子：

```
让 s = [1, 2, 3, 4, 5, 6];
让 翻倍s = s.映射(执行 % * 2)
```

```
let s = [1, 2, 3, 4, 5, 6];
let double_s = s.map(do % * 2)
```

`映射`（`map`）的函数签名为 `List<E> map<T, E=T>(List<T>, E <= (T))`，其中第 2 个参数接受一个函数，该函数的作用是从一个数 `T` 转换成另外一个数 `E`，它是一个转换函数，`map` 函数的作用就是将一列数根据你的需要转换得另外一列数。比如你的转换函数是将传入的整数乘于 2,那么输入 `[1,2,3]` 经过 `map` 转换得出 `[2,4,6]`。

上例中，变量 s 的数据类型是 `列表<整数>`（`List<Int>`）, 由类型推导出 `map` 函数个类型参数是 `List<Int> map(List<Int>, Int <= (Int))`，其中转换函数的签名为：

`整数 <= (整数)`
`Int <= (Int)`

因此我们需要可以构造一个符合这个签名的匿名函数：

`匿名函数 整数 (整数 n) = n * 2`
`fn Int (Int n) = n * 2`

这个匿名函数可以简化为：

`匿名函数 (n) = n * 2`
`fn (n) = n * 2`

使用 `执行` 的版本：

`执行 % * 2`
`do % * 2`

跟标准的匿名函数一样，简化版匿名函数也是支持多行的，只要在 `执行`（`do`） 关键字后面换行即可写多行函数体，然后以 `以上`（`end`）关键字表示结束，示例：

```
让 翻倍s = s.映射(执行
        % * 2
    以上)
```

```
let double_s = s.map(do
        % * 2
    end)
```

### 函数的重载

可以定义两个或以上名称相同、返回值的数据类型相同，但参数列表不同的函数，这种情况称为函数的重载。函数重载一般是为了让调用者可以有选择地灵活调用函数。

示例：

```
01  函数 说 (字符串 name) = 输出行 (name)
02  函数 说 (字符串 name, 字符串 sentence) = 输出格式行 ("{} 你好, {}", name, sentence)
03
04  说 ("张三", "吃饭了吗") // 调用 02 函数
05  说 ("张三") // 调用 01 函数
```

```
01  function say (String name) = writeLine (name)
02  function say (String name, String sentence) = formatWriteLine ("Hi {}, {}", name, sentence)
03
04  say ("Foo", "What's up") // call 02
05  say ("Foo") // call 01
```

* 函数重载要求参数列表必须**不相同**，可以是参数数量不相同，可以是参数的类型不相同，可以是参数的模式不相同，或者上面三种情况的组合。
* 函数重载要求返回值数据类型必须**相同**，实际上，在同一个模块里不允许同名但不同返回值类型的函数。

### 函数的模式匹配

函数的参数不仅仅用于引用或记录传入的参数值（类似赋值语句），同时还能作为模式匹配，即只有实参跟参数要求的模式相匹配时，该参数才会获取相应的值。示例：

```
01  函数 显示余数 (Int a, Int 0) = 输出行 ("除数不能为零")
02  函数 显示余数 (Int a, Int b) = 输出格式行 ("余数为 {}", a `余` b)
03
04  显示余数 (5, 2) // 02 行函数被调用
05  显示余数 (5, 0) // 01 行函数被调用
```

```
01  function showRemain (Int a, Int 0) = writeLine ("The divisor cannot be zero")
02  function showRemain (Int a, Int b) = writeLine ("The remainder is {}", a `rem` b)
03
04  showRemain (5, 2) // 02 function is invoked
05  showRemain (5, 0) // 01 function is invoked
```

运行环境会根据函数的定义顺序，对每个函数的参数列表进行匹配，只有匹配中的那个函数定义才会被执行。

函数重载其实也是函数模式匹配的一种，对一个函数的匹配顺序是：

1. 检查参数的个数；
2. 检查每个参数的数据类型；
3. 检查每个参数是否模式匹配。

详细的模式匹配见 [08 模式](pattern-and-matching.md)

### 函数的条件分支

即使参数通过了模式匹配，函数还能再增加一道条件分支（在诸如 Haskell 语言里也叫 "哨卫"，"guard"）。

改写上一个示例如下：

```
01  函数 显示余数 (Int a, Int 0) = 输出行 ("除数不能为零")
02  函数 显示余数 (Int a, Int b) 条件
03      分支 b > a: 输出行 ("除数比被除数还大")
04      分支 b == a: 输出行 ("没有余数")
05      默认: 输出行 ("余数为 {}", a `余` b)
06  以上
07
08  显示余数 (5, 5) // 执行 04 行
09  显示余数 (5, 2) // 执行 05 行
10  显示余数 (5, 6) // 执行 03 行
11  显示余数 (5, 0) // 执行 01 行
```

```
01  function showRemain (Int a, Int 0) = writeLine ("The divisor cannot be zero")
02  function showRemain (Int a, Int b) condition
03      case b > a: writeLine ("divisor is larger than dividend")
04      case b == a: writeLine ("no remainder")
05      default: writeLine ("The remainder is {}", a `rem` b)
06  end
07
08  showRemain (5, 5) // execute line 04
09  showRemain (5, 2) // execute line 05
10  showRemain (5, 6) // execute line 03
11  showRemain (5, 0) // execute line 01
```

每一个分支都是独立的一个函数主体。如果函数主体是多行语句，则需要在语句 `case...:` 后面换行，然后是函数分支的主体，然后以 `end` 关键字结束分支。

最后一个分支通常是不限任何条件的默认分支，如果没有默认分支，当所有的条件都不满足时，运行环境会抛出一个运行时错误。

## 纯函数

纯函数是指函数的返回值只跟输入的参数值有关，跟所在的上下文环境无关，而且只要输入参数值一样，总能得出一样的结果值。

典型的纯函数如 `加`，`减` 等算术函数，运行环境提供了一系列纯函数，这些函数被标上 `@pure` 标注。

需要注意纯函数必须有具有意义的输入参数和返回值，当用户定义的函数只调用纯函数时，它自身也会自动成为纯函数。

XiaoXuan 运行环境能够对纯函数作优化处理，比如延迟执行、缓存结果、并行执行等。