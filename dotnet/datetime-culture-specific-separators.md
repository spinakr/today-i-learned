# DateTime Culture Specific Separators

When using the `DateTime.ToString()`-method with a specific formatting string, `:` `/` are not formatted as the visual characters, but as what is called 'cultur specific time/date separators'. This is easy to forget which might lead to different formats than expected.

Let us say you want to print a string on a specific format to fulfill some external integration requirement, e.g:

```csharp
var date = new DateTime(1991, 9, 16, 10, 0, 0);
var formattedDateString = date.ToString("yyyy/MM/dd HH:mm:ss");
```

One could think that this code guarantees that the formatted string will be on the specified format. Unfortunately that is not the case - `/` and `:` will instead be replaced by the culture specified separators, e.g. possibly `-` and `.` respectively.

To avoid this one could either specify `CultureInfo.InvariantCulture` in the `ToString()`-method, or escape the separators as follows:

```csharp
var date = new DateTime(1991, 9, 16, 10, 0, 0);
var formattedDateString = date.ToString("yyyy'/'MM'/'dd HH':'mm':'ss");
```


- [https://stackoverflow.com/questions/9488764/c-sharp-formatting-datetime-giving-me-wrong-result](https://stackoverflow.com/questions/9488764/c-sharp-formatting-datetime-giving-me-wrong-result)
- [https://docs.microsoft.com/en-us/dotnet/standard/base-types/custom-date-and-time-format-strings](https://docs.microsoft.com/en-us/dotnet/standard/base-types/custom-date-and-time-format-strings)