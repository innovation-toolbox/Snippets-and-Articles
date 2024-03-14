# Code pieces 

Program.cs : 
```csharp
using custom_indexing_api.Config;
using CustomIndexingApi.Config;
using CustomIndexingApi.Utils.CustomException;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;


var host = new HostBuilder()
.ConfigureFunctionsWorkerDefaults(builder =>
{
    builder.UseMiddleware<ExceptionLoggingMiddleware>();
})
.ConfigureServices((context, services) =>
{
    services.AddSingleton<CompletionService>();
    services.AddSingleton<Prompts>();
})
.Build();

host.Run();
```

Exception Handler :
```csharp
using Microsoft.Azure.Functions.Worker;
using Microsoft.Azure.Functions.Worker.Http;
using Microsoft.Azure.Functions.Worker.Middleware;
using Microsoft.Extensions.Logging;
using System;
using System.Net;
using System.Threading.Tasks;

namespace CustomIndexingApi.Utils.CustomException
{
    public class ExceptionLoggingMiddleware : IFunctionsWorkerMiddleware
    {
        public async Task Invoke(FunctionContext context, FunctionExecutionDelegate next)
        {
            try
            {
                await next(context);
            }
            catch (BadRequestException ex)
            {
                //From https://stackoverflow.com/questions/68350021/azure-function-middleware-how-to-return-a-custom-http-response
                var req = await context.GetHttpRequestDataAsync();
                var response = req!.CreateResponse();
                response.StatusCode = HttpStatusCode.BadRequest;
                response.Headers.Add("Content-Type", "application/json; charset=utf-8");
                response.WriteString(ex.Message);
                context.GetInvocationResult().Value = response;
            }
            catch (Exception ex)
            {
                var logger = context.GetLogger(context.FunctionDefinition.Name);
                logger.LogError("Unexpected Error in {0}: {1}", context.FunctionDefinition.Name, ex.Message);

            }
        }
    }
}
```
BadRequestException.cs :
```csharp
namespace CustomIndexingApi.Utils.CustomException
{
    public class BadRequestException : Exception
    {
        public BadRequestException() { }

        public BadRequestException(string message)
            : base(message) { }

        public BadRequestException(string message, Exception inner)
            : base(message, inner) { }
    }
}
```

Usage : 
```csharp
throw new BadRequestException("Please pass a valid request body");
```


# Resources 
1. [StackOverflow - Exception Handler Middleware](https://stackoverflow.com/questions/69855276/azure-net-5-isolated-functions-using-middleware-to-catch-exceptions)
1. [StackOverflow - Exception Handler Middleware 2](https://stackoverflow.com/questions/68350021/azure-function-middleware-how-to-return-a-custom-http-response)