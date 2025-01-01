<style>

code {
  white-space : pre-wrap !important;
  word-break: break-word;
}

</style>

# Basics

## 1.0 - Basic CS and Programming Concepts
While you don't really *need* to know CS in order to code, it's very useful to understand the way your code works in the first place so you can better decide the best way to do what you want to do. Probably the most important things to have an understanding of is basic terminology and also how **memory** works.

Examples for these concepts will be provided in pseudo-code (not actually real code) since this doesn't necessarily have to do with coding itself and is universal to any coding language.

<hr>

### 1.1 - Terminology
- **Programming language:** A language used to write programs (wow)
    - **Compiled language:** A programming language that is made to be directly ran by the computer itself; instructions for the computer
    - **Interpreted language:** Also called a scripting language. A programming language that is made to be run by a separate program (the interpreter); instructions for another program
- **High/low level language:** The further a language is away from directly interacting with the computer, the higher level it is. Scripting languages are generally high level since they interact with a program that interacts with the computer instead of just interacting with the computer
- **Variable:** Something that stores a value, concept from algebra
- **Data types:** The different types of information that can be stored in a variable. These can be simple values like numbers, letters, and words (primitive data types) or more complicated structures
    - **Complex data type:** A data type that holds multiple values within it
- **Function:** Something that (generally) can take inputs (parameters), run some code, and return outputs, concept from algebra
    - **Void function:** A function that doesn't give an output
- **Bit:** The smallest unit of stored information; a single binary digit (0 or 1). Most values are encoded as multiple bits, usually powers of 2.

There is some more terminology but that will be explained within the rest of the doc sections!!

<hr>

### 1.2 - Memory Basics
Memory is what a computer uses to store information. All information is stored as binary values, which basically every language will automatically do for you. Say you were to have some code like this:
```
x = 10
```
In this example, ``x`` is a variable set to the value ``10``. Wow! When this is executed, some memory is **allocated** (reserved) for ``x``, and is filled in with the binary value for ``10``. Wherever this memory is allocated is called the **memory address**. Variables generally can fill their allocated memory with a new value, allowing it to change over time:
```
x = 10
x = 20
x = 5
```

Generally, variables will always allocate their own memory, so if you set one variable to be equal to another variable:
```
x = 10
y = x
```
then they will both have different memory addresses. This means that if ``x`` were to be changed to a different value in this example:
```
x = 10
y = x
x = 5
```
then ``y`` would still remain ``10`` even though it was set to ``x`` which is now ``5``; instead of copying ``x`` itself it copies whatever ``x`` is currently equal to. There are, however, **reference** variables which do refer to the exact memory address of another variable, effectively making it so it changes with it:
```
x = 10
y references x
x = 5
```
in this example, ``y``, like ``x``, will start out as ``10`` and be changed to ``5``. Most higher-level languages will not use references unless it is for more complicated data types than just numbers.

Memory is physically stored, so there is a limit to how much can be stored. You will generally never hit this limit on a modern computer unless you REALLY fuck up, but generally the less you have available the slower everything is, so you want to avoid keeping useless memory allocated. When useless memory is kept allocated, it is called a **memory leak**. Scripting languages typically have **garbage collectors** that detect when memory is 100% not used and won't be used and automatically deallocates it, doing most of the job for you.

<hr>

### 1.3 - Data Types
Data is always stored in the same binary format, so the computer uses its data type to know how to understand it. Different data types will require more or less memory to store information. The most common data types are:

- **Int(integer):** A positive or negative whole number
- **Float(ing point):** A number that can have a decimal
    - **Double:** A float that uses twice the information, so it can store bigger numbers
- **Bool(ean):** A binary value, usually represented as being either ``true`` or ``false``
- **Str(ing):** Any text/characters. Typically represented by wrapping it between ``""`` or ``''``. Strings might sometimes look like other value types (e.g. ``"true"`` or ``"5"``) but they are treated just as any other string
- **Null:** Called ``nil`` in lua, just represents there being no value at all

A language that is **strongly typed** will require you to manually define the type of a variable. Many scripting languages aren't, and instead automatically assume it based on the value.

There are more complex data types, but these generally depend on the language itself. The most common one that is practically universal between languages is the **array**. An array is essentially an ordered list of multiple values. Most languages are **0-index**, meaning that the first value stored in an array is treated like the 0th value, but Lua (what Roblox uses) is **1-index**, meaning that the first value is treated as the 1st. Here is an example of the way an array works in a 1-index language:
```
a = [5, "something", false]

x = a[1]
y = a[2]
z = a[3]
```
In this example, ``a`` is set to an array containing the values ``5`` (an int), ``"something"`` (a string), and ``false`` (a bool). ``x`` is then set to the ``1``st element of the array (``5``), ``y`` to the ``2``nd (``"something"``), ``z`` to the ``3``rd (``false``). This process of referring to a specific value within a complex data structure is called **indexing**.

<hr>

