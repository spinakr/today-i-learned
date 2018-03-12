# Url re-write rules to co-host api and web app

When developing web applications a common setup will contain a API and a front-end. These can be hosted separately or together. If iis is used as web server, this can be done by using the url rewrite module and configuring rules in Web.config of the ASP.NET web project. 

A typical rewrite rule setup can look as follows, from the `<system.webServer>`-node of the Web.config.

```xml
<rewrite>
    <rules>
        <rule name="Match all urls that are not directed to static content directory or api base route,
                    and redirect to location of index.html">
            <match url="^(.*)"/>
            <conditions logicalGrouping="MatchAll">
                <add input="{REQUEST_FILENAME}" matchType="Pattern" pattern=".*\.ico$" negate="true"/>
                <add input="{REQUEST_URI}" pattern="^/MyApp.Web/static" negate="true"/>
                <add input="{REQUEST_URI}" pattern="^/MyApp.Web/api/.*" negate="true"/>
            </conditions>
            <action type="Rewrite" url="/Arronax.Web/app/build/"/>
        </rule>
        <rule name="Match static content in web app and rewrite to actual location">
            <match url="(.*)"/>
            <conditions logicalGrouping="MatchAny">
                <add input="{REQUEST_FILENAME}" matchType="Pattern" pattern=".*\.ico$"/>
                <add input="{REQUEST_URI}" pattern="^/MyApp.Web/static"/>
            </conditions>
            <action type="Rewrite" url="/MyApp.Web/app/build/{R:1}"/>
        </rule>
    </rules>
</rewrite>
```

Here any request to locations other than `/api` or `/static` will be redicreted to the location of `index.html`. Then requests to `/static` are redirected to to directory where the fron-end app are located.

This requires that all api actions have a common base route `/api/xxx`
