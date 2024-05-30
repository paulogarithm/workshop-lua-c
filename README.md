<h1 align=center> Lua x C </h1>

> [!IMPORTANT]
> This workshop is about the **lua** programming language. A little lua
> knowledge is requiered to do this workshop.<br/>
> You will also need C or C++ knowledges.
> 
> If you never touched lua in your life, you should follow the
> [`negative tasks`](#-1--a-lua-world) before.
> Else, go directly to [`ground zero`](#0--building-lua);

> [!TIP]
> This part of lua are advanced mechanics, so feel free to ask me if you
> need help !

## `-1` | A lua world

Lua is an interpretted language created in 1993. It kinda looks like Ruby.

Here is what creating a variable looks like:
```lua
local num = 3
local str = "hello"
```

Lua is not strict typed, however, you can have a warning on the vscode
if your variable changes type
```lua
local a = 42
a = "foo" -- warning
```

And now you also learnt above how to do a comment !

Lua have 8 (or 9 it depends how you count) types:
- nil <br>
`local a = nil; local a;`
- number (and integer) <br>
`local a = 42; local a = 3.14`
- string <br>
`local a = "hello"`
- boolean <br>
`local a = true`
- thread <br>
`local a = coroutine.create(...)`
- function <br>
`function a() ... end`
- table <br>
`local a = { ... }`
- userdata <br>
`can't be created, it's a C pointer`

So normally you should understand the types from nil to thread.

The other ones are the ones we are mostly gonna use in this workshop.

However, we have to see more basics before.

## `-2` | Basic lua knowledge

Now we are gonna see some lua basics.

Here is three ways to do looping in lua:
```lua
local a = 0
while a < 10 do
    print(a)
    a = a + 1
end

for b = 0, 10 do
    print(b)
end

local c = 0
repeat
    print(c)
    c = c + 1
until c >= 10
```

Here is how to do conditions:
```lua
if you == "league player" then
    print("stinky")
elseif you == "at epitech" then
    print("stinky too")
else
    print("sticky again")
end
```
If checks for a `non-nil` and `not false` value.

```lua
if nil then -- wont do
    ... 
end
if 1 == 2 then -- 1 == 2 is false, so wont do
    ...
end
```

## `-3` | or functions, and

And here is a function (example for factorial recursive):
```lua
local function factorial(n)
    if (n == 1) or (n == 0) then
        return 1
    end
    return n * factorial(n)
end
```

Lua can have `;` but it is for psychopaths. You can also write lua on a single line, but that's also for psychopaths.
```lua
function f(n)if n<=0 then return 1 end return n*f(n-1)end
```
And yes this works.

You can also do ternary using only `or` and `and`. These opperators are really usefull since its not like C.

Here, the `or` will chose the first non-nil and non-false value, else it takes the last one:
```lua
false or 3 -- 3
42 or 53 -- 42
nil or false -- false
false or nil -- nil
```

`and` will do the opposite and return the first nil or false value he found, else it returns the last value:
```lua
false and 3 -- false
42 or 53 -- 53
nil or false -- nil
false or nil -- false
```

So using this, you can do ternary like that:
```lua
(condition and first) or second
```

Now we can improve our psychopath's factorial:
```lua
f=function(n)return n<=0 and 1 or n*f(n-1)end
```

## `-3` | Tables ~~and chairs~~

Tables are the most 

## `0` | Building lua

1. Install lua using the following table:

|        | installation command             |
|--------|----------------------------------|
| dnf    | `sudo dnf install lua-devel -y`  |
| apt    | `sudo apt install liblua5.4-dev` |
| pacman | `sudo pacman -Syu lua`           |
| brew   | `sudo brew install lua`          |

2. Export the lua cpath like so
```sh
export LUA_CPATH="$(pwd)/?.so;;"
```

3. Write your code. **A starter function should look like this:**
```c
#include <lua.h>
#include <lauxlib.h>
#include <lualib.h>

int luaopen_coolmodule(lua_State *L) // 'coolmodule' is the name of your module !
{
    // your code
    return 0; // this number is for the number of variables
}
```

4. Compile the lua using the following table:

|        | compilation command                                    |
|--------|--------------------------------------------------------|
| apt    | `gcc -shared -fpic module.c -llua5.4 -lm -o module.so` |
| others | `gcc -shared -fpic module.c -llua -lm -o module.so`    |

## `1` | My First Module
> ${\textsf{\color{red}╸}}$━━━━━━━━━━━━━━━━━━━

See this little bar ? This is your lua skills that are upgrading !

You will first have to create a module that returns a number:
```lua
local foo = require("coolmodule.so")

print(foo) -- 42
```

## `2` | 
