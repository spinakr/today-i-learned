# Http client should be shared

HttpClient should not be wrapped in a using statement.

While bein disposable, http client is actually reentrant and thread safe by itself. After using a HttpClient and disposing it windows will keep the connection alive in the `TIME_WAIT`-state, which means that the connection was closed on the client side, but kept alive, to see if any packets are received or has to be retried etc. The connection will stay in this state for 240 seconds, effectively exhausting the connection pool. 


HttpClient should thus be used as follows:

```C#
private static HttpClient Client = new HttpClient();
public static async Task Main(string[] args) 
{
    for(int i = 0; i<10; i++)
    {
        var result = await Client.GetAsync("http://dotnet.net");
        Console.WriteLine(result.StatusCode);
    }
}

```


*not* like this:
```C#
for(int i = 0; i<10; i++)
{
    using(var client = new HttpClient())
    {
        var result = await client.GetAsync("http://dotnet.net");
        Console.WriteLine(result.StatusCode);
    }
}


```


Reference: 

- [YOU'RE USING HTTPCLIENT WRONG AND IT IS DESTABILIZING YOUR SOFTWARE](https://aspnetmonsters.com/2016/08/2016-08-27-httpclientwrong/)

