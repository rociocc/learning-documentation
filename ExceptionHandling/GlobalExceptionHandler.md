### Resources:
- https://www.milanjovanovic.tech/blog/global-error-handling-in-aspnetcore-8

# Exception Handling

-  There are 2 ways to implement Exception Handling in ASP.NET Core:
    1. Using Middleware
    2. Using IExceptionHandler
       
           internal sealed class GlobalExceptionHandler : IExceptionHandler
           {
                private readonly ILogger<GlobalExceptionHandler> _logger;
            
                public GlobalExceptionHandler(ILogger<GlobalExceptionHandler> logger)
                {
                    _logger = logger;
                }
            
                public async ValueTask<bool> TryHandleAsync(
                    HttpContext httpContext,
                    Exception exception,
                    CancellationToken cancellationToken)
                {
                    _logger.LogError(
                        exception, "Exception occurred: {Message}", exception.Message);
            
                    var problemDetails = new ProblemDetails
                    {
                        Status = StatusCodes.Status500InternalServerError,
                        Title = "Server error"
                    };
            
                    httpContext.Response.StatusCode = problemDetails.Status.Value;
            
                    await httpContext.Response
                        .WriteAsJsonAsync(problemDetails, cancellationToken);
            
                    return true;
                }
           }

       - Add an IExceptionHandler implementation to the ASP.NET Core Request pipeline:
         1. Register the IExceptionHandler service with dependency injection

                // call the AddExceptionHandler method to register the GlobalExceptionHandler as a service
                builder.Services.AddExceptionHandler<GlobalExceptionHandler>();
                // calling AddProblemDetails to generate a Problem Details response for common exceptions.
                builder.Services.AddProblemDetails();
            
         3. Register the ExceptionHandlerMiddleware with the request pipeline

                // call UseExceptionHandler to add the ExceptionHandlerMiddleware to the request pipeline
                app.UseExceptionHandler();
