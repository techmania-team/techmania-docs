Applies to version: 2.2 (Theme API version 3)

# A C# programmer's quick guide to Lua

This article aims to cover the minimal amount of Lua knowledge required to develop TECHMANIA themes. If you are already familiar with Lua, you don't need to read this article. It will also not contain anything specific to TECHMANIA's Theme API.

## General

Lua is a light-weight, interpreted language, meaning you don't need to compile your code.

Lua is fundamentally not an object-oriented language, but there are features and syntaxes that imitate one. More on this later.

You don't need to write semicolons at the end of statements.

Lua comments begin with `--` instead of `//`. Wrap multi-line comments between `--[[` and `]]`.

## Types

There are a total of 8 types in Lua:

- **Nil**: similar with C#'s null. The value `nil` is of this type.
- **Boolean**: `true` or `false`.
- **Number**: similar with C#'s double. There is no integer type in Lua.
- **String**: you can call the built-in function `tostring(value)` to convert values of other types to strings.
- **Function**: similar with C#'s Action and Func – values of this type can be assigned to variables.
- **Userdata**: since Lua is often embedded into another language (the host language), when the host language exposes an object to Lua, it will be of type userdata from Lua's perspective. It can be used like a table.
- **Thread**: not used in TECHMANIA themes.
- **Table**: like a Dictionary, except each key and each value doesn't have to be the same type within a table.

Table is the most powerful and versatile type in Lua. If keys are positive integers, a table can be used like a 1-indexed array. If keys are strings, and values are booleans, number, strings, functions and other tables, a table can be used like an object.

## Variables

By default, all variables are in the global scope – even those declared in a block or function. The only exception is that function parameters and loop variables are local to the function and loop. In other cases, make a habit to declare variables with the `local` modifier to limit their scope.

Also, variables don't have types; only values do. Therefore you don't need to specify a type when declaring a variable.

```
globalVariable = "foo"
local localVariable = 42
```

## Operators

- Logical operators are `and`, `or` and `not`, instead of `&&`, `||` and `!`.
- Inequality is `~=` instead of `!=`. [[1]](#footnotes)
- String concatenation is `..` instead of `+`.
- There is no `++` or `--`.
- There is no `+=`, `-=` and the like.

## Control statements and functions

```
variable, ..., variable = expression, ..., expression
```
You can assign multiple variables in one statement.

```
if exp then
...
end
```
You don't need to write parentheses around the condition expression, but it's not an error to write them either.

```
if exp then
...
else
...
end
```

```
if exp then
...
elseif exp then
...
end
```
Notice the lack of space between "else" and "if".

```
while exp do
...
end
```

```
for variable = first, last, step do
...
end
```

```
for index, value in ipairs(table) do
...
end
```

```
for key, value in pairs(table) do
...
end
```

Note that there is `break`, but no `continue`, in loops. Refer to the [Common table operations](#common-table-operations) section for examples on traversing tables.

```
function name(parameter, ..., parameter)
...
end
```

Note that functions can return multiple values. If you call a function with fewer arguments than it has parameters, the unassigned parameters will get the value `nil`.

To define a function as a value in a table:

```
tableName.key = function() ... end
-- or --
tableName = {
  key = function() ... end
}
```

## Tables vs objects

As mentioned earlier, if a Lua table's keys are strings, and its values are booleans, numbers, strings, functions and other tables, it can be used like an object:

```
table = {}
table["fieldName"] = "foo"
table["methodName"] = function() ... end
table["methodName"]()
```

Lua has a syntatic sugar where `table.key` is the same as `table["key"]`, allowing these statements to resemble objects even more:

```
table = {}
table.fieldName = "foo"
table.methodName = function() ... end
table.methodName()
```

At this point functions in a table are unable to access other values in the same table. This can be solved by adding a `self` parameter:

```
table = {}
table.fieldName = "foo"
table.methodName = function(self)
  self.fieldName = "bar"
end
table.methodName(table)
```

And yes there is another syntactic sugar for that, in the form of colons:

```
table = {}
table.fieldName = "foo"
function table:methodName()
  self.fieldName = "bar"
end
table:methodName()
```

For more information on how to write object-oriented code in Lua, including classes and inheritance, refer to: https://www.lua.org/pil/16.html

## Common table operations

These operations allow you to use a table like a dictionary:

```
local tableLiteral = { foo = 1, bar = 2 }  -- keys are "foo" and "bar"

local demoTable = {}
-- Uninitialized values are nil
print(demoTable["foo"])
-- To set value at a specific key
demoTable["foo"] = 123
demoTable.bar = 456  -- syntactic sugar for ["bar"]
-- To remove a key
demoTable.baz = nil
-- To test the existence of a key
if (demoTable["baz"] ~= nil) then ... end

-- To iterate over all pairs, regardless of key type
-- Output:
-- foo 123
-- bar 456
--
-- Note that nil values are skipped
for key, value in pairs(demoTable) do
  print(key .. " " .. value)
end
```

These operations allow you to use a table like an array:

```
local arrayLiteral = { 101, 102, 103 }  -- keys are 1, 2 and 3

local demoArray = {}
-- To set value at the next available positive integer
table.insert(demoArray, 123)  -- key is 1
table.insert(demoArray, 456)  -- key is 2
-- To get length, use #, but it will only count consecutive positive integer keys
print(#demoArray)  -- length is 2

-- To iterate over positive integer keys
-- Output:
-- 1 123
-- 2 456
for index, value in ipairs(demoArray) do
  print(index .. " " .. value)
end
```

## Footnotes

[1] Lua manual says `~=` but somehow `!=` works just fine in TECHMANIA themes. I don't know why.
