<style>

code {
  white-space : pre-wrap !important;
  word-break: break-word;
}

</style>

# Conditionals and Iterations

## 2.0 - Conditionals and Iterations in Lua
**Conditionals** are fundamental for programming. They allow your code to do different things depending on the situation. **Iteration** is fundamental for automating things and making code less repetitive. Wow!

<hr>

### 2.1 - Conditionals
A conditional is something that runs code only if a certain condition is met. The most basic conditional is the ``if`` statement:
```lua
if (condition) then
    -- run something
end
```
The ``if`` statement evaluates a single condition. If whatever is put into it is truthy, then the code inside of it (between ``then`` and ``end``) will run. Otherwise, it skips over to ``end``. For example:
```lua
local age = 16

if age < 18 then
    print("Roblox devs want you")
end
```
Here, ``age < 18`` gives the value ``true``. Since ``true`` is truthy, the code inside of it runs. There are two extra conditionals that function within an ``if`` statement: ``elseif`` and ``else``:
```lua
if (condition1) then
    print("condition1 is truthy, only this will run")
elseif (condition2) then
    print("condition1 is falsey but condition2 is truthy! Only this will run")
elseif (condition3) then
    print("condition1 and condition2 are falsey but condition3 is truthy! Only this will run")
else
    print("Every above condition was falsey, so only this will run")
end
```

<hr>

### 2.2 - Iteration
There are many ways to iterate in lua. Iteration is the process of repeating an instruction multiple times, a single repetition being called an iteration. It can be achieved with **loops** and **recursive functions**. Both are fundamentally based on conditionals. When iterating, it is important to make sure that the iteration eventually ends or pauses, otherwise it will cause a **stack overflow** error where it essentially tries and fails to do an infinite amount of tasks at once. For most of the following examples, the methods for iteration will count upwards from ``1`` and stop when it reaches ``10``.

### 2.3 - Recursive Functions
A recursive function is a function that calls itself within its own execution:
```lua
local function countToTen(currentNumber)
    print("Counted", currentNumber)
    
    if currentNumber < 10 then
        countToTen(currentNumber + 1)
    end

end

countToTen(1)
```
The function will first on the last line run with the argument ``1``, meaning that ``currentNumber`` will equal ``1``. It will then print ``"Counted" 1`` and check if ``currentNumber < 10``. As ``1 < 10``, this will result in ``true``, and thus the code inside of the ``if`` statement runs. It calls itself again with the argument ``currentNumber + 1``, which would be ``1 + 1``, meaning this time ``currentNumber`` will be ``2``. It will keep doing this over and over again until the condition checked by the ``if`` statement is false, which is when the argument is ``10``. This will result in the following output:
> "Counted" 1
> "Counted" 2
> "Counted" 3
> "Counted" 4
> "Counted" 5
> "Counted" 6
> "Counted" 7
> "Counted" 8
> "Counted" 9
> "Counted" 10

<hr>

### While Loop
Generally, for something as simple as this, a loop is prefered. The ``while`` loop repeatedly evaluates a condition and runs code until thaat condition is falsey:
```lua
local currentNumber = 1
while currentNumber <= 10 do
    print("Counted", currentNumber)

    currentNumber = currentNumber + 1

end
```
The way this code works is that it first evaluates ``currentNumber <= 10``. Since that is ``true`` and thus truthy, the code inside the ``while`` loop runs. By the end of this, ``currentNumber`` becomes ``2``. As ``2`` is still less than or equal to ``10``, the code inside the ``while`` loop will run again. It will keep doing this until ``currentNumber`` becomes ``11``, and as ``11 < 10`` would give false, the code inside the while loop would stop running. It is important to note that this uses ``<= 10`` whereas the recursive function uses ``< 10`` as ``currentNumber`` is increased *before* it checks its value ``while`` loop while it's effectively increased *after* it checks its value.

Loops can prematurely be ended (similar to ``return`` in a function) using ``break``:
```lua
local currentNumber = 1
while true do -- This condition will always be truthy, so the while loop will theoretically run infinitely. However, we eventually break out of it, so it does not cause a stack overflow
    print("Counted", currentNumber)

    if currentNumber >= 10 then
        break
    end

    currentNumber = currentNumber + 1

end
```
The way the loop works this time is that it will firstly *always* run, as the condition we gave it is forever just ``true``. It will then evaluate ``currentNumber >= 10`` *within* the loop. As ``1 >= 10`` is ``false`` which is falsey, what's inside of the ``if`` statement won't run and it will not break. ``currentNumber`` then is increased and the loop runs again. It will keep doing this until ``currentNumber >= 10`` is ``true``, where it will then finally break, ending before ``currentNumber`` can be set to 11.

<hr>

### For Loop
There are two types of ``for`` loops in Lua, one that simply counts a variable upwards until it reaches a certain value (exactly what we want for this) called the **numeric for**, and one that uses an **iterator function** (this one's more complicated) called the **generic for**. The first type works like this:
```lua
for currentNumber = 1, 10, 1 do --1, 10, 1 means start at 1, stop at 10, increase by 1 every iteration. If a third number is not provided, it will automatically assume it is 1
    print("Counted", currentNumber)
end
```
This code will create a ``currentNumber`` variable that only exists within the ``for`` loop and then sets it to ``1``. It will then run the code within it, and increase it by ``1``. It will keep doing this until it reaches ``10``, where it will then iterate one final time. Like ``while`` loops, ``for`` loops can be prematurely ended with ``break``.

### Generic For Loop
The second type of ``for`` loop is definitely the most complicated method of iterating. The easiest way to do it is by analyzing an example:
```lua
local function iterator(limit, currentValue)
	currentValue = currentValue and currentValue + 1 or 1
	
	if currentValue > limit then
		return nil
	end
	
	return currentValue
	
end

for currentNumber in iterator, 10 do
	print("Counted", currentNumber)
end
```
Now we gotta break down how this all works. So, firstly, we can tell that this is a generic ``for`` as it uses ``in`` and an iterator function. The iteration function will then be called, and the arguments passed will be whatever is to the right of the indicator (``10``) and whatever is returned by the previous iteration (since there is no previous iteration, it will just be ``nil``). This means in the first iteration, it calls ``iterator(10, nil)``. The first line of the iterator function changes ``currentValue`` to be ``currentValue and currentValue + 1 or 1``. Substituting in its value for this iteration gives us ``nil and nil + 1 or 1`` (which can be read as ``(nil and nil + 1) or 1``). This simplifies to ``nil or 1`` which gives ``1``. It then checks ``currentValue > limit``, which would be ``1 > 10``, which is false, meaning the ``if`` won't run. Finally, it returns ``currentValue`` which is again, now ``1``. This return value gets put into ``currentNumber``, and then the contents of the for loop run, printing out ``"Counted" 1``. The for loop then iterates again, using the previous iteration's output (``1``) as an additional argument for ``iterator``, meaning that this time it now calls ``iterator(10, 1)``. This means that this time ``limit`` is still ``10`` and ``currentValue`` is now 1, resulting in ``currentValue and currentValue + 1 or 1`` giving ``1 and 1 + 1 or 1``, or ``1 and 2 or 1``, finally giving 2. The condition for the ``if`` statement still is not met, meaning that it returns ``currentValue``, which is now ``2``. This gets fed into ``currentNumber``, it runs the code inside the ``for`` loop again, and this keeps repeating until ``iterator`` eventually returns nil.

<br>

That was a lot of information wow. Good job princesses!