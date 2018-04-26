# Code Value Classes
When working with relational databases, lookup tables is a common pattern to represent e.g. states and enumerable types. To ensure [referential integrity](https://en.wikipedia.org/wiki/Referential_integrity) it is important to have such values represented in the database as well as in code. One way of doing this is with enumerables and a single column table in the database. A better and more rubest pattern is to use separate code value classes/structs to hold the values. Independant of the how this is done, it is important to have integration tests checking that all values exists in both code and in the database.

An example of a code table implementation can be implemented like the following:

```csharp
public class TaskType
{
    public string Code { get; }
    public string Description { get; }

    public static readonly TaskType Type1 = new TaskType(1, "This is a description of the task type, typically human readable");
    public static readonly TaskType Type2 = new TaskType(2, "This is a description of the task type, typically human readable");

    public static readonly TaskType[] All = { Type1, Type2 };

    private TaskType(string code, string description)
    {
        Code = code;
        Description = description;
    }

    public static TaskType FromCode(string code)
    {
        return All.Single(s => s.Code == code);
    }

    protected bool Equals(TaskType other)
    {
        return string.Equals(Code, other.Code);
    }

    public override bool Equals(object obj)
    {
        if (ReferenceEquals(null, obj)) return false;
        if (ReferenceEquals(this, obj)) return true;
        if (obj.GetType() != this.GetType()) return false;
        return Equals((TaskType)obj);
    }

    public override int GetHashCode()
    {
        return (Code != null ? Code.GetHashCode() : 0);
    }

    public override string ToString()
    {
        return Code;
    }
}
```

The code property of this class will typically be used as foreign key in the database model. When read into code the value will be used to instantiate the correct class member, providing description and type checking runtime. In a class representing a read model it ca nbe used as follows: 

```csharp
//...
[NotMapped]
public TaskType TaskType { get; private set; }

public string TaskTypeCode
{
    set => TaskType = TaskType.FromCode(value);
}
```

To ensure that no incompatible values are used, an integration test can be used to verify values in code vs values in the database:

```csharp
[TestFixture]
public class TaskTypeTests
{
    private List<TaskType> _dbCodes;

    [OneTimeSetUp]
    public async Task Setup()
    {
        using (var connection = ComponentsForTesting.GetDatabaseConnection())
        {
            _dbCodes = (await connection.QueryAsync<TaskType>("select Code, Description from no.TaskType")).ToList();
        }
    }

    [Test]
    public void All_Codes_Should_Have_Matching_Db_Value()
    {
        foreach (var taskTypeCode in TaskType.All)
        {
            _dbCodes.Any(c => c.Equals(taskTypeCode))
                .Should().BeTrue($"{taskTypeCode.Code} does not have a code value in the database and can thus not be used");
        }
    }

    [Test]
    public void All_Db_Values_Should_Have_Matching_Code()
    {
        var codesInCode = TaskType.All;

        foreach (var dbCode in _dbCodes)
        {
            codesInCode.Any(c => c.Equals(dbCode))
                .Should().BeTrue($"{dbCode.Code} does not have a code value defined in {nameof(TaskType)}, " +
                                  $"if you have deleted a code make sure to also migrate the database");
        }
    }
}
```


