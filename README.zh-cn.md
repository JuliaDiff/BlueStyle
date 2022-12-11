# Blue: a Style Guide for Julia
# Blue: Julia 风格指南

[![Code Style: Blue](https://img.shields.io/badge/code%20style-blue-4495d1.svg)](https://github.com/invenia/BlueStyle)

本文档指定了Julia代码的编程风格惯例。这些惯例的说明参考了包括Python的[PEP8](http://legacy.python.org/dev/peps/pep-0008/), Julia的[Notes for Contributors](https://github.com/JuliaLang/julia/blob/master/CONTRIBUTING.md)以及Julia的[Style Guide](https://docs.julialang.org/en/v1/manual/style-guide/)在内的多种来源。

*其他语言版本: [English](README.md), [简体中文](README.zh-cn.md)

## 关于一致性

当您秉承本风格时，意识到这是指南而非规则是很重要的。

这[恰如PEP8中所述](http://legacy.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds):

> A style guide is about consistency.
>
> （风格指南是关于一致性的。）
>
> Consistency with this style guide is important.
>
> （与这个风格指南保持一致是重要的。）
>
> Consistency within a project is more important.
>
> （同一个项目内部的一致性是更重要的。）
>
> Consistency within one module or function is the most important.
>
> （同一个模组或函数内的一致性是最重要的。）
>
> But most importantly: know when to be inconsistent -- sometimes the style guide just doesn't apply.
>
> （但最最重要的还是：得知道什么时候不保持一致 -- 因为有时风格指南就是不能适用。）
>
> When in doubt, use your best judgment.
>
> （当有疑问时， 听从你自己的判断。）
>
> Look at other examples and decide what looks best.
>
> （参考一下其他的例子并决定怎样看起来最好。）
>
> And don't hesitate to ask!
>
> （不要犹豫尽管提问！）

## 简介

请尝试同时遵循 [Julia贡献指南(Julia Contribution Guidelines)](https://github.com/JuliaLang/julia/blob/master/CONTRIBUTING.md#general-formatting-guidelines-for-julia-code-contributions)， [Julia风格指南(Julia Style Guide)](https://docs.julialang.org/en/v1/manual/style-guide/)以及本指南。

如有冲突以本指南优先（已知的冲突将会在本指南中注明）。

- 每层缩进使用4个空格，不要使用 `tab`。
- 尝试遵守每行92字母的长度限制。
- [模组](https://docs.julialang.org/en/v1/manual/modules/)和[类型](https://docs.julialang.org/en/v1/manual/types/)使用大驼峰式命名法。
- 方法名使用带下划线的小写（注：与此相反，[Julia的base库代码](https://github.com/JuliaLang/julia/tree/master/base)常常选择用不带下划线的小写。）
- 使用`using`导入模组，每行一个模组并且尽量放在文件的前面。
- 注释是好的，试着解释清楚代码的意图。
- 使用空格来增加代码的可读性。
- 行尾不要添加多余的空格。
- 避免在括号内填充空格，例如：`Int64(value)`比`Int64( value )`好。

## 目录

- [代码格式](#代码格式)
    - [模组导入](#模组导入)
    - [函数导出](#函数导出)
    - [全局变量](#全局变量)
    - [函数命名](#函数命名)
    - [方法定义](#方法定义)
    - [关键字参数](#关键词参数)
    - [空格](#空格)
    - [具名元组](#具名元组)
    - [Numbers](#numbers)
    - [三元运算符](#三元运算符)
    - [For 循环](#For-循环)
    - [模组](#模组)
    - [类型注释](#类型注释)
    - [包版本的指定](#包版本的指定)
    - [注释](#注释)
    - [文档](#文档)
- [测试格式化](#测试格式化)
    - [测试集](#测试集)
    - [比较](#比较)
- [性能和优化](#性能和优化)
- [编辑器配置](#编辑器配置)
    - [Sublime Text 设置](#sublime-text-设置)
    - [Vim 设置](#vim-设置)
    - [Atom 设置](#atom-设置)
    - [VS-Code 设置](#vs-code-设置)
- [代码风格徽章](#代码风格徽章)

## 代码格式

### 模组导入

[模组导入](https://docs.julialang.org/en/v1/manual/modules/#Summary-of-module-usage-1) 应该放在文件最前面或者紧跟在 `module` 声明后面。

通过 `include` 加载的文件应该避免在其内指定模组导入，而应该将导入语句放在加载其的文件中（例如 "src/Example.jl" 或 "test/runtests.jl"）。

导入模组时应当每行只指定一个包。

导入语句应该以包/模组的名称的字母顺序进行排序（相对导入在绝对导入之前）。

```julia
# Yes:
using A
using B

# No:
using A, B

# No:
using B
using A
```

明确声明要把什么带入作用域的导入应该被分组为：模组、常量、类型、宏和函数。

这些分组应该按照该顺序指定，每组的内容应该按照字母顺序排序，通常模组的导入在单独的一行。

写成伪代码就是：

```julia
using Example: $(sort(modules)...)
using Example: $(sort(constants)...), $(sort(types)...), $(sort(macros)...), $(sort(functions)...)
```

如果你只明确导入少数几个项目，你可以选择使用如下的单行形式：

```julia
using Example: $(sort(modules)...), $(sort(constants)...), $(sort(types)...), $(sort(macros)...), $(sort(functions)...)
```

在某些情况下，只要在逻辑上更说得通，在一个组内可以轮流排序。
例如，当明确导入"Period"的子类型时，你可能会想按照单位从大到小排序：

```julia
using Dates: Year, Month, Week, Day, Hour, Minute, Second, Millisecond
```

超过单行长度的显式导入行应使用续行或对同一个包使用多个导入语句。
在创建轮流分组时，应倾向于使用多个导入语句。

```julia
# Yes:
using AVeryLongPackage: AVeryLongType, AnotherVeryLongType, a_very_long_function,
    another_very_long_function

# Yes:
using AVeryLongPackage: AVeryLongType, AnotherVeryLongType
using AVeryLongPackage: a_very_long_function, another_very_long_function

# No:
using AVeryLongPackage:
    AVeryLongType,
    AnotherVeryLongType,
    a_very_long_function,
    another_very_long_function

# No:
using AVeryLongPackage: AVeryLongType
using AVeryLongPackage: AnotherVeryLongType
using AVeryLongPackage: a_very_long_function
using AVeryLongPackage: another_very_long_function
```

注意：在编写包时，请倾向于使用带有明确声明的导入。
这样做可使包的维护变得更为容易，因为可以知道包在导入什么功能，以及什么时候可以安全地去掉相应依赖。

请倾向于使用 `using` 而非 `import`，以确保对一个函数的扩展总是明确的和有目的的。

```julia
# Yes:
using Example

Example.hello(x::Monster) = "Aargh! It's a Monster!"
Base.isreal(x::Ghost) = false

# No:
import Base: isreal
import Example: hello

hello(x::Monster) = "Aargh! It's a Monster!"
isreal(x::Ghost) = false
```

如果你的确需要使用 `import` ，那么请为 `import` 和 `using` 语句分别创建单独的分组，以空行划分：

```julia
# Yes:
import A: a
import C

using B
using D: d

# No:
import A: a
using B
import C
using D: d
```

### 函数导出

所有打算作为公共API的函数都应该被导出。
所有的函数导出应该出现在主模组文件的顶部，在模组导入之后。
避免分割单个导出到多行；要么每行定义一个导出，要么按主题分组。

```julia
# Yes:
export foo
export bar
export qux

# Yes:
export get_foo, get_bar
export solve_foo, solve_bar

# No:
export foo,
    bar,
    qux
```

### 全局变量

应尽可能避免使用全局变量。当需要时，全局变量应当在前面加上  `const` ，并且变量名全大写，用下划线分隔（例如：`MY_CONSTANT` ）。
它们应该被定义在文件的顶部，紧随导入和导出之后，但在 `__init__` 函数之前。
如果你真的想要可变全局变量风格的行为，你可能会想了解一下可变容器或者闭包。

### 函数命名

函数的名称应该描述一个动作或属性，而不用考虑参数的类型；参数的类型已经提供了这个信息。
例如，`submit_bid(bid)` 应该是 `submit(bid::Bid)`，而 `bids_in_batch(batch)` 应该是`bids(batch::Batch)` 。

函数的名称通常应限于一或两个小写的单词，用下划线进行分隔。
如果你发现很难在不丢失信息的情况下缩短你的函数名称，你可能需要在类型签名中考虑更多的信息，或者将函数的职责细分成两个或多个函数。
一般来说，具有明确职责的较短函数是首选。

**注意**：仅供内部使用的函数应该在前面加下划线进行标记（例如，`_internal_utility_function(X, y)`）。
同样的命名规则也可以用于内部类型和常量（例如，`_MyInternalType`, `_MY_CONSTANT`），虽然这应该不那么常见。
将一个函数标记为内部或私有可以让他人知道，他们不应该期望从该函数的使用中获得任何形式的API稳定性。

### 方法定义

只有在能保持单行的情况下才使用短形式的函数定义。

```julia
# Yes:
foo(x::Int64) = abs(x) + 3

# No:
foobar(array_data::AbstractArray{T}, item::T) where {T<:Int64} = T[
    abs(x) * abs(item) + 3 for x in array_data
]
```
```julia
# Yes:
function foobar(array_data::AbstractArray{T}, item::T) where T<:Int64
    return T[abs(x) * abs(item) + 3 for x in array_data]
end

# No:
foobar(
    array_data::AbstractArray{T},
    item::T,
) where {T<:Int64} = T[abs(x) * abs(item) + 3 for x in array_data]
```

当使用长形式的函数时[总是使用 `return` 关键字](https://groups.google.com/forum/#!topic/julia-users/4RVR8qQDrUg)：


```julia
# Yes:
function fnc(x::T) where T
    result = zero(T)
    result += fna(x)
    return result
end

# No:
function fnc(x::T) where T
    result = zero(T)
    result += fna(x)
end
```
```julia
# Yes:
function Foo(x, y)
    return new(x, y)
end

# No:
function Foo(x, y)
    new(x, y)
end
```

当使用 `return` 关键字时，总是明确地返回一个值，即便它是 `return nothing`。
```julia
# Yes:
function maybe_do_thing()
    # code
    return nothing
end

# No:
function maybe_do_thing()
    # code
    return
end
```

对参数超过92个字符的函数定义，每个参数都应该用换行来分隔，并缩进一级。

```julia
# Yes:
function foobar(
    df::DataFrame,
    id::Symbol,
    variable::Symbol,
    value::AbstractString,
    prefix::AbstractString="",
)
    # code
end

# Ok:
function foobar(df::DataFrame, id::Symbol, variable::Symbol, value::AbstractString, prefix::AbstractString="")
    # code
end

# No: 如果参数不能全部放进去，就不要把任何参数放在与左括号相同的一行.
function foobar(df::DataFrame, id::Symbol, variable::Symbol, value::AbstractString,
    prefix::AbstractString="")

    # code
end

# No: 在这种情况下，所有的args都应该另起一行.
function foobar(
    df::DataFrame, id::Symbol, variable::Symbol, value::AbstractString,
    prefix::AbstractString=""
)
    # code
end

# No: 缩进太多啦.
function foobar(
        df::DataFrame,
        id::Symbol,
        variable::Symbol,
        value::AbstractString,
        prefix::AbstractString="",
    )
    # code
end
```

如果所有参数加起来满足92个字符的限制，那么你可以把它们放在一行里。
同样的，如果你把位置参数和关键字参数分开成两行，你也可以分别遵循同样的规则。

```julia
# Ok:
function foobar(
    df::DataFrame, id::Symbol, variable::Symbol, value::AbstractString; prefix::String=""
)
    # code
end

# Ok: 把所有的args和所有的kwargs放在不同的行上是可以的.
function foobar(
    df::DataFrame, id::Symbol, variable::Symbol, value::AbstractString;
    prefix::String=""
)
    # code
end

# Ok: 把所有的位置参数放在一行，而每个kwarg放在不同的行上.
function foobar(
    df::DataFrame, id::Symbol, variable::Symbol, value::AbstractString;
    prefix="I'm a long default setting that probably shouldn't exist",
    msg="I'm another long default setting that probably shouldn't exist",
)
    # code
end

# No: 因为单行超过了92个字符.
function foobar(
    df::DataFrame, id::Symbol, variable::Symbol, value::AbstractString; prefix::AbstractString=""
)
    # code
end

# No: args和kwargs应该分成两行.
function foobar(
    df::DataFrame, id::Symbol, variable::Symbol,
    value::AbstractString; prefix::AbstractString=""
)
    # code
end

# No: kwargs超过了92个字符.
function foobar(
    df::DataFrame, id::Symbol, variable::Symbol, value::AbstractString;
    prefix="I'm a long default setting that probably shouldn't exist", msg="I'm another long default settings that probably shouldn't exist",
)
    # code
end
```

### 关键词参数

当调用一个函数时，永远用分号把关键字参数和位置参数分开。
这避免了在模棱两可的情况下的错误（如对`Dict`做splatting操作时）。

```julia
# Yes:
xy = foo(x; y=3)
ab = foo(; a=1, b=2)

# Ok:
ab = foo(a=1, b=2)

# No:
xy = foo(x, y=3)
```

### 空格

- 避免紧靠小括号、方括号或大括号内的多余空格。

    ```julia
    # Yes:
    spam(ham[1], [eggs])
    
    # No:
    spam( ham[ 1 ], [ eggs ] )
    ```

- 避免紧靠在逗号或分号之前的多余空格。

    ```julia
    # Yes:
    if x == 4 @show(x, y); x, y = y, x end
    
    # No:
    if x == 4 @show(x , y) ; x , y = y , x end
    ```

- 在范围索引中避免在`:`两边加空格。使用括号来阐明两边的表达式。

    ```julia
    # Yes:
    ham[1:9]
    ham[9:-3:0]
    ham[1:step:end]
    ham[lower:upper-1]
    ham[lower:upper - 1]
    ham[lower:(upper + offset)]
    ham[(lower + offset):(upper + offset)]

    # No:
    ham[1: 9]
    ham[9 : -3: 1]
    ham[lower : upper - 1]
    ham[lower + offset:upper + offset]  # 不要这样写，很容易理解成 `ham[lower + (offset:upper) + offset]`
    ```

- 避免为了与另一个运算符对齐而在一个赋值（或其他）运算符两边使用一个以上的空格。

    ```julia
    # Yes:
    x = 1
    y = 2
    long_variable = 3

    # No:
    x             = 1
    y             = 2
    long_variable = 3
    ```


- 在大多数二进制运算符的两边各加一个空格：赋值(`=`), [更新运算符](https://docs.julialang.org/en/v1/manual/mathematical-operations/#Updating-operators-1) (`+=`, `-=`, 等等), [数值比较运算符](https://docs.julialang.org/en/v1/manual/mathematical-operations/#Numeric-Comparisons-1) (`==`, `<`, `>`, `！=`, 等等), [lambda运算符](https://docs.julialang.org/en/v1/manual/functions/#man-anonymous-functions-1) (`->`) 。可排除在本指南规范之外的二进制运算符包括：[范围运算符](https://docs.julialang.org/en/v1/base/math/#Base.::) (`:`), [有理数运算符](https://docs.julialang.org/en/v1/base/math/#Base.://) (`//`), [指数运算符](https://docs.julialang.org/en/v1/base/math/#Base.:^-Tuple{Number,Number}) (`^`), [可选参数/关键字](https://docs.julialang.org/en/v1/manual/functions/#Optional-Arguments-1) (例如：`f(x=1; y=2)`)。

    ```julia
    # Yes:
    i = j + 1
    submitted += 1
    x^2 < y

    # No:
    i=j+1
    submitted +=1
    x^2<y
    ```

- 避免在一元运算数和表达式之间使用空格。

    ```julia
    # Yes:
    -1
    [1 0 -1]

    # No:
    - 1
    [1 0 - 1]  # Note: evaluates to `[1 -1]`
    ```

- 避免多余的空行。在单行方法定义之间避免空行，除此之外用单个空行来分隔函数，如果需要的话再加上注释。

    ```julia
    # Yes:
    # Note: an empty line before the first long-form `domaths` method is optional.
    domaths(x::Number) = x + 5
    domaths(x::Int) = x + 10
    function domaths(x::String)
        return "A string is a one-dimensional extended object postulated in string theory."
    end
    
    dophilosophy() = "Why?"
    
    # No:
    domath(x::Number) = x + 5
    
    domath(x::Int) = x + 10



    function domath(x::String)
        return "A string is a one-dimensional extended object postulated in string theory."
    end


    dophilosophy() = "Why?"
    ```

- 不能满足行数限制放在一行的函数调用应该被分开来，使包含开始和结束括号的行缩进到同一水平，而函数的参数则再缩进一级。
  在大多数情况下，参数和/或关键字应该分别放在不同的行上。
  注意，这条规则与Julia的典型惯例相冲突，即缩进下一行以与包含参数的左括号对齐。
  如果在一个具有不同惯例的包中工作，请遵循该包中使用的惯例，而不是使用本指南。
    ```julia
    # Yes:
    f(a, b)
    constraint = conic_form!(
        SOCElemConstraint(temp2 + temp3, temp2 - temp3, 2 * temp1),
        unique_conic_forms,
    )
  
    # No:
    # Note: `f`调用是短到可以放进单行的
    f(
        a,
        b,
    )
    constraint = conic_form!(SOCElemConstraint(temp2 + temp3,
                                               temp2 - temp3, 2 * temp1),
                             unique_conic_forms)
    ```

- 使用数组、元组或函数展开形式的赋值时，其第一个左括号应放在赋值运算符的同一行上，右括号应与赋值的缩进程度一致。
  另外当赋值较短时，你可以在单行上进行赋值：

    ```julia
    # Yes:
    arr = [
        1,
        2,
        3,
    ]
    arr = [
        1, 2, 3,
    ]
    result = func(
        arg1,
        arg2,
    )
    arr = [1, 2, 3]
  
    # No:
    arr =
    [
        1,
        2,
        3,
    ]
    arr =
    [
        1, 2, 3,
    ]
    arr = [
        1,
        2,
        3,
        ]
    ```

- 采用展开形式的嵌套数组或元组应将左右括号放在同一缩进水平：

    ```julia
    # Yes:
    x = [
        [
            1, 2, 3,
        ],
        [
            "hello",
            "world",
        ],
        ['a', 'b', 'c'],
    ]

    # No:
    y = [
        [
            1, 2, 3,
        ], [
            "hello",
            "world",
        ],
    ]
    z = [[
            1, 2, 3,
        ], [
            "hello",
            "world",
        ],
    ]
    ```

- 在处理展开形式的数组、元祖或函数表达式时，总是包括尾部的逗号。
  这使得将来可以轻松地移动元素或添加额外的元素。
  当表达式只在单行内时，应不加尾部的逗号。

    ```julia
    # Yes:
    arr = [
        1,
        2,
        3,
    ]
    result = func(
        arg1,
        arg2,
    )
    arr = [1, 2, 3]
  
    # No:
    arr = [
        1,
        2,
        3
    ]
    result = func(
        arg1,
        arg2
    )
    arr = [1, 2, 3,]
    ```


- 包含三重引号和三重反引号的多行语句应该缩进。
  由于三重引号使用最小缩进行的缩进（不包括开始的引号），所以字符串中缩进最少的一行或结束的引号应缩进一次。
  三重反引号也应遵循这种风格，尽管缩进对它们来说并不重要。

    ```julia
    # Yes:
    str = """
        hello
        world!
        """
    cmd = ```
        program
            --flag value
            parameter
        ```

    # No:
    str = """
    hello
    world!
    """
    cmd = ```
          program
              --flag value
              parameter
          ```
    ```

- 使用三重引号或三重反引号的赋值应将开始的引号与赋值运算符放在同一行。

    ```julia
    # Yes:
    str2 = """
           hello
       world!
       """

    # No:
    str2 =
       """
           hello
       world!
       """
    ```

- 将类似的单行语句组合在一起。

    ```julia
    # Yes:
    foo = 1
    bar = 2
    baz = 3

    # No:
    foo = 1

    bar = 2

    baz = 3
    ```

- 使用空行来分隔不同的多行代码块。

    ```julia
    # Yes:
    if foo
        println("Hi")
    end

    for i in 1:10
        println(i)
    end

    # No:
    if foo
        println("Hi")
    end
    for i in 1:10
        println(i)
    end
    ```

- 在函数定义之后和end语句之前，不要插入空行。

    ```julia
    # Yes:
    function foo(bar::Int64, baz::Int64)
        return bar + baz
    end

    # No:
    function foo(bar::Int64, baz::Int64)

        return bar + baz
    end

    # No:
    function foo(bar::In64, baz::Int64)
        return bar + baz

    end
    ```

- 在控制流语句和return语句之间换行。


    ```julia
    # Yes:
    function foo(bar; verbose=false)
        if verbose
            println("baz")
        end
    
        return bar
    end
    
    # Ok:
    function foo(bar; verbose=false)
        if verbose
            println("baz")
        end
        return bar
    end
    ```

### 具名元组

在 `NamedTuple` 中的 `=` 字符应该像在关键字参数中的那样间隔。
变量名和它的值之间不应该有空格。
`NamedTuple`不应该在开始加";"作前缀。
空的 `NamedTuple` 应当写作 `NamedTuple()` 而不是 `(;)`。

```julia
# Yes:
xy = (x=1, y=2)
x = (x=1,)  # Trailing comma required for correctness.
x = (; kwargs...)  # Semicolon required to splat correctly.

# No:
xy = (x = 1, y = 2)
xy = (;x=1,y=2)
x = (; x=1)
```

### Numbers

浮点数应开头和/或结尾的零不应省去。

```julia
# Yes:
0.1
2.0
3.0f0

# No:
.1
2.
3.f0
```

### 三元运算符

三元运算符(`?:`)一般只占一行。
不要将多个三元运算符嵌套使用。
如果嵌套了许多条件，可以考虑使用`if`-`elseif`-`else`条件语句，或者派发，或者一个字典。

```julia
# Yes:
foobar = foo == 2 ? bar : baz

# No:
foobar = foo == 2 ?
    bar :
    baz
foobar = foo == 2 ? bar : foo == 3 ? qux : baz
```

作为一种替代方案，你可以使用一个复合的布尔类型表达式。

```julia
# Yes:
foobar = if foo == 2
    bar
else
    baz
end

foobar = if foo == 2
    bar
elseif foo == 3
    qux
else
    baz
end
```

### For 循环
For 循环应该总是使用 `in` ，而不要用 `=` 或 `∈` 。这也适用于列表和生成器推导式。

```julia
# Yes
for i in 1:10
    #...
end

[foo(x) for x in xs]

# No:
for i = 1:10
    #...
end

[foo(x) for x ∈ xs]
```

### 模组

通常情况下，包含模组定义的文件不应包含任何在该模组之外运行的其他代码。
也就是说，应该在文件的顶部用 `module` 关键字声明模组，在文件结尾用`end` 结束声明。
前后都不要放其他代码（除了模组前的docstring）。
在这种情况下，模组中的代码**不应该**缩进。

有时，例如为了测试，或者为了命名一个枚举时，*最好*在文件的中间声明一个子模组。
在这种情况下，子模组中的代码**应该**缩进。


### 类型注释

函数定义的类型注释应该尽可能的通用。

```julia
# Yes:
splicer(arr::AbstractArray, step::Integer) = arr[begin:step:end]

# No:
splicer(arr::Array{Int}, step::Int) = arr[begin:step:end]
```

尽可能地使用通用类型，这可以实现各种输入，使你的代码更加通用。

```julia
julia> splicer(1:10, 2)
1:2:9

julia> splicer([3.0, 5, 7, 9], 2)
2-element Array{Float64,1}:
 3.0
 7.0
```

对类型字段的类型注释需要多加考虑。
为字段使用特定的具体类型可以使得Julia优化内存布局，但会降低灵活性。
例如，让我们看看 `MySubString` 类型，它允许我们在不复制数据的情况下处理字符串的一个小节：

```julia
mutable struct MySubString <: AbstractString
    string::AbstractString
    offset::Integer
    endof::Integer
end
```

我们希望这个类型能够容纳`AbstractString`的任何子类型，但我们是否需要让`offset`和`endof`能够容纳`Integer`的任何子类型？
其实并不需要，我们在这里使用`Int`应该就可以了（64位系统上使用`Int64`，32位系统上使用`Int32`）。
注意，尽管我们使用的是 `Int`，但用户仍然可以像这样调用：`MySubString("foobar", 0x4, 0x6);`，因为这里提供的 `offset` 和 `endof` 的值将被转换为 `Int`类型。

```julia
mutable struct MySubString <: AbstractString
    string::AbstractString
    offset::Int
    endof::Int
end
```

如果我们真的很关心性能，我们还可以参数化我们的类型。
`MySubString` 目前的定义允许我们在任何时候用 `AbstractString` 的任何子类型修改 `string` 字段。
使用参数化类型允许我们在构造时使用 `AbstractString` 的任何子类型，但字段类型将被设置为具体的（如`String`），并且在实例的生命周期内不能改变。


```julia
mutable struct MySubString{T<:AbstractString} <: AbstractString
    string::T
    offset::Integer
    endof::Integer
end
```

总的来说，最好一开始就保持通用的类型，然后再使用参数化的类型来优化它们。
在代码设计过程中过早地进行优化会影响你对先前代码做重构的能力。

### 包版本的指定

为了简单起见，并与更广泛的 Julia 社区保持一致，在指定软件包的版本要求时，避免使用默认的插入符（caret， `^` ）指定。

```julia
# Yes:
DataFrames = "0.17"

# No:
DataFrames = "^0.17"
```

### 注释

注释应该被用来说明代码的预期行为。
当代码在做一些初看起来并不明显，但实则非常巧妙的事情时，这一点尤其重要。
避免赘述代码显然在做的事。

```julia
# Yes:
x = x + 1      # 补偿边界

# No:
x = x + 1      # 增量 x
```

与代码相左的注释比没有注释还糟。
始终优先考虑保持注释与代码相同步!

注释应该是完整的句子。
如果一条注释是单个短语或句子，它的第一个词应该大写，除非它是一个以小写字母开头的标识符（永远不要变更标识符的大小写！）。

如果注释很短，结尾的句号可以省略。
块注释（Block comments）一般由一个或多个完整的句子组成，每个句子应以句号结束。

注释应与表达式至少隔开两个空格，并且在 `#` 后面有一个空格。

当在文档中引提到Julia时，请注意 "Julia "指的是编程语言，而 "julia"（通常在反引号内，如`julia`）指的是可执行程序。

只有在适合行长限制的情况下才使用行内注释。如果你的注释不能放在行内，那么就把注释放在它所注解内容的上面。

```julia
# Yes:

# Number of nodes to predict. Again, an issue with the workflow order. Should be updated
# after data is fetched.
p = 1

# No:

p = 1  # Number of nodes to predict. Again, an issue with the workflow order. Should be
# updated after data is fetched.
```

### 文档

大多数模组、类型和函数都应该有[docstring](http://docs.julialang.org/en/v1/manual/documentation/)。
话虽如此，只有被导出的函数才需要有文档。
避免为像 `==` 这样的方法写文档，因为该函数的内置docstring已经很好地涵盖了相关细节。
尽量为函数而不是单个方法写文档，因为通常所有的方法都有类似的docstring。
如果你在给一个已经有docstring的函数添加方法，请只在你的函数的行为偏离了现有的docstring时才添加一个新的docstring。

用[Markdown](https://en.wikipedia.org/wiki/Markdown)的格式编写docstring，并且保持简单精炼。
单行长度超过92个字符的docstring需要进行换行。

```julia
"""
    bar(x[, y])

Compute the Bar index between `x` and `y`. If `y` is missing, compute the Bar index between
all pairs of columns of `x`.
"""
function bar(x, y) ...
```

当类型或方法有很多参数时，也许写不出一个简明的文档串。
在这些情况下，建议你使用下面的模板。
注意，如果某个部分不适用或过于冗长（例如下面出现的"Throws"，如果你的函数中并没有抛出异常语句），可以将其去掉。
当内容足够长时，建议你在标题和内容之间放一个空行。
在同一个docstring中是否使用额外的空格，要尽量保持一致。
请注意，额外的空格只是为了方便阅读原始的markdown，并不影响渲染的版本。

类型模板（如果与构造函数的docstring重复，则应被跳过）。

```julia
"""
    MyArray{T,N}

My super awesome array wrapper!

# Fields
- `data::AbstractArray{T,N}`: stores the array being wrapped
- `metadata::Dict`: stores metadata about the array
"""
struct MyArray{T,N} <: AbstractArray{T,N}
    data::AbstractArray{T,N}
    metadata::Dict
end
```

函数模板（只有被导出的函数需要）。

```julia
"""
    mysearch(array::MyArray{T}, val::T; verbose=true) where {T} -> Int

Searches the `array` for the `val`. For some reason we don't want to use Julia's
builtin search :)

# Arguments
- `array::MyArray{T}`: the array to search
- `val::T`: the value to search for

# Keywords
- `verbose::Bool=true`: print out progress details

# Returns
- `Int`: the index where `val` is located in the `array`

# Throws
- `NotFoundError`: I guess we could throw an error if `val` isn't found.
"""
function mysearch(array::AbstractArray{T}, val::T) where T
    ...
end
```

如果你的方法包含很多参数或关键字，你可能会想把他们从第一行的方法签名中去掉，转而使用 `args...` 和/或 `kwargs...`。

```julia
"""
    Manager(args...; kwargs...) -> Manager

A cluster manager which spawns workers.

# Arguments

- `min_workers::Integer`: The minimum number of workers to spawn or an exception is thrown
- `max_workers::Integer`: The requested number of workers to spawn

# Keywords

- `definition::AbstractString`: Name of the job definition to use. Defaults to the
    definition used within the current instance.
- `name::AbstractString`: ...
- `queue::AbstractString`: ...
"""
function Manager(...)
    ...
end
```

可以在同一个docstring中为一个函数的多个方法写文档。
请注意只对你已经定义的函数这样做。

```julia
"""
    Manager(max_workers; kwargs...)
    Manager(min_workers:max_workers; kwargs...)
    Manager(min_workers, max_workers; kwargs...)

A cluster manager which spawns workers.

# Arguments

- `min_workers::Int`: The minimum number of workers to spawn or an exception is thrown
- `max_workers::Int`: The requested number of workers to spawn

# Keywords

- `definition::AbstractString`: Name of the job definition to use. Defaults to the
    definition used within the current instance.
- `name::AbstractString`: ...
- `queue::AbstractString`: ...
"""
function Manager end

```

如果重点的文档超过92个字符，该行应换行并略微缩进。
避免将文本与`:`对齐。

```julia
"""
...

# Keywords
- `definition::AbstractString`: Name of the job definition to use. Defaults to the
    definition used within the current instance.
"""
```

关于在Julia关于文档书写的其他细节，请参见[官方文档](https://docs.julialang.org/en/v1/manual/documentation/)。


对于写在Markdown文件中的文档，如`README.md`或`docs/src/index.md`，保持每行一句话。

## 测试格式化

### 测试集

Julia 提供了[测试集](https://docs.julialang.org/en/v1/stdlib/Test/#Working-with-Test-Sets-1)，它允许开发者将测试按照逻辑进行分组。
测试集可以是嵌套的，理想情况下，代码包应该只包含一个"root"测试集。
建议在"runtests.jl"文件中包含root测试集，而root测试集中包含其余的测试。

```julia
@testset "PkgExtreme" begin
    include("arithmetic.jl")
    include("utils.jl")
end
```

### 比较

大多数测试是以`@test x == y`的形式写的。
由于`==`函数不考虑类型，所以 `@test 1.0 == 1` 这样的测试是有效的。
避免在测试比较中加入视觉噪声,影响代码的简练清晰程度：


```julia
# Yes:
@test value == 0

# No:
@test value == 0.0
```

## 性能和优化

下面这些tips中的一些可以在Julia的[性能提示](https://docs.julialang.org/en/v1/manual/performance-tips/)中找到。

Julia的性能提升主要来自于它能够根据函数的输入类型对函数进行特化处理。
将变量和功能放在全局命名空间或模组的命名空间中会阻碍这一点。
这样做的一个后果是，传统MATLAB风格的脚本将产生慢得出奇的代码。
有两种方法可以缓解这种情况：

- 将尽可能多的功能移入函数。
- 使用`const`将全局变量声明为常量。

记住，当你第一次调用具有某种特定类型签名的函数时，它会为给定的输入类型编译该函数。
编译有时会占大量时间，所以要避免在函数第一次运行时对其进行分析/计时。来自[BenchmarkTools](https://github.com/JuliaCI/BenchmarkTools.jl)软件包的`@benchmark`和`@btime`宏可能会有用，因为它们会多次运行函数，并报告时间和内存分配汇总的统计数据，减少了在进行基准测试前先运行函数的需要。

## 编辑器配置

### Sublime Text 设置

如果你是 Sublime Text 的用户，我们建议你在你的 Julia 语法具体设置中配置以下几项。
要修改这些设置，首先在 Sublime Text 中打开任何 Julia 文件 (`*.jl`)。
然后前往：`Preferences > Settings - More > Syntax Specific - User`：

```json
{
    "translate_tabs_to_spaces": true,
    "tab_size": 4,
    "trim_trailing_white_space_on_save": true,
    "ensure_newline_at_eof_on_save": true,
    "rulers": [92]
}
```

### Vim 设置

如果你是Vim用户，我们建议你添加如下代码到你的 `.vim/vimrc` 文件：

```
" ~/.vim/vimrc
set tabstop=4       " Set tabstops to a width of four columns.
set softtabstop=4   " Determine the behaviour of TAB and BACKSPACE keys with expandtab.
set shiftwidth=4    " Determine the results of >>, <<, and ==.

" Identify .jl files as Julia. If using julia-vim plugin, this is redundant.
autocmd BufRead,BufNewFile *.jl set filetype=julia
```

然后创建或编辑 `.vim/after/ftplugin/julia.vim`，添加Julia相关配置：

```
" ~/.vim/after/ftplugin/julia.vim
setlocal expandtab       " Replace tabs with spaces.
setlocal textwidth=92    " Limit lines according to Julia's CONTRIBUTING guidelines.
setlocal colorcolumn=+1  " Highlight first column beyond the line limit.
```

此外，你可能会发现使用以下插件很有用：
[julia-vim plugin](https://github.com/JuliaEditorSupport/julia-vim)，
它增加了Julia语法高亮和一些其他很酷的功能。

### Atom 设置

Atom默认行长为80个字符。我们希望写julia的时候，在92个字符的时候就换行。
设置如下：

1. 进入 `Atom -> Preferences -> Packages`。
2. 搜索 "language-julia" 包，并打开它的设置。
3. 找到 preferred line length（在 "Julia Grammar" 下面），并将其改为92。


### VS-Code 设置

如果你是 VS Code 用户，我们建议你在你的 Julia 语法特定设置中配置如下。
请用以下命令打开你的VS代码设置：<kbd>CMD</kbd>+<kbd>,</kbd> (Mac OS) or <kbd>CTRL</kbd>+<kbd>,</kbd> (其他操作系统), 并在 `settings.json` 中添加如下代码：

```json
{
    "[julia]": {
        "editor.detectIndentation": false,
        "editor.insertSpaces": true,
        "editor.tabSize": 4,
        "files.insertFinalNewline": true,
        "files.trimFinalNewlines": true,
        "files.trimTrailingWhitespace": true,
        "editor.rulers": [92],
    },
}
```

此外，你可能会发现[Julia VS-Code插件](https://github.com/julia-vscode/julia-vscode)很有用。


## 代码风格徽章

通过在你的 `README.md` 中加入徽章，让贡献者们知道你的项目遵循了Blue风格指南。
```md
[![Code Style: Blue](https://img.shields.io/badge/code%20style-blue-4495d1.svg)](https://github.com/invenia/BlueStyle)
```

这个徽章是蓝色的：
[![Code Style: Blue](https://img.shields.io/badge/code%20style-blue-4495d1.svg)](https://github.com/invenia/BlueStyle)
