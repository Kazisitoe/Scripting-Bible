<style>

code {
  white-space : pre-wrap !important;
  word-break: break-word;
}

</style>

# State and Behavior, Object Oriented Programming

## 3.0 - State and Behavior in Lua via OOP
**State** and **behavior** describe the two main aspects of what code does. State describes the properties of something and behavior describes how it acts based on the state. In **object oriented programming** (OOP), which is one of the most common and widely-used programming **paradigms** (styles), state and behavior are grouped together in "objects", which can be represented as tables in lua. Objects typically follow a blueprint (the **class**), which can **inherit** from **base classes** (essentially copy a base class and add extra stuff to it). **Properties** are values stored within the object that describe its state, and **methods** are functions that are referenced within the object that act based on its state. Lua tables are specially set up to allow for OOP to look more elagant, which will be covered as we go over how OOP is implemented in lua. In this chapter, we will create a ``Player`` class as an example.

<hr>

### 3.1 - Objects as Tables
In lua, properties are fundamentally represented as hashmaps with string keys. As a refresher, hashmaps map "keys" to "values":
```lua
local hashmap = {
    ["key"] = "value", -- String key "key" mapped to string value "value"
    [2] = true, -- Number key 2 mapped to boolean value true

}

local x = hashmap["key"] -- x = "value"
local y = hashmap[2] -- y = true
```
Because of how common string keys are, lua has a more simplified format for them:
```lua
local hashmap = {
    key1 = "value1", -- Simplified
    ["key2"] = "value2",
}
local w = hashmap.key1 -- "value1"
local x = hashmap["key1"] -- Still "value1"
local y = hashmap.key2 -- "value2"
local z = hashmap["key2"] -- Still "value2"
```
By not using brackets, it automatically assumes that the key is a string. By indexing with ``.``, it automatically assumes we are indexing with a string. With this in mind, we can represent the properties of an instance of a ``Player`` class like so:
```lua
local playerInstance = {
    name = "Kazi",
    gender = "Male", -- I swear
    age = 18,
    health = 100,
    maxHealth = 100,
}
```
Now, we can try to add some behavior to the ``playerInstance`` with a method!
```lua
playerInstance.greetings = function()
    print(playerInstance.name, "says hi!", "They are a", playerInstance.age, "year old", playerInstance.gender)
end

playerInstance.greetings()
```
Output:
> "Kazi" "says hi!" "They are a" 18 "year old" "Male"

This works! But there is an issue with this. It would be a huge pain to constantly do this with every single ``Player`` object, having to define every property and make unique functions for every method in it. To fix this issue, we will need to make a **constructor** function (a function that creates and returns an object based on the class), and a class table that will contain all the methods that will then be referenced by the object. A constructed object based on a class is called an **instance** of the class.

<hr>

### 3.2 - Classes in Lua
In lua, classes and constructors are usually created in the same table. Before, when defining a method inside of the ``playerInstance`` table, we did ``playerInstance.greeting = function()``, but due to how frequently methods and functions stored in tables are, we will see that lua has another way of doing this. Now, let's create our class table and constructor function!
```lua
local Player = {} -- Class table

-- This will be our constructor function. Calling it "new" is standard convention
function Player.new(name, gender, age, maxHealth) -- Doing `function table.x()` is shorthand for doing `table.x = function` or `table["x"] = function()
    local playerObject = {
        name = name,
        gender = gender,
        age = age,
        health = maxHealth,
        maxHealth = maxHealth,

    }

    return playerObject

end

local playerInstance = Player.new("Kazi", "Male", 18, 100)
```
Nice! Now we can call ``Player.new`` every time we want to make a new ``Player`` instance. Now, let's give it some behavior. Since the behavior will always be the same between instances, we should not define it in the instance itself like properties, but instead store it in the base class and make them as methods that can work with any table instead of referring to one specific one:
```lua
local Player = {}

