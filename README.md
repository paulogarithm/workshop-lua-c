<h1 align=center> Lua x C </h1>

> [!IMPORTANT]
> This workshop is about the **lua** programming language. A little lua
> knowledge is requiered to do this workshop.<br/>
> You will also need C or C++ knowledges.
> 
> If you never touched lua in your life, you should follow the negative tasks
> before. Else, go directly to [1](#1--my-first-module);

## `-1` | A lua world


## `0` | Building lua

1. Install lua using the following table:

|        | installation command             | compilation command                                |
|--------|----------------------------------|----------------------------------------------------|
| dnf    | `sudo dnf install lua-devel -y`  | `gcc -shared -fpic module.c -llua -o module.so`    |
| apt    | `sudo apt install liblua5.4-dev` | `gcc -shared -fpic module.c -llua5.4 -o module.so` |
| pacman | `sudo pacman -Syu lua`           | `gcc -shared -fpic module.c -llua -o module.so`    |
| brew   | `sudo brew install lua`          | `gcc -shared -fpic module.c -llua -o module.so`    |

2. Export the lua cpath like so
```sh
export LUA_CPATH="$(pwd)/?.so;;"
```

3. Compile the lua 


## `1` | My First Module
> ${\textsf{\color{red}╸}}$━━━━━━━━━━━━━━━━━━━

See this little bar ? This is your lua skills that are upgrading !

You will first have to create a module that returns a number:
```lua
local foo = require("mymodule.so")

print(foo) -- 42
```

## `2` | 
