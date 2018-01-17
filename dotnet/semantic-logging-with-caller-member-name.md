# Semantic logging with "CallerMemberName"

Semantic logging is a structured way of logging events from your application. Instead of writing a more or less random message somewhere, we create strongly typed events for things worthy of logging. ``typeof`` and ``CallerMemberName`` or usefull in creating consistent log events.

```C#
class MysimpleService
{
    ILogEventSource logSource = new LogSource();
    public void DoSomeWork()
    {
        try
        {
            var resultId = SomeOtherService.HelpMeDoThisThing();
            logSource.RequestSentToTheOtherService<MySimpleService>(resultId);
        }
        catch(Exception e)
        {
            logSource.RequestToOtherServiceFailed<MySimpleService>(e);
            throw;
        }
    }
}
```

This looks all logical, we write a log when the work is done, and another log if it fails. We include useful information in the log method as well as the type of the class doing the work.
The implementation of the logger can look as follows:

```C#
class LogSource : ILogEventSource
{
    private ILogger LogContext<T>(string callerMember, [CallerMemberName] string logMethodName="")
    {
        return Log
            .ForContext("CallerType", typeof(T).FullName)
            .ForContext("EventName", logMethodName)
            .ForContext("CallerMember", callerMember);

    }

    public RequestSentToTheOtherService void<T>(Guid resultId, [CallerMemberName]string callerMember = "")
    {
            LogContext<T>(callerMember)
                .Information("Successfull request to other service, with id {ResultId}", resultId);
    }
}
```
Note the use of ``[CallerMemberName]``, this will make sure the name of the caller method will be included in the log method automatically, some with the log method name itself.