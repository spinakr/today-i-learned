# Fluent Builder Pattern in C#
Using contructor paramets to initialize classes typically makes code unreadable e.g:

```csharp
Person person = new Person("Anders", "andy", 20);
```

As long as objects are small this might work, but as number of properties increase, this way of initializing objects quickly becomes impossible to handle. This can be solved using a builder class an implicit conversion operands.

```csharp
public class PersonBuilder 
{
    private string Name;
    private string Nickname;
    private int Age;

    public PersonBuilder WithName(string name)
    {
        this.Name = name;
        return this;
    }

    public PersonBuilder WithName(string nickname)
    {
        this.Nickname = nickname;
        return this;
    }
    
    public PersonBuilder WithAge(int age)
    {
        this.Age = age;
        return this;
    }

    //Implicit conversion operand
    public static implicit operator Person(PersonBuilder builder)
    {
        return new Person(builder.Name, builder.Nickname, builder.Age);
    }
}
```

This class can then be used to initialize objects in a more fluent and readable fashion:

```csharp
Person person = new PersonBuilder()
    .WithName("Anders")
    .WithNickane("andy")
    .WithAge(20);
```

The implicit conversion operand is called when an implicit cast from PersonBuilder is executed. 

### `implicit` vs `explicit` operator
When marking an casting operator as implicit it should be "safe" to use and never lose information, as it will be called automatically without explicitly asking for it. If the operation can't be desing in a safe manner, use explicit instead, this way the user will have to explicitly invoke it via casting. 