### 1.4 - Functions
A function can generally take in any number of inputs (parameters) and return any number of outputs. Functions can also store code within them that runs whenver the function is called (used). Whatever values are passed into the function (arguments) will fill in the values of the parameters. Might not have been the simplest way to explain it but an example should clear stuff up (using Lua for the example this time):
```lua hl_lines="12"
-- In lua, doing -- makes the rest of the line a comment. You can write anything in a comment and it won't run
function add(number1, number2) -- All the code between here and 'end' will run when the funciton is called
    -- The parameters are 'number1' and 'number2'

    local sum = number1 + number2 -- 'local' basically means in Lua that this is a new variable and not just changing the value of one that already exists
    return sum -- Returning values ends the function immedeately and gives it a value

end

local x = add(5, 7) -- The arguments here are 5 and 7. This means that when 'add' is called this time, the parameter number1 will be 5 and the parameter number2 will be 7. This will make the sum variable 5+7 and thus 12 will be returned. x is set to the return value, so x is now 12

local y = add(8, 9) - 1 -- Figure out what the arguments are, what 'sum' will be equal to in this case, what gets returned, and what y is equal to`
```
<details>
<summary>The arguments are:</summary>
8 and 9, meaning that when add is called in the highlighted line, number1 will equal 8 and number2 will equal 9

</details>

<details>
<summary>The value returned is:</summary>
17. sum is set to equal number1 (8) + number2 (9) which is 17. sum is then returned
</details>

<details>
<summary>y will be equal to:</summary>
16. y is set to equal the return value of add(8, 9) - 1. The return value is 17, and 17 - 1 is 16.
</details>

Most programming languages come with many built-in functions. The ``print`` function will log any arguments into the program's output.
```lua
print("something", 5-9, false)
```
**Output**:
> "something"  -14  false

<hr>

### 1.5 - Operations
Operations are pretty simple. The symbols for operations are almost entirely the same between languages, but there are some differences and I will be using the way they are done in lua. In lua, the value types **number** (int, float, and double are all treated as one type in lua), **string**, and **boolean** all have unique operations, and there are some operations that are universal to all data types. They are all generally straightforwards, although boolean operations are somewhat difficult.

**Universal operations:**

- Equality: ``a == b`` outputs ``true`` if the values are equivilent and ``false`` otherwise
    - ``5 == 3`` outputs ``false``
- Unequality: ``a ~= b`` outputs ``true`` if the values are not the same and ``false`` otherwise
    - ``5 ~= 3`` outputs ``true``
- If these are used with complex data types, it will compare their memory addresses instead of the values themselves

**Numerical operations (arithmetic):**

- Addition: ``a + b`` outputs the sum of the two numbers
    - ``5 + 3`` outputs ``8``
- Subtraction: ``a - b`` outputs the difference of the two numbers
    - ``5 - 3`` outputs ``2``
- Multiplication: ``a * b`` outputs the product of the two numbers
    - ``5 * 3`` outputs ``15``
- Division: ``a / b`` outputs the quotient of the two numbers
    - ``5 / 3`` outputs ``1.66666666666...``
- Exponentiation: ``a ^ b`` outputs ``a`` to the power of ``b``
    - ``5 ^ 3`` outputs ``125``

These all follow the standard order of operations. Parentheses can be used.

**Numerical operations (Inequalities):**

- Greater than or equal to: ``a >= b`` outputs ``true`` if ``a`` is greater than or equal to ``b`` and ``false`` otherwise
    - ``5 >= 3`` outputs ``true``
- Less than or equal to: ``a <= b`` outputs ``true`` if ``a`` is less than or equal to ``b`` and ``false`` otherwise
    - ``5 <= 3`` outputs ``false``

**String operations (there's really only one):**

- Concatination: ``a .. b`` combines the two strings into one
    - ``"combined ".."words"`` outputs ``"combined words"``

### 1.5.1 - Boolean Operations
These get their own sections because they're kind of complicated. Boolean operations can actually be used on any data type, not just booleans. However, they are based on boolean math and treat data types as if they were either ``true`` or ``false`` even if they are not booleans. If a data type is treated as being ``true`` it is **truthy** and if it is treated as being false it is **falsey**. In lua, all data types are truthy except ``false`` and ``nil``.

- ``a and b`` outputs ``b`` if ``a`` is truthy, otherwise outputs ``a``
    - ``true and 5`` outputs ``5``
    - ``false and 3`` outputs ``false``
    - ``nil and false`` outputs ``nil``
- ``a or b`` outputs ``a`` if ``a`` is truthy, otherwise outputs ``b``
    - ``true or 5`` outputs ``true``
    - ``false or 3`` outputs ``3``
    - ``nil or false`` outputs ``false``
- ``not a`` outputs ``false`` if ``a`` is truthy, otherwise outputs ``true``
    - ``not true`` output ``false``
    - ``not 5`` outputs ``false``
    - ``not nil`` outputs `true`
    - ``not false`` outputs ``true``
    - ``not not 2`` outputs ``true``

Now try to see if you can figure out these values!

<details>
<summary>5 and 3</summary>
3
</details>
<details>
<summary>2 or 3</summary>
2
</details>
<details>
<summary>5*5 or 1</summary>
25
</details>
<details>
<summary>nil and 2*2</summary>
nil
</details>
<details>
<summary>true and "wow" or "woah"</summary>
"wow"
</details>
<details>
<summary>(not true) and "wow" or (not not "woah")</summary>
true
</details>
<details>
<summary>5 == 3 and "calla is mid" or 3 == 2 and "what" or 2*2 == 2^2 and "kazi is so pretty"</summary>
"kazi is so pretty"
</details>

<hr>

That's it for all the computer science/math based stuff! Wow! You did it! Yay! More coming soon!