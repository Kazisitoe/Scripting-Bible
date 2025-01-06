<style>

code {
  white-space : pre-wrap !important;
  word-break: break-word;
}

</style>

# State and Behavior, Object Oriented Programming

## 3b.0 - Tips!
Here's just some misc advice, concepts, and stuff about lua I couldn't really fit anywhere else.

<hr>

### 3b.1 - Lua Stuff
In lua, you can simplify ``x = x + 1`` as ``x += 1``. This applies for all operations.

There are ternary operators in lua. They look similar to if statements, but they are NOT if statements, and instead of running code, they just select values. All ternary operations can be represented with boolean operations (``and``/``or``) but they are typically easier to follow along:
```lua
local person = "Kazi"
local description = if person == "Kazi" then "So so pretty and cute the best #1 etc" elseif person == "Calla" then "Alright I guess..." else "Mid"
```
<details>
<summary>What is decription equal to?</summary>
"So so pretty and cute the best #1 etc"
</details>

<hr>

### 3b.2 Coding Tips
Don't use an if statement if you don't have to. Here's an example:
```lua
local person = "Kazi"
local cuteness
if person == "Kazi" then
    cuteness = 999999999
else
    cuteness = 0
end
```
While this does work, it runs slower and takes longer to write than doing:
```lua
local person = "Kazi"
local cuteness = person == "Kazi" and 999999999 or 0
```
or the equivilent with ternary operations. The practice of avoiding **branches**, of which the ``if`` statement is one of, is called **branchless programming**.

On the topic of branches, try to avoid ``if``-``else`` spam:
```lua
if ... then
elseif ... then
elseif ... then
elseif ... then
elseif ... then
elseif ... then
elseif ... then
else
end
```
This is some nasty yandev bs. Not only does it end up looking ugly as shit, it's also slow. And God forbid only the last condition is met or it reaches the ``else``, then it'd have checked every. single. condition before it. Which is slow as shit. If you're at this point, there's always 100% a better way to do it.

Some more advice with branches: it is often easier to read conditions and change their order if you use guard clauses. Guard clauses reimagine things from being "if this condition is met, then do this" to being "if this condition isn't met, then don't do anything. Otherwise, continue as usual":
```lua
local condition = true
local function doesUseGuardClause()
    if not condition then
        return -- Recall that returning will always just end a function right there
    end

    print("Printed!")

end
local function doesNotUseGuardClause()
    if condition then
        print("Printed!")
    end

end
```
By doing it this way, it's a lot easier to change the order of conditions and keep track of which conditions have been checked. This can be done within functions using ``return`` and loops using ``continue`` (``continue`` forcibly stops the rest of the loop from running and does a new iteration).

<hr>

Now we ready to get into ROBLOX!! WOOOHOO!!!!!