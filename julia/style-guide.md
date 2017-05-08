#  Style Guide

This document specifies the Invenia coding conventions for Julia code.
These conventions were created from a variety of sources including Python's [PEP8](http://legacy.python.org/dev/peps/pep-0008), Julia's [Notes for Contributors](https://github.com/JuliaLang/julia/blob/master/CONTRIBUTING.md), and Julia's [Style Guide](https://docs.julialang.org/en/latest/manual/style-guide.html).

## A Word on Consistency

When adhering to this style it's important to realize that these are guidelines and not rules.
This is [stated best in the PEP8](http://legacy.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds):

>A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is most important.

> But most importantly: know when to be inconsistent -- sometimes the style guide just doesn't apply. When in doubt, use your best judgment. Look at other examples and decide what looks best. And don't hesitate to ask!


## Synopsis

Attempt to follow both the [Julia Contribution Guidelines](https://github.com/JuliaLang/julia/blob/master/CONTRIBUTING.md#general-formatting-guidelines-for-julia-code-contributions), the [Julia Style Guide](https://docs.julialang.org/en/latest/manual/style-guide.html), and this guide.
When convention guidelines conflict this guide takes precedence (known conflicts will be noted in this guide).

 *  Use 4 spaces per indentation level, no tabs.
 *  Try to adhere to a 92 character line length limit.
 *  Use upper camel case convention for [modules](http://julia.readthedocs.org/en/latest/manual/modules/) and [types](http://julia.readthedocs.org/en/latest/manual/types/).
 *  Use lower case with underscores for method names (note: Julia code likes to use lower case without underscores).
 *  Comments are good, try to explain the intentions of the code.
 *  Use whitespace to make the code more readable.
 *  No whitespace at the end of a line (trailing whitespace).
 *  Avoid padding brackets with spaces. ex. `Int64(value)` preferred over `Int64( value )`.

## Editor Configuration

### Sublime Text Settings

If you are a user of Sublime Text we recommend that you have the following options in your Julia syntax specific settings.
To modify these settings first open any Julia file (`*.jl`) in Sublime Text. Then navigate to: `Preferences > Settings - More > Syntax Specific - User`

```json
{
    "translate_tabs_to_spaces": true,
    "tab_size": 4,
    "trim_trailing_white_space_on_save": true,
    "ensure_newline_at_eof_on_save": true,
    "rulers": [92]
}
```

### Vim Settings

If you are a user of Vim we recommend that you add the following options to your `.vimrc` file.

```
set tabstop=4                             " Sets tabstops to a width of four columns.
set softtabstop=4                         " Determines the behaviour of TAB and BACKSPACE keys with expandtab.
set shiftwidth=4                          " Determines the results of >>, <<, and ==.

au FileType julia setlocal expandtab      " Replaces tabs with spaces.
au FileType julia setlocal colorcolumn=93 " Highlights column 93 to help maintain the 92 character line limit.
```

By default, Vim seems to guess that `.jl` files are written in Lisp.
To ensure that Vim recognizes Julia files you can manually have it check for the `.jl` extension, but a better solution is to install [Julia-Vim](https://github.com/JuliaLang/julia-vim), which also includes proper syntax highlighting and a few cool other features.

### Atom Settings

Atom defaults preferred line length to 80 characters. We want that at 92 for julia.  
To change it:

1. Go to Atom -> Preferences -> Packages.
2. Search for the "language-julia" package and open the settings for it.
3. Find preferred line length (under "Julia Grammar") and change it to 92.

## Code Formatting

### Function Naming

Names of functions should describe an action or property irrespective of the type of the argument; the argument's type provides this information instead.
For example, `submit_bid(bid)` should be `submit(bid::Bid)` and `bids_in_batch(batch)` should be `bids(batch::Batch)`.

Names of functions should usually be limited to one or two lowercase words separated by underscores.
If you find it hard to shorten your function names without losing information, you may need to factor more information into the type signature or split the function's responsibilities into two or more functions.
In general, shorter functions with clearly-defined responsibilities are preferred.

### Method Definitions

Only use short-form function definitions when they fit on a single line:

```julia
# Yes: 
foo(x::Int64) = abs(x) + 3
# No: 
foobar{T<:Int64}(array_data::AbstractArray{T}, item::T) = T[
    abs(x) * abs(item) + 3 for x in array_data
]

# No:
foobar{T<:Int64}(
    array_data::AbstractArray{T},
    item::T,
) = T[abs(x) * abs(item) + 3 for x in array_data]
# Yes:
function foobar{T<:Int64}(array_data::AbstractArray{T}, item::T)
    return T[abs(x) * abs(item) + 3 for x in array_data]
end
```

When using long-form functions [always use the `return` keyword](https://groups.google.com/forum/#!topic/julia-users/4RVR8qQDrUg):

```julia
# Yes:
function fnc{T}(x::T)
    result = zero(T)
    result += fna(x) 
    return result
end
# No:
function fnc{T}(x::T)
    result = zero(T)
    result += fna(x) 
end

# Yes:
function Foo(x, y)
    return new(x, y)
end
# No:
function Foo(x, y)
   new(x, y)
end
```

Functions definitions with parameter lines which exceed 92-characters should seperate each parameter by a newline and indent by one-level:

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

# No:
function foobar(df::DataFrame, id::Symbol, variable::Symbol, value::AbstractString, prefix::AbstractString="")
    # code
end
# No:
function foobar(df::DataFrame, id::Symbol, variable::Symbol, value::AbstractString,
    prefix::AbstractString="")

    # code
end
# No:
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

### Keyword Arguments

When calling a function always separate your keyword arguments from your positional arguments with a semicolon.
This avoids mistakes in ambiguous cases (such as splatting a `Dict`).

```julia
# Yes: 
xy = foo(x; y=3)
# No: 
xy = foo(x, y=3)
```

### Whitespace

Avoid extraneous whitespace in the following situations:

* Immediately inside parentheses, square brackets or braces.

    ```julia
    Yes: spam(ham[1], [eggs])
    No:  spam( ham[ 1 ], [ eggs ] )
    ```

* Immediately before a comma or semicolon:

    ```julia
    Yes: if x == 4 @show(x, y); x, y = y, x end
    No:  if x == 4 @show(x , y) ; x , y = y , x end
    ```

* When using ranges unless additional operators are used:

    ```julia
    Yes: ham[1:9], ham[1:3:9], ham[1:3:end]
    No:  ham[1: 9], ham[1 : 3: 9]
    ```

    ```julia
    Yes: ham[lower:upper], ham[lower:step:upper]
    Yes: ham[lower + offset : upper + offset]
    Yes: ham[(lower + offset):(upper + offset)]
    No:  ham[lower + offset:upper + offset]
    ```

* Immediately before the open bracket that starts and indexing or the argument list of a function call:

    ```julia
    Yes: arr[1] = vec[end]
    No:  arr [1] = vec [end]  # Note: Deprecated in Julia 0.4
    ```

    ```julia
    Yes: spam(1)
    No:  spam (1)  # Note: Deprecated in Julia 0.4
    ```

* More than one space around an assignment (or other) operator to align it with another:

    ```
    Yes:
    x = 1
    y = 2
    long_variable = 3

    No:
    x             = 1
    y             = 2
    long_variable = 3
    ```

* Always surround these binary operators with a single space on either side: assignment (=), [updating operators](http://julia.readthedocs.org/en/latest/manual/mathematical-operations/#updating-operators) (+=, -= etc.), [numeric comparisons operators](http://julia.readthedocs.org/en/latest/manual/mathematical-operations/#numeric-comparisons) (==, <, >, !=, etc.). Note that this guideline does not apply when performing assignment in method definitions.

    ```
    Yes: i = i + 1
    No:  i=i+1
    
    Yes: submitted += 1
    No:  submitted +=1

    Yes: x^2 < y
    No:  x^2<y
    ```

* Assignments using expanded array, tuple, or function notation should have the first open bracket on the same line assignment operator and the closing bracket should match the indentation level of the assignment. Alternatively you can perform assignments on a single line when they are short:

    ```julia
    Yes:
    arr = [
        1,
        2,
        3,
    ]
    arr = [
        1, 2, 3,
    ]
    result = Function(
        arg1,
        arg2,
    )
    arr = [1, 2, 3]
        

    No:
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

* Nested array or tuples that are in expanded notation should have the opening and closing brackets at the same indentation level:
    
    ```julia
    Yes:
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

    No:
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

* Always include the trailing comma when working with expanded arrays, tuples or functions notation. This allows future edits to easily move elements around or add additional elements:

    ```julia
    Yes:
    arr = [
        1,
        2,
        3,
    ]
    result = Function(
        arg1,
        arg2,
    )

    No:
    arr = [
        1,
        2,
        3
    ]
    result = Function(
        arg1,
        arg2
    )
    ```

### Type annotation

Annotations for function definitions should be as general as possible.

```julia
# Yes:
splicer(arr::AbstractArray, step::Integer) = arr[1:step:end]

# No:
splicer(arr::Array{Int}, step::Int) = arr[1:step:end]
```

Using as generic types as possible allows for a variety of inputs and allows you code to be more general:

```julia
julia> splicer(1:10, 2)
1:2:9

julia> splicer([3.0,5,7,9], 2)
2-element Array{Float64,1}:
 3.0
 7.0
```

Annotations on type fields need to be given a little more thought. Using specific concrete types for fields allows Julia to optimize the memory layout but can reduce flexibility.
For example lets take a look at the type `MySubString` which allows us to work with a subsection of a string without having to copy the data:

```julia
type MySubString <: AbstractString
    string::AbstractString
    offset::Integer
    endof::Integer
end
```

We want the type to be able to hold any subtype of `AbstractString` but do we need to have `offset` and `endof` to be able to hold any subtype of `Integer`? Really, no we should be ok to use `Int` here (`Int64` on 64-bit systems and `Int32` on 32-bit systems)
Note that even though we're using `Int` a user can still do things like `MySubString("foobar", 0x4, 0x6);` as provided `offset` and `endof` values will be converted to an `Int`.

```julia
type MySubString <: AbstractString
    string::AbstractString
    offset::Int
    endof::Int
end
```

If we truely care about performance there is one more thing we can do by making our type parametric.
The current definition of `MySubString` allows us to modify the `string` field at any time with any subtype of `AbstractString`.
Using a parametric type allows to use any subtype of `AbstractString` upon construction but the field type will be set to something concrete (like `String`) and cannot be changed for the lifetime of the instance.

```
type MySubString{T<:AbstractString} <: AbstractString
    string::T
    offset::Integer
    endof::Integer
end
```

Overall, it is best to keep the types general to start with and later optimize the using parametric types.
Optimizing too early in the code design process can impact your ability to refactor the code early on.


### Comments

Comments should be used to state the intended behaviour of code.
This is especially important when the code is doing something clever that may not be obvious upon first inspection.
Avoid writing comments that state exactly what the code obviously does.

```julia
Yes:
x = x + 1      # Compensate for border

No:
x = x + 1      # Increment x
```

Comments that contradict the code are much worse than no comments.
Always make a priority of keeping the comments up-to-date with code changes!

Comments should be complete sentences.
If a comment is a phrase or sentence, its first word should be capitalized, unless it is an identifier that begins with a lower case letter (never alter the case of identifiers!).

If a comment is short, the period at the end can be omitted.
Block comments generally consist of one or more paragraphs built out of complete sentences, and each sentence should end in a period.

Comments should be separated by at least two spaces from the expression and have a single space after the `#`.

When referencing Julia in documentation note that "Julia" refers to the programming language
while "julia" (typically in backticks, e.g. `julia`) refers to the executable.

### Documentation

It is recommended that most modules, types and functions should have [docstrings](http://docs.julialang.org/en/latest/manual/documentation.html).
That being said, only exported functions are required to be documented.
Avoid documenting methods like `==` as the built in docstring for the function already covers the details well.
Try to document a function and not individual methods where possible as typically all methods will have similar docstrings.
If you are adding a method to a function which was defined in `Base` or another package only add a docstring if the behaviour of your function deviates from the existing docstring.

Docstrings are written in [Markdown](https://en.wikipedia.org/wiki/Markdown) and should be
concise.

```julia
"""
    bar(x[, y])

Compute the Bar index between `x` and `y`. If `y` is missing, compute
the Bar index between all pairs of columns of `x`.
"""
function bar(x, y) ...
```

When types or methods have lots of parameters it may not be feasible to write a concise docstring.
In these cases it is recommended you use the templates below. Note if a section doesn't apply or is overly verbose (for example "Throws" if your function doesn't throw an exception) it can be excluded.

Type Template (should be skipped if is redundant with the constructor(s) docstring):

```julia
"""
    MyArray{T,N}

My super awesome array wrapper!

# Fields
* `data::AbstractArray{T,N}`: stores the array being wrapped
* `metadata::Dict`: stores metadata about the array
"""
type MyArray{T,N} <: AbstractArray{T,N}
    data::AbstractArray{T,N}
    metadata::Dict
end
```

Function Template (only required for exported functions):

```julia
"""
    mysearch{T}(array::MyArray{T}, val::T; verbose=true) -> Int

Searches the `array` for the `val`. For some reason we don't want to use Julia's
builtin search :) 

# Arguments
* `array::MyArray{T}`: the array to search
* `val::T`: the value to search for

# Keywords
* `verbose::Bool=true`: print out progress details
    
# Returns
* `Int`: the index where `val` is located in the `array`

# Throws
* NotFoundError: I guess we could throw an error if `val` isn't found.
"""
function mysearch{T}(array::AbstractArray{T}, val::T)
    ...
end
```

If your method contains lots of arguments or keywords you may want to exclude them from the
method signature on the first line and instead use `args...` and/or `kwargs...`

```julia
"""
    Manager(args...; kwargs...) -> Manager

A cluster manager which spawns workers.

# Arguments
* `min_workers::Integer`: The minimum number of workers to spawn or an exception is thrown
* `max_workers::Integer`: The requested number of worker to spawn

# Keywords
* `definition::AbstractString`: Name of the job definition to use. Defaults to the definition used within the current instance.
* `name::AbstractString`: ...
* `queue::AbstractString`: ...
"""
function Manager(...)
    ...
end
```

Feel free to document multiple methods for a function within the same docstring. Be careful
to only do this for functions you have defined.

```julia
"""
    Manager(max_workers; kwargs...)
    Manager(min_workers:max_workers; kwargs...)
    Manager(min_workers, max_workers; kwargs...)

A cluster manager which spawns workers.

# Arguments
* `min_workers::Int`: The minimum number of workers to spawn or an exception is thrown
* `max_workers::Int`: The number of requested workers to spawn

# Keywords
* `definition::AbstractString`: Name of the job definition to use. Defaults to the definition used within the current instance.
* `name::AbstractString`: ...
* `queue::AbstractString`: ...
"""
function Manager end

```

For additional details on documenting in Julia see the [official documentation](http://docs.julialang.org/en/latest/manual/documentation.html).

## Test Formatting

### Testsets

Julia provides [test sets](http://docs.julialang.org/en/stable/stdlib/test/?highlight=testset#working-with-test-sets) which allows developers to group tests into logical groupings.
Test sets can be nested and ideally packages should only have a single "root" test set.
It is recommended that the "runtests.jl" file contains the root test set which contains the remainder of the tests:

```julia
@testset "PkgExtreme" begin
    include("arithmetic.jl")
    include("utils.jl")
end
```

### Comparisons

Most tests are written in the form `@test x == y`.
Since the `==` function doesn't take types into account tests like the following are valid: `@test 1.0 == 1`.
Avoid adding visual noise into test comparisons:

```julia
# Yes:
@test value == 0

# No:
@test value == 0.0
```

## Performance and Optimization

Several of these tips are contained within Julia's [Performance Tips](http://docs.julialang.org/en/latest/manual/performance-tips/).

Much of Julia's performance gains come from being able to specialize functions on their input types.
Putting variables and functionality in the global namespace or module's namespace thwarts this.
One consequence of this is that conventional MATLAB-style scripts will result in surprisingly slow code. There are two ways to mitigate this:

 *  Move as much functionality into functions as possible.
 *  Declare global variables as constants using `const`.

Remember that the first time you call a function with a certain type signature it will compile that function for the given input types.
Compilation is sometimes a significant portion of time, so avoid profiling/timing functions on their first run. 
