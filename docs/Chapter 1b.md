<style>

code {
  white-space : pre-wrap !important;
  word-break: break-word;
}

</style>


# Basics in Lua

## 1b.0 - Basic Programming Concepts in Lua
The previous chapter was more general to all programming languages. Since the rest of this doc will be on lua specifically, I'll show how everything in the previous chapter works in lua. Well that's kind of a lie, it'll actually be Luau, Roblox's derivitive of lua. Luau has all the features of lua with some extra stuff on top of it.

<hr>

### 1b.1 - Complex Data Types
A lot of programming languages have the complex data types **hashmap**/**dictionary** */ and **array**. An array, as explained before, just contains multiple values in an ordered list. A hashmap maps "keys" to "values". For example, in javascript
```js
let Player = {
    "health": 100, // Key is "health", value is 100
    "name": "Kazi", // Key is "name", value is "Kazi"
    "age": 18, //Key is "age", value is 18

}
```
You can then index it to get values (e.g. ``Player["health"]`` will be ``100``)

Instead of having structs and arrays, lua has **tables** which can represent both:
```lua
local arrayTable = {5, "something", false}
local playerTable = {
    ["health"] = 100,
    ["name"] = "Kazi",
    ["age"] = 18,

}
```
An array table is just an automated way to do the following:
```lua
{
    [1] = ...,
    [2] = ...,
    [3] = ...,
    ...
}
```
You can modify the values inside of a table, or even add new ones:
```lua
arrayTable[3] = true -- arrayTable is now {5, "something", true}
playerTable["cute"] = true -- playerTable is now { ["health"] = 100, ["name"] = "Kazi", ["age"] = 18, ["cute"] = true, }
```
<hr>

### 1b.2 References
Lua doesn't have references for the most part. However, variables set to tables will always act as references:
```lua
local playerTable = {
    ["health"] = 100,
    ["name"] = "Kazi",
    ["age"] = 18,
    ["cute"] = true,

}

local playerNamedKazi = playerTable
playerNamedKazi["health"] = 80 -- This affects playerTable!! Now they will both be { ["health"] = 80, ["name"] = "Kazi", ["age"] = 18, ["cute"] = true, }
```
<hr>

Wow! We good for progressing now!