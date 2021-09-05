My notes on learning Elixir from https://elixir-lang.org/getting-started/

You need access to Elixir REPL for trying out the example. Install at https://elixir-lang.org/install.html

### Getting into the Elixir REPL

You can use the `iex` command to enter into the REPL after installing it.

### Getting help

Use the `h`

```exs
iex> h Kernel.trunc
```

If the function has multiple signatures, specify the number of signatures to select the function.

```exs
iex> h Kernel.trunc/1
```

### Booleans

`true` and `false`

You can compare like

`true` == `false`


### Checking data type

`is_boolean`, `is_integer`, `is_float`, `is_number` etc.

### Atoms (Symbols)

They are constants.

Example

`:apple`, `:orange` etc

Atoms are equal if the names are equal

```exs
iex> :apple == :orange`
true
```

`true` and `false` are also atoms. You can skip `:` for them.

```exs
> :true == true
true
```

### Strings

Encoded using double quotes.

Supports string interpolation.

```exs
iex> string = "hello"
"hello"
iex> "#{string} world"
"hello world"
```

You can print using
```exs
iex> IO.puts("hello")
hello
:ok
```

Strings are represented as bytes. `ö` takes 2 bytes to be represnted in UTF-8.

You can concatenate strings using `<>`

```exs
iex(47)> x = "hello"
"hello"
iex(48)> y = "world"
"world"
iex(49)> x <> " " <> y
"hello world"
```

```exs
iex> is_binary("hellö")
true
iex> byte_size("hellö")
6
iex> String.length("hellö")
5
iex> String.upcase("hello")
"HELLO"
```

### Anonymous functions


```exs
iex> add = fn a, b -> a + b end
#Function<43.40011524/2 in :erl_eval.expr/5>
iex> add(1, 3)
** (CompileError) iex:5: undefined function add/2

iex> add.(1, 3)
4
iex> is_function(add)
true
```

The `.` is required to distinguish between normal functions.

You can access the variables that are in scope when the function is defined.

```exs
iex> x = 7
7
iex> print_x = fn -> IO.puts(x) end
#Function<45.40011524/0 in :erl_eval.expr/5>
iex> print_x.()
7
:ok
```

Variabled declared inside the function does not affect the surrounding environment

```exs
#Function<45.40011524/0 in :erl_eval.expr/5>
iex> y = 2                  
2
iex> set_y = fn -> y = 3 end
warning: variable "y" is unused (there is a variable with the same name in the context, use the pin operator (^) to match on it or prefix this variable with underscore if it is not meant to be used)
  iex:14

#Function<45.40011524/0 in :erl_eval.expr/5>
iex> set_y.()
3
iex> y
2
```

### Lists

Lists are implemented as Linked Lists.

```exs
iex> list = [1, 2, "three", :four, true, false]  
[1, 2, "three", :four, true, false]
```

You can concatenate or subtract lists. Returns a new list. The Elixr data structures are immutable.
```exs
iex(17)> list = [1, 2, "three", :four, true, false]  
[1, 2, "three", :four, true, false]
iex(18)> [1, 2] ++ [3, 4]
[1, 2, 3, 4]
iex(19)> [1, 2, 3, 4, 5, true, false] -- [4, 5, false]
[1, 2, 3, true]
```

Head is the first element and tail is the remaining elements.
```
iex(23)> hd(x)
1
iex(24)> tl(x)
[2, 3, 4, 5]
```

You can use `length` to get the length of the list. Since list is a linked
list the length operation takes linear time. In general, "length" is used to
refer for operations that take linear time and size is used for operations that
don't constant time.


### Tuples

Tuples are stored continously in memory.

```
iex> x = {1, "2", :hello, true, [1, 2]}
{1, "2", :hello, true, [1, 2]}
```

So unlike lists you can access them using index.

```exs
iex> elem(x, 2)                        
:hello
```

You can also change the element of a tuple and returns a new tuple. The new tuple share the
elements with the old tuple except the one that has been replaced.

```exs
iex(38)> x
{1, "2", :hello, true, [1, 2]}
iex(39)> put_elem(x, 3, :world)
{1, "2", :hello, :world, [1, 2]}
iex(40)> x
{1, "2", :hello, true, [1, 2]}
```

You can use the `tuple_size` function to get the size of the tuple.

``
iex(42)> x            
{1, "2", :hello, true, [1, 2]}
iex(43)> tuple_size(x)
5
```


### Boolean operators

Similiar to Python, there is `and` and `or` for evaluating boolean values.
``iex
iex(55)> true and false        
false
iex(56)> false and true
false
iex(57)> true or false
true
iex(58)> true or raise("Error")
true
```

Unlike Python the left hand operator has to be a boolean.

```exs
iex(62)> true and 1      
1
iex(63)> 1 or true
** (BadBooleanError) expected a boolean on left-side of "or", got: 1

iex(63)> 1 and true
** (BadBooleanError) expected a boolean on left-side of "and", got: 1

iex(63)> true or 1
true
iex(64)> true and 3
3
```

But there are the traditional `||`, `&&` and `!` operators which can be
evaulated using any data typles. 0 is not considered to be false.

```exs
iex(86)> nil && 1 
nil
iex(87)> nil && 2
nil
iex(88)> true and 2          
2
iex(89)> true and 3
3
iex(90)> 0 || 5
0
iex(91)> false || 5
5
iex(92)> false || 0
0
iex(93)> false && true
false
iex(94)> 0 && true    
true
iex(95)> 0 && nil 
nil
iex(96)> nil && 1
nil
```



