# Windsor container in Owin Web Api




### Composite root class, replacing controller activator

In `Startup.cs` configure http controller activator to use windsor composite root instead:

```csharp
var config = new HttpConfiguration();
config.Services.Replace(typeof(IHttpControllerActivator),
    new WindsorCompositionRoot(_container));
```

```csharp
public class WindsorCompositionRoot : IHttpControllerActivator
{
    private readonly IWindsorContainer _container;

    public WindsorCompositionRoot(WindsorContainer container)
    {
        _container = container;
    }

    public IHttpController Create(HttpRequestMessage request, HttpControllerDescriptor controllerDescriptor, Type controllerType)
    {
        var controller = (IHttpController)_container.Resolve(controllerType);

        request.RegisterForDispose(new Release(() => _container.Release(controller)));

        return controller;
    }

    private class Release : IDisposable
    {
        private readonly Action _release;

        public Release(Action release)
        {
            _release = release;
        }

        public void Dispose()
        {
            _release();
        }
    }
}
```