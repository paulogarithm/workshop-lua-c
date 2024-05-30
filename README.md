<h1 align=center> Lua x C </h1>

> [!IMPORTANT]
> This workshop is about the **lua** programming language. A little lua
> knowledge is requiered to do this workshop.<br/>
> You will also need C or C++ knowledges.
> 
> If you never touched lua in your life, you should follow the
> [`negative tasks`](#-1--a-lua-world) before.
> Else, go directly to [`ground zero`](#0--prerequisites-met);

> [!TIP]
> This part of lua are advanced mechanics, so feel free to ask me if you
> need help !

<details>
    <summary>  Lua crash course: </summary>

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

## `-4` | Tables ~~and chairs~~

Tables are the most important mechanics in lua. It allows user to store any `value` at a given `key`. The key can be litterally anything.

To create a table, you just need to use `{}`. By default, if you don't give a key, it acts like an array, and the key are `1, 2, 3...`

```lua
local fruits = { "banana", "apple", "mango" }
print(fruits[1]) -- banana (not apple)
```

If your key is a string, you can access it with a dot:
```lua
local me = {
    money = 0,
    bitches = 0,
    int32limit = 2147483647
}
me.charism = 0
print(me.bitches) -- 0
```

You can also loop through the tables using the for loop:
```lua
for key, value in pairs(me) do
    print(key, value) -- money 0, bitches 0, ...
end
```

Tables can have functions in it, which is interesting in OOP mechanics.

So to call them, you can just call them with `.`, but if you need to actually get the table used to call the function, you can use `:` and you will get the `self` variable.
```lua
local me = {
    age = 19,
    name = "pol"
}

function me.sayHi()
    print("Hi")
end

function me:birthday()
    self.age = self.age + 1
end

me.sayHi() -- Hi
print(me.age) -- 19
me:birthday()
print(me.age) -- 20
```

You can also use something that is called metatables to actually give tables funny attributes. But I let you search by your own.

Finally ! Now you can start the workshop !

</details>

## `0` | Prerequisites met

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
> ${\textsf{\color{#f00}╸}}$━━━━━━━━━━━━━━━━━━━ `0xp`

See this little bar ? This is your lua skills that are upgrading ! Each steps 

You will first have to create a module that returns a number:
```lua
local foo = require("coolmodule")

print(foo) -- 42
```

## `2` | Your own adder !
> ${\textsf{\color{#c04}━╸}}$━━━━━━━━━━━━━━━━━━ `10xp`

Hey see your lua skills ! They upgraded ! Now let's create a simple adder.

So your goal is to create a adder, which is how I want to store my number in the super game i'm developping.

I should be able to increment my variable stored in my adder object. This is how you would do it !

1. Create a new userdata type
```lua
local adder = require("coolmodule")
print(type(adder)) -- userdata
```

2. Make your userdata store a int variable and display it:
```lua
adder:display() -- 0
```

> [!TIP]
> Did you know you can apply `metatable` to a light user data ?
>
> The __index metamethod is definetely usefull !

3. Make a function that increments your adder value:
```lua
adder:display() -- 0
adder:increment()
adder:display() -- 1
```

4. Make your function take an optionnal parametter that is the number it can holds:
```lua
adder:display()     -- 0
adder:increment()
adder:display()     -- 1
adder:increment(3)
adder:display()     -- 4
```

> [!TIP]
> Did you know that passing 0 parametters is like giving `nil` as
> parametter ?

## `3` | An Adder with a capital 'A'
> ${\textsf{\color{#808}━━━━━╸}}$━━━━━━━━━━━━━━ `50xp`

Wow, ok you are definetely getting great at lua, however, would you pass this one ?

Ok so I want to have multiple and individuals Adder in my game, so can you make me a Adder class that allows me to create multiple adders !

```lua
local Adder = require("coolmodule")

local adder1 = Adder.new()
adder1:increment(42)

local adder2 = Adder.new()
adder2:increment(69)

adder2:display() -- 69
adder1:display() -- 42
```

Hmm ok this looks kind of what I want, can  you now (please) make it printable ?
```lua
local adder = require("coolmodule").new()
adder:increment(12)
print(adder) -- 12
```
> [!TIP]
> You should definitely check out the new `__tostring` metamethod, it's an absolute banger what it can do !

## `4` | My Window
> ${\textsf{\color{#40f}━━━━━━━╸}}$━━━━━━━━━━━━ `70xp`

Wait, I just realised something... it's useless to do an adder, since you already have the lua variables to do it !

Instead I will do something funnier !

Do you remember you Tek1 bsq/setting up ? Do you still have it ? Perfect! Because we are not gonna use it.

Instead, you will encapsulate any graphical C/C++ library of your choice. 

Don't worry you don't have to create everything, just 2 simple things:

1. A `createWindow` method to create a new window, which takes the title and the size.

2. An implicite destructor that clears your window, which is called at the end of the file.

> [!TIP]
> Destructor ? Does it exist a metamethod for destruction in lua ?

Walkthrough:

1. You should create a simple function that returns a new light user data.

2. Then you can set the userdata to a new window.

3. Then you can try to apply metamethod to the userdata with the destructor.

4. And then you are done !

Here is a code snippet that you should base your work on:
```lua
local Graphics = require("coolmodule")

local window = Graphics.createWindow("foo", 800, 600)
print(type(window))
os.execute("sleep 3")
print("foo")
```

It should outputs:
```c
userdata
// waits for 3 seconds
foo
destructor
```

And here is a little [video](./assets/video.mp4)

## `5` | The role changed...
> ${\textsf{\color{#08f}━━━━━━━━━━━━╸}}$━━━━━━━━━━ `120xp`