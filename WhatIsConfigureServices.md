---
title: "What is ConfigureServices?"
description: "Explain ConfigureSerices of .NET"
---

## `ConfigureServices` Method in .NET

### What is `ConfigureServices`?

`ConfigureServices` is a method in the `Startup` class of an ASP.NET Core application. It's used to configure the application's services, which are then available to be injected into other parts of the application via Dependency Injection (DI).

### How Does `ConfigureServices` Work?

`ConfigureServices` is called by the ASP.NET Core runtime when the application starts. It takes an `IServiceCollection` parameter named `services`. This parameter is used to register services that will be available throughout the application.

### Where Does the `services` Argument Come From?

The `services` argument is provided by the ASP.NET Core runtime. When the application starts, the runtime creates a default `IServiceCollection` instance and passes it to the `ConfigureServices` method. This collection is where you register services that you want to make available for Dependency Injection.

### How Does Dependency Injection Happen?

1. **Registration:** In the `ConfigureServices` method, you register services with the `IServiceCollection`. You specify the type of the service and its implementation, as well as the service's lifetime (Transient, Scoped, or Singleton).

2. **Injection:** When a component (like a controller or a service) needs a dependency, it specifies that dependency in its constructor. The ASP.NET Core DI container automatically resolves the dependency by providing an instance of the required service.

3. **Service Provider:** After all services are registered, the `IServiceCollection` is used to build a `ServiceProvider`, which is the DI container that the application uses to resolve dependencies at runtime.

### Example of `ConfigureServices`

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        // Register a transient service
        services.AddTransient<IMyTransientService, MyTransientService>();

        // Register a scoped service
        services.AddScoped<IMyScopedService, MyScopedService>();

        // Register a singleton service
        services.AddSingleton<IMySingletonService, MySingletonService>();

        // Register MVC services
        services.AddControllersWithViews();
    }

    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        // Middleware configuration
    }
}
```
### Breakdown of the Example

- **Transient**: A new instance is created every time the service is requested.
- **Scoped**: A new instance is created once per request.
- **Singleton**: A single instance is created and shared throughout the application's lifetime.

### How Does the Injection Work?

Consider a controller that needs to use `IMyTransientService`:

```csharp
public class MyController : Controller
{
    private readonly IMyTransientService _transientService;

    public MyController(IMyTransientService transientService)
    {
        _transientService = transientService;
    }

    public IActionResult Index()
    {
        // Use _transientService here
        return View();
    }
}
```
- **Constructor Injection** The `MyController` constructor requests an `IMyTransientService`. The DI (Dependency Injection) container checks its registrations, sees that `IMyTransientService` is registered as a transient service, and provides an instance of `MyTransientService`.

### Summary

- **ConfigureServices** is used to register services with the DI container.
- The **services** argument is an `IServiceCollection` provided by the ASP.NET Core runtime.
- **Dependency Injection** happens by resolving registered services when needed, based on their registration (Transient, Scoped, Singleton).
