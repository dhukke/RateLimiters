# RateLimiters

Playing around with dotnet 7 rate limter new functionality.

## Example

All the magic happens here:

```csharp
builder.Services.AddRateLimiter(
    rateLimiterOptions =>
    {
        rateLimiterOptions.RejectionStatusCode = StatusCodes.Status429TooManyRequests;

        rateLimiterOptions.AddPolicy(
            "fixed",
            httpContext => RateLimitPartition.GetFixedWindowLimiter(
                partitionKey: httpContext.User.Identity?.Name?.ToString(),
                factory: _ => new FixedWindowRateLimiterOptions
                {
                    PermitLimit = 3,
                    Window = TimeSpan.FromSeconds(10)
                }
            )
        );
    }
);
```

then we need to add it to an endpoint and add the rate limiter to the app:

```csharp
//mininal api endpoint
.RequireRateLimiting("fixed");

app.UseRateLimiter();
```

## References

<https://www.youtube.com/watch?v=PIfGHbvuAtM&ab_channel=MilanJovanovi%C4%87>
<https://www.youtube.com/watch?v=1tPVVDEDGtE&ab_channel=MilanJovanovi%C4%87>
<https://www.youtube.com/watch?v=bOfOo3Zsfx0&ab_channel=MohamadLawand>