function Player.new(name, gender, age, maxHealth)
    local playerObject = {
        name = name,
        gender = gender,
        age = age,
        health = maxHealth,
        maxHealth = maxHealth,

    }

    return playerObject

end

function Player.greetings(instance)
    print(instance.name, "says hi!", "They are a", instance.age, "year old", instance.gender)
end

local playerInstance = Player.new("Kazi", "Male", 18, 100)
Player.greetings(playerInstance)
```
Output:
> "Kazi" "says hi!" "They are a" 18 "year old" "Male"

It is a little bit fucked to have to directly use the base class in order to access the behavior of a specific instance though, it would be much nicer to just do ``playerInstance.greetings(PlayerInstance)``. We could in theory just take every method inside the ``Player`` class table and store it inside the instances the same way we do with properties, but that can get very tedious! Instead, what if we could just made it so whenever we index ``playerInstance`` with something it doesn't contain, it instead just pulls it from the ``Player`` table automatically! Well, we can, yay!! Let's do it then!!!!

<hr>

### 3.3 - Metatables
**Metatables** are a special feature of lua that allows you to change the way a table works. It can allow you to make it so you can add, subtract, divide, multiply, etc. a table, call it as a function, and much more. The thing we'd want to change for making objects is what happens when you index with a key that doesn't exist in a table:
```lua
local table = {
    key1 = "value"
}

local x = table.key2
```
<details>
<summary>What do you expect x to equal?</summary>
nil
</details>
Since ``key2`` does not exist/have a value in ``table``, the default behavior of lua would be to just assume that it is ``nil``. With metatables, we can override this default behavior. Then, when we do ``playerInstance.greetings()``, we can make it so instead of ``greetings`` just being nil (since it does not exist within ``playerInstance``) and everything not working, it will just pull the ``greetings`` function from ``Player``.

Metatables work through **metamethods**, functions that define what the new behavior will be stored inside of a table. The metamethod we are interested in is ``__index``, which is called whenever you index a table with a key it does not have a value for. You then call ``setmetatable`` to attatch the metatable to a specific table. Here's an example to give a table a new default value:
```lua
local table = {
    key1 = "value"
}

local metatable = {
    __index = function(tableBeingIndexed, index) -- Roblox will automatically pass in the table the metatable is attatched to and what you are trying to index it with as the two arguments
        return "default value for "..index
    end

}
setmetatable(table, metatable)

local w = table.key1
local x = table.key2
local y = table.key3
local z = table.wow
```
<details>
<summary>What is the value of w</summary>
"value"
</details>
<details>
<summary>What is the value of x</summary>
"default value for key2"
</details>
<details>
<summary>What is the value of y</summary>
"default value for key3"
</details>
<details>
<summary>What is the value of z</summary>
"default value for wow"
</details>

Now, we can use ``__index`` to pull values from ``Player`` as default values for instances:
```lua
local Player = {}

function Player.new(name, gender, age, maxHealth)
    local playerObject = {
        name = name,
        gender = gender,
        age = age,
        health = maxHealth,
        maxHealth = maxHealth,

    }

    setmetatable(playerObject, {
        __index = function(tableBeingIndexed, index)
            return Player[index]
        end,

    })

    return playerObject

end

function Player.greetings(instance)
    print(instance.name, "says hi!", "They are a", instance.age, "year old", instance.gender)
end

local playerInstance = Player.new("Kazi", "Male", 18, 100)
playerInstance.greetings(playerInstance)
```
Now, on the last line, here is what will happen:

1. It will try to get the value of ``playerInstance.greetings``
2. Since it does not exist, it would normally just default to nil. However, it will check to see if it has a metatable with ``__index``. It does, so it calls ``__index`` with ``playerInstance, "greetings"``
3. This will return ``Player["greetings"]``, meaning that ``playerInstance.greetings`` will give us ``Player.greetings``
4. ``Player.greetings`` is then called with ``playerInstance``

Since it is common with OOP to pass a table into its method, like we do with ``playerInstance.greetings(playerInstance)``, lua has some shorthand for that specifically. By using ``:`` instead of ``.``, lua will make the first argument the table, meaning you can do ``playerInstance:greetings()``. Similarly, we can define methods the same way:
```lua
function Player:greetings()
    print(self.name, "says hi!", "They are a", self.age, "year old", self.gender)
