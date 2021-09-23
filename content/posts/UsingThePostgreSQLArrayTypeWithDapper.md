---
title: "Using the PostgreSQL array type with Dapper"
date: 2021-09-23T21:00:00.6463905+00:00
draft: false
---
PostgreSQL has some fancy types like the array type. But its not obvious how to return e.g. an `int[]` or an `uuid[]` in a single row from Dapper.

In fact the first hit on [StackOverflow](https://stackoverflow.com/questions/62915756/how-to-deserialize-array-using-dapper/69179355#69179355) said it was impossible.

Not so... with a little insight, luck and duct tape you can do anything. It's actually quite easy.

If you use Dapper and NpgSql "out of the box" you will get an error "Invalid type owner for DynamicMethod" if you go for an array in your select like this:

```csharp
var arrayOfThreeElements =  
        (await _conn.QueryAsync<int[]>("SELECT ARRAY[1, 2, 3]"))                  
              .FirstOrDefault();
```

The only thing you need is a TypeHandler which you can use to tell Dapper how to handle the array type.

```csharp
public class GenericArrayHandler<T> : SqlMapper.TypeHandler<T[]>
{
    public override void SetValue(IDbDataParameter parameter, T[] value)
    {
        parameter.Value = value;
    }

    public override T[] Parse(object value) => (T[]) value;
}
```

I made it generic in ordet to work for all kinds of types.

Then you just add this handler in the Initialization code of your app or website like this:

```csharp
SqlMapper.AddTypeHandler(new GenericArrayHandler<int>());
```

Now the query works and returns the 3 elements, 1,2 and 3 in an integer array.

I have also tested the handler with an `uuid[]` which works just as fine:

```csharp
SqlMapper.AddTypeHandler(new GenericArrayHandler<Guid>());
```


