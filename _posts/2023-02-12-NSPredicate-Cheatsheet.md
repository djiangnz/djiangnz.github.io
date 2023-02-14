---
layout: post
title: NSPredicate Cheatsheet
description:
summary:
tags: cheatsheet iOS
---

`public convenience init(format predicateFormat: String, _ args: CVarArg...)`

## Format String Summary

> e.g. `NSPredicate(format: "attributeName == %@", "a name")`

|                          |                                                                     |
| ------------------------ | ------------------------------------------------------------------- |
| `"name == %@"`           | object’s name is equal to value passed in                           |
| `"%K == %@"`             | pass a string variable to `%K`, it will be represented as a keypath |
| `"%name IN $NAME_LIST"`  | checks if the value of key name is in `$NAME_LIST`                  |
| `"'name' IN $NAME_LIST"` | checks if the constant value `name` is in `$NAME_LIST`              |

## Keypath collection queries

> e.g. `NSPredicate(format: "expenses.@avg.doubleValue < 200")`

|          |                                                                     |
| -------- | ------------------------------------------------------------------- |
| `@avg`   | the average of the objects in the collection as an NSNumber         |
| `@count` | the number of objects in a collection as an NSNumber                |
| `@min`   | the minimum value of the objects in the collection as an NSNumber   |
| `@max`   | the maximum value of the objects in the collection as an NSNumber   |
| `@sum`   | the sum of the objects in the collection @sum based on the property |

## Basic comparisons

> e.g. `NSPredicate(format: "expenses BETWEEN {200, 400}")`

|           |                                                                        |
| --------- | ---------------------------------------------------------------------- |
| `=,==`    | Left hand expression is equal to right hand expression                 |
| `>=,=>`   | Left hand expression is greater than or equal to right hand expression |
| `<=,=<`   | Left hand expression is less than or equal to right hand expression    |
| `>`       | Left hand expression is greater than right hand expression             |
| `<`       | Left hand expression is less than right hand expression                |
| `!=,<>`   | Left hand expression is not equal to right hand expression             |
| `IN`      | i.e. `name IN {'Milk', 'Eggs', 'Bread'}`                               |
| `BETWEEN` | i.e. `expenses Between {0, 33}`. 0 or 33 it would also make this true  |

## Basic Compound Predicates

> e.g. `NSPredicate(format: "age == 40 AND price > 67")`

- `AND, &&`
- `OR, ||`
- `NOT, !`

## String Comparison Operators

> `NSPredicate(format: "name CONTAINS[c] 'cancel'")` The [c] part means case-insensitive

> e.g. `NSPredicate(format: "name BEGINS WITH 'm'")`

|               |                                                            |
| ------------- | ---------------------------------------------------------- |
| `BEGINS WITH` | Left hand expression begins with the right hand expression |
| `CONTAINS`    | Left hand expression contains the right hand expression    |
| `ENDSWITH`    | Left hand expression ends with the right hand expression   |
| `LIKE`        | `?` matches `1` character and `_` matches `0+` characters  |
| `MATCHES`     | `NSPredicate(format: "SELF MATCHES %@", "^\\S{233}+$")`    |

## Aggregate Operators

> e.g. `NSPredicate(format: "ALL expenses > 1000")`

- `ANY,SOME`
- `ALL`
- `NONE`

## Reference

- [realm.io](https://academy.realm.io/posts/nspredicate-cheatsheet/)
