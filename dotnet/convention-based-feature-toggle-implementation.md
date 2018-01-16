#Convention based feature toggle implementaion

Simple feature toggling implementation. Convention based on dervied type name. 

```C#
public class FeatureToggle
{
    public bool IsEnabled { get; }

    protected FeatureToggle()
    {
        var featureName = GetType().Name;
        bool.TryParse(ConfigurationManager.AppSettings[$"Feature.{featureName}"], out bool toggle);
        IsEnabled = toggle;
    }
}
```

### Example usage:

```C#
public class SomeFeature : FeatureToggle
{
}

public class Program 
{
    static void Main(string[] args)
    {
        if(new SomeFeature().IsEnabled)
        {
            NewFeaturedService.Call();
        }
        else
        {
            OldFeaturedService.Call();
        }
    }
}
```

This will look for a app config setting named Feature.SomeFeature, which can be set to true or false, if the setting is not found it will default to true.


