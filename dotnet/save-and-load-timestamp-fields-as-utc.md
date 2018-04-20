# Saving and loading timestamp fields as utc

When working with timestamps or other time relatev properties which might be used in compare operations, it is important to ensure correct date time kind (typically always utc). 

Consider the following write model, inserting a timestamp into the database. 

Note that the fields has Utc in the name, this is important, since the read model must know this to correctly handle it when reading and parsing it.

```csharp
public class MyObject 
{
    public MyObject()
    {
        CreatedUtc = DateTime.UtcNow;
    }

    public DateTime CreatedUtc { get; }
}

```

When reading the date, we can set the datetime kind in the property setter to ensure correct handling. 

```csharp
public class MyObject 
{
    private DateTime _createdUtc;

    public DateTime CreatedUtc
    {
        get => _createdUtc;
        set => _createdUtc = value.SetUtcKind();
    }

    public static DateTime SetUtcKind(this DateTime utcDateTime)
    {
        if (utcDateTime.Kind != DateTimeKind.Unspecified)
        {
            throw  new InvalidOperationException("Conversion method can only be used with DateTimeKind.Unspecified");
        }

        return new DateTime(utcDateTime.Year, utcDateTime.Month, utcDateTime.Day, utcDateTime.Hour, utcDateTime.Minute, 
                            utcDateTime.Second, utcDateTime.Millisecond,DateTimeKind.Utc);
    }
}
```
