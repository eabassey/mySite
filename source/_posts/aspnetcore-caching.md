---
title: "Caching in ASP.NET CORE"
catalog: true
date: 2017-12-27 12:22:16
subtitle:
header-img:
tags:
- C#
- ASP.NET CORE
catagories:
- C#
- .NET

---
ASP.NET CORE is the new baby in Microsoft's house. This framework has come a long way and is very promising. Traditional ASP.NET is fully featured but that also comes with a lot of noise that makes it hard for Microsoft to target it as their devine framework for building microservices. The world as changed and we are all going to microservices.

## The Basics
---
Caching is a necessity if one is building a system that scales. Fetching data from the database everytime a request is made is very inefficient and will not meet the expectations the world has on us as developers. This is partly due to the fact that everyone is using mobile devices and requests need to be lean.

Caching, therefore, involves keeping a copy of data in a place that can be accessed easily and quickly than going to the database directly, mostly for reads.

## InMemory Cache
---
Distributed caching technologies like Redis give us peace of mind but ASP.NET CORE itself comes with an `IMemoryCache interface`, which represents a cache stored in the memory of the local web server.

An in-memory cache is stored in the memory of a single server hosting an ASP.NET app. If an app is hosted by multiple servers in a web farm or cloud hosting environment, the servers may have different values in their local in-memory caches. Apps that will be hosted in server farms or on cloud hosting should use a distributed cache to avoid cache consistency problems.

## Configuration
---
First step is to add nuget dependencies:

```JSON
    "dependencies": {
    "Microsoft.AspNetCore.Server.IISIntegration": "1.0.0-rc2-final",
    "Microsoft.AspNetCore.Server.Kestrel": "1.0.0-rc2-final",
    "Microsoft.Extensions.Caching.Memory": "1.0.0-rc2-final",
    "Microsoft.Extensions.Logging": "1.0.0-rc2-final",
    "Microsoft.Extensions.Logging.Console": "1.0.0-rc2-final"
  },
```

The caching service itself is a `middleware`, so one has to add it to the service collectin in `ConfigureServices` method in StartUp.cs as so:

```C#
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMemoryCache();
    }
```

To use the caching interface, request an instance of `IMemoryCache` in a controller or middleware constructor. Example:

```C#
    public GreetingMiddleware(RequestDelegate next,
    IMemoryCache memoryCache,
    ILogger<GreetingMiddleware> logger,
    IGreetingService greetingService)
    {
        _next = next;
        _memoryCache = memoryCache;
        _greetingService = greetingService;
        _logger = logger;
    }
```

## Reading and Writing to the Cache
---
Basically, there are two methods for working with caching layer. A `Get` and a `Set`. It doesn't come as a surprise right?

```C#
public Task Invoke(HttpContext httpContext)
{
    string cacheKey = "GreetingMiddleware-Invoke";
    string greeting;

    // try to get the cached item; null if not found
    // greeting = _memoryCache.Get(cacheKey) as string;

    // alternately, TryGet returns true if the cache entry was found
    if(!_memoryCache.TryGetValue(cacheKey, out greeting))
    {
        // fetch the value from the source
        greeting = _greetingService.Greet("world");

        // store in the cache
        _memoryCache.Set(cacheKey, greeting,
            new MemoryCacheEntryOptions()
            .SetAbsoluteExpiration(TimeSpan.FromMinutes(1)));
        _logger.LogInformation($"{cacheKey} updated from source.");
    }
    else
    {
        _logger.LogInformation($"{cacheKey} retrieved from cache.");
    }

    return httpContext.Response.WriteAsync(greeting);
}
```
