# SQL Restrictions

SQL restrictions will allow you to add ad-hoc SQL to the criteria you are building.  Simple enough, but we have made great strides to make this look easy for a developer but behind the scenes we take each sql you pass and compile it so we can abstract all the native Java types for you.  This means that you can use it in a similar manner to `executeQuery()` in CFML.

## Method Signature

The method you will use for this restriction is `sql()` 

```javascript
/**
 * Use arbitrary SQL to modify the resultset
 *
 * @sql The sql to execute, it can contain parameters via positional `?` placeholders
 * @params This is an array of value definitions which need to be a struct of { value: , type: } or if the value is a simple value, we will try to infer it's type
 */
function sql( required string sql, array params=[] )
```

### Arguments

The method takes in two arguments:

* `sql` - The ad-hoc query to execute.  You can use positional parameters via the `?` placeholder.
* `params` - An array of parameters to bind the SQL with, can be simple values or a struct of a values and a supported type or a combination of both.

### SQL Params

The parameters you bind the SQL with can be of two types

1. Plain values.
2. Typed values

If you use plain values, then we will INFER the type from it, which is not as accurate as using a typed value. A typed value is a struct with the value and a valid type.

```javascript
c.sql( "isActive = true" );

// simple values
c.sql( "id = ?", [ 123 ] );
c.sql( "userName = ? and firstName like ?", [ "joe", "%joe%"] );

// strong typed values
c.sql( "id = ?", [ { value:123, type=c.TYPES.integer } ] );
c.sql( "isActive = ?", [ { value:true, type=c.TYPES.boolean } ] );
c.sql( "userName = ? and firstName like ?", [
    { value : "joe", type : "string" },
    { value : "%joe%", type : "string" }
] );
```

#### Valid Types

Below you can see the `TYPES` struct available in the criteria builder object which map the CFML types to the native Hibernate Types.

```javascript
this.TYPES = {
    "string"      : "StringType",
    "clob"        : "ClobType",
    "text"        : "TextType",
    "char"        : "ChareacterType",
    "boolean"     : "BooleanType",
    "yesno"       : "YesNoType",
    "truefalse"   : "TrueFalseType",
    "byte"        : "ByteType",
    "short"       : "ShortType",
    "integer"     : "IntegerType",
    "long"        : "LongType",
    "float"       : "FloatType",
    "double"      : "DoubleType",
    "bigInteger"  : "BigIntegerType",
    "bigDecimal"  : "BigDecimalType",
    "timestamp"   : "TimestampType",
    "time"        : "TimeType",
    "date"        : "DateType",
    "calendar"    : "CalendarType",
    "currency"    : "CurrencyType",
    "locale"      : "LocaleType",
    "timezone"    : "TimeZoneType",
    "url"         : "UrlType",
    "class"       : "ClassType",
    "blob"        : "BlobType",
    "binary"      : "BinaryType",
    "uuid"        : "UUIDCharType",
    "serializable": "SerializableType"
};
```

#### Inferred Types

The inferred types we infer are the following and in the following order.

1. Binary
2. Boolean
3. Time
4. Date
5. uuid
6. float
7. numeric
8. url
9. string
10. text



