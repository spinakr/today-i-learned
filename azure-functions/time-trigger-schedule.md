# Azure Functions timer triggered functions

Creating azurer functions triggered by a timer uses chron expressions to schedule time.

The following function is triggered 09.30 every day. Chron expressions follows the format `{second} {minute} {hour} {day} {month} {day-of-week}`. Where 0 symbols not running and * run at all occurences. By change the first zero in this example would have the function run every second between 09.30 and 09.31, while having a zero to the far left of an expression would have the trigger never run.


```csharp
public static class UpdateVinmonopoletRepo
{
    [FunctionName("UpdateVinmonopoletRepo")]

    public static void Run(
        [TimerTrigger("0 30 9 * * *", RunOnStartup=true)]TimerInfo myTimer, //9.30 every day
        [Table(Constants.TableNames.VinmonopoletWinesTableName)] CloudTable vinmonopoletRepo,
        TraceWriter log)
    {
        log.Info("UpateVinmonopoletRepo function trigger on a timed shcdule");


    }
}
```