end
```
``self`` is used to refer to the first parameter, which is assumed to be a class instance.

Since it is really common to have it so ``__index`` pulls values from another table, lua has some shorthand for ``__index`` that allows us to simplify the code:
```lua
local Player = {}

function Player.new(name, gender, age, maxHealth)
    local playerObject = {
        name = name,
        gender = gender,
        age = age,
        health = maxHealth,
        maxHealth = maxHealth,

    }

    setmetatable(playerObject, {
        __index = Player,
    })

    return playerObject

end

function Player:greetings()
    print(self.name, "says hi!", "They are a", self.age, "year old", self.gender)
end
```
This will do the same thing as before. Furthermore, since every instance has the same metatable, there isn't a need to create a new metatable every time we construct an object. We can instead just make the class table also a metatable!
```lua
local Player = {}

Player.__index = Player

function Player.new(name, gender, age, maxHealth)
    local playerObject = {
        name = name,
        gender = gender,
        age = age,
        health = maxHealth,
        maxHealth = maxHealth,

    }

    setmetatable(playerObject, Player)

    return playerObject

end
```

Now that we finally have it set up, let's add some new methods to the ``Player`` class:
```lua
local Player = {}

Player.__index = Player

function Player.new(name, gender, age, maxHealth)
    local playerObject = {
        name = name,
        gender = gender,
        age = age,
        health = maxHealth,
        maxHealth = maxHealth,

    }

    setmetatable(playerObject, Player)

    return playerObject

end

function Player:greetings()
    print(self.name, "says hi!", "They are a", self.age, "year old", self.gender)
end

function Player:damage(amount)
    if amount < 10 then
        print("Eh wasn't that bad", self.name, "only took", amount, "damage")
    else
        print("OH FUCK OWOWOWOW FUCK OW OW", self.name, "TOOK", amount, "DAMAGE OH FUCK")
    end

    self.health = self.health - amount
    if self.health <= 0 then
        print("Oh FUCK", self.name, "DIED OH NOOOO!!")
    end

end

function Player:revive()
    if self.health > 0 then
        print(self.name, "is already alive lol")
    else
        self.health = self.maxHealth
        print("Wow!", self.name, "was revived :D they now have", self.health, "health!")
    end

end
```
Now, let's see if you can tell what the following will do:
```lua
local kazi = Player.new("Kazi", "Male", 18, 100)

kazi:damage(5) -- What is the output here? (Q1)
kazi:damage(10) -- What is the output here? (Q2)
kazi:revive() -- What is the output here? (Q3)
kazi:damage(90) -- What is the output here? (Q4)
kazi:revive() -- What is the output here? (Q5)
```
<details>
<summary>Q1</summary>
"Eh wasn't that bad" "Kazi" "only took" 5 "damage"
</details>
<details>
<summary>Q2</summary>
"OH FUCK OWOWOWOW FUCK OW OW" "Kazi" "TOOK" 10 "DAMAGE OH FUCK"
</details>
<details>
<summary>Q3</summary>
"Kazi" "is already alive lol"
</details>
<details>
<summary>Q4</summary>
"OH FUCK OWOWOWOW FUCK OW OW" "Kazi" "TOOK" 90 "DAMAGE OH FUCK"<br>"Oh FUCK" "Kazi" "DIED OH NOOOO!!"
</details>
<details>
<summary>Q5</summary>
"Wow!" "Kazi" "was revived :D they now have" 100 "health!"
</details>

<hr>

Good job queens! We basically ready to move onto Roblox now, but I'll probably write a quick chapter with some tips for lua and coding in general.