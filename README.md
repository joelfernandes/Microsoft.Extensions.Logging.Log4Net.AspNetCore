# Microsoft.Extensions.Logging.Log4Net.AspNetCore

Allows to configure Log4net as Microsoft Extensions Logging handler on any ASP.NET Core application.

Thanks to [@anuraj](https://github.com/anuraj) for this [original blog post](https://dotnetthoughts.net/how-to-use-log4net-with-aspnetcore-for-logging/).

[![NuGet](https://img.shields.io/nuget/dt/Microsoft.Extensions.Logging.Log4net.AspNetCore.svg)](https://preview.nuget.org/packages/Microsoft.Extensions.Logging.Log4Net.AspNetCore/)

## Example of use

* Install the package or reference the project into your asp.net core application.

* Add the `AddLog4Net()` call into your `Configure` method of the `Startup` class.

```csharp
using Microsoft.Extensions.Logging;

public class Startup
{
    //...

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        //...

        loggerFactory.AddLog4Net(); // << Add this line
        app.UseMvc();

        //...
    }
}
```

* Add a `log4net.config` file with the content:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<log4net>
  <appender name="DebugAppender" type="log4net.Appender.DebugAppender" >
    <layout type="log4net.Layout.PatternLayout">
      <conversionPattern value="%date [%thread] %-5level %logger - %message%newline" />
    </layout>
  </appender>
  <root>
    <level value="ALL"/>
    <appender-ref ref="DebugAppender" />
  </root>
</log4net>
```

### Use custom exception logging formatter

If you want to manage explicitly your exception logging when you pass it as parameter, you should initialize the logger on your  `Startup` class as follows:


```csharp
using Microsoft.Extensions.Logging;

public class Startup
{
    //...

    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        //...

        loggerFactory.AddLog4Net((state, exception) => return "yourString");  // Change the lambda by your formatting exception method.
        app.UseMvc();

        //...
    }
}
```
