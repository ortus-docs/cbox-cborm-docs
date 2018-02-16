A method expression is made up of the prefixes: `findBy, findAllBy, countBy` followed by the expression that combines a query upon one or more properties:

```javascript
User.findBy{property}[Conditional=equal][Operator]?{property}[Conditional][Operator]
User.findAllBy{property}[Conditional=equal][Operator]?{property}[Conditional][Operator]
User.countBy{property}[Conditional=equal][Operator]?{property}[Conditional][Operator]
```

If a conditional keyword is not passed, we assume you want equality. Remember that!

> **IMPORTANT** The ? means that you can concatenate the same pattern over and over again. 

## Conditionals

The available conditionals in ColdBox are:

* `LessThanEquals` - Less than or equal to passed value
* `LessThan` - Less than to passed value
* `GreaterThanEquals` - Greater than or equal to passed value
* `GreaterThan` - Greater than to passed value
* `Like` - Equivalent to the SQL like expression
* `NotEqual` - Not equal to the passed value
* `isNull` - The property must be null
* `isNotNull` - The property must not be null
* `NotBetween` - The property value must not be between two values
* `Between` - The property value must be between two values
* `NotInList` - The property value must not be in the passed in simple list or array
* `inList` - The property value must be in the passed in simple list or array

## Operators
The only valid operators are:

* `And`
* `Or`