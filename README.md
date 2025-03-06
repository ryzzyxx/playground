.Net rules for AI
1. Validate and Sanitize All User Input
Rule:
Use model validation attributes to enforce proper input formats and prevent malicious data from propagating through your application.
Example:
csharpCopypublic class UserInputModel {
    [Required]
    [StringLength(100, MinimumLength = 3)]
    public string Name { get; set; }
        [Required]
    [EmailAddress]
    public string Email { get; set; }
}
 
2. Prevent SQL Injection with Parameterized Queries or ORM
Rule:
Always use parameterized queries or an ORM like Entity Framework to interact with your database, which avoids SQL injection vulnerabilities.
Example (Entity Framework):
csharpCopyvar user = await _context.Users.FirstOrDefaultAsync(u => u.Email == email);
3. Use Anti-Forgery Tokens for CSRF Protection
Rule:
Apply anti-forgery tokens in your Razor forms and validate them in your controller actions to guard against CSRF attacks.
Example (Controller):
csharpCopy[HttpPost]
[ValidateAntiForgeryToken]public IActionResult Submit(UserInputModel model){
    // Process form data securelyreturn RedirectToAction("Index");
}
 
Example (Razor View):
htmlCopy<form asp-action="Submit" method="post">    @Html.AntiForgeryToken()    <!-- form fields --></form>
4. Enforce HTTPS and HSTS
Rule:
Configure middleware to ensure that your application only communicates over HTTPS, and leverage HSTS to enforce this policy.
Example (Startup.cs):
csharpCopypublic void Configure(IApplicationBuilder app, IWebHostEnvironment env){
    app.UseHttpsRedirection();    app.UseHsts();
    // Other middleware registrations
}
 
5. Secure Cookies with Proper Flags
Rule:
Configure cookies to use HttpOnly, Secure, and SameSite attributes to prevent client-side access and cross-site attacks.
Example (Startup.cs):
csharpCopyservices.Configure<CookiePolicyOptions>(options =>
{
    options.HttpOnly = Microsoft.AspNetCore.CookiePolicy.HttpOnlyPolicy.Always;
    options.Secure = CookieSecurePolicy.Always;
    options.MinimumSameSitePolicy = SameSiteMode.Strict;
});
6. Implement Centralized Exception Handling
Rule:
Use exception-handling middleware to catch unhandled exceptions, log them securely, and return generic error messages without exposing sensitive details.
Example (Startup.cs):
csharpCopypublic void Configure(IApplicationBuilder app, IWebHostEnvironment env){
    app.UseExceptionHandler("/Home/Error");
    // Custom error handling or logging can be implemented here// Other middleware registrations
}
 
7. Leverage Razor’s Automatic HTML Encoding
Rule:
Rely on Razor’s built-in HTML encoding to prevent Cross-Site Scripting (XSS) by ensuring that all dynamic content is safely encoded.
Example (Razor View):
htmlCopy<p>@Model.UserInput</p>  <!-- Content is automatically HTML-encoded -->
8. Secure API Endpoints with Authentication and Authorization
Rule:
Protect your API endpoints using ASP.NET Core Identity or token-based authentication, and secure access with appropriate authorization attributes.
Example:
csharpCopy[Authorize]
[ApiController]
[Route("api/[controller]")]public class SecureDataController : ControllerBase{
    // API actions here
}
 
9. Avoid Insecure Deserialization
Rule:
Never deserialize untrusted data without validation. Use safe serialization libraries (like System.Text.Json) with properly configured options to prevent deserialization attacks.
Example:
csharpCopyvar options = new JsonSerializerOptions {
    PropertyNameCaseInsensitive = true,
    // Configure other safe settings as needed
};
var data = JsonSerializer.Deserialize<MyModel>(jsonString, options);
 
10. Use Dependency Injection Correctly
Rule:
Avoid using static instances for services that hold sensitive information. Leverage the built-in dependency injection container to manage service lifetimes securely.
Example (Startup.cs):
csharpCopypublic void ConfigureServices(IServiceCollection services){
    services.AddScoped<IMyService, MyService>();    // Other service registrations
}
 
