# Prompty - GitHub Copilot Custom Instructions

## Project Overview
Prompty is a production-ready Blazor Server application featuring GitHub OAuth authentication and repository management. The application is designed to provide a robust, secure, and maintainable foundation for building enterprise-grade web applications with .NET and Blazor.

## Technology Stack
- **Framework**: Blazor Server (ASP.NET Core)
- **Runtime**: .NET 8+ (or latest LTS)
- **Authentication**: GitHub OAuth
- **Frontend**: Blazor components with Razor syntax
- **Backend**: C# with ASP.NET Core services
- **Testing**: xUnit/NUnit with bUnit for component testing
- **Dependency Injection**: ASP.NET Core DI container

## C# Coding Conventions

### Naming Conventions
- Use PascalCase for classes, methods, properties, and public members
- Use camelCase for parameters, local variables, and private fields
- Prefix private fields with underscore (`_fieldName`)
- Use descriptive, intention-revealing names
- Avoid abbreviations unless widely accepted (e.g., `Url`, `Http`, `Id`)

### Code Style
- Group `using` directives with `System.*` namespaces first, then third-party, then project namespaces, all alphabetically sorted
- Use `var` only when the type is obvious from the right-hand side
- Keep lines under 120 characters
- Always use explicit braces `{}` even for single-statement blocks
- Place opening braces on new lines (Allman style)
- Use file-scoped namespaces where supported (C# 10+)
- Prefer expression-bodied members for simple properties and methods
- Use null-coalescing operators (`??`, `??=`) and null-conditional operators (`?.`, `?[]`) appropriately

### Modern C# Features
- Use records for immutable data transfer objects
- Prefer pattern matching over type checking with `is` and casts
- Use `async`/`await` consistently; never use `async void` except for event handlers
- Use `ValueTask<T>` for frequently called async methods that often complete synchronously
- Leverage init-only properties for immutable objects
- Use target-typed new expressions where appropriate
- Employ switch expressions for cleaner code

## Blazor-Specific Guidelines

### Component Structure
- Component files must match their class names (e.g., `MyComponent.razor`)
- Use `@code {}` blocks instead of deprecated `@functions {}`
- Organize components by feature/domain: `Pages/`, `Components/`, `Shared/`, `Layouts/`
- Split large components into smaller, reusable child components
- Consider using partial classes to separate complex logic from markup

### Component Lifecycle
- Understand and use lifecycle methods appropriately: `OnInitialized[Async]`, `OnParametersSet[Async]`, `OnAfterRender[Async]`
- Implement `IDisposable` or `IAsyncDisposable` to clean up resources, event handlers, and timers
- Use `ShouldRender()` to optimize rendering when necessary

### Data Binding and Parameters
- Never mutate `[Parameter]` properties directly in child components
- Use `[Parameter]` for input and `EventCallback<T>` for output/events
- Prefer `EventCallback<T>` over `Action` or `Func` delegates for component events
- Use two-way binding with `@bind` or `@bind-Value` appropriately
- Use `[CascadingParameter]` for passing data down component trees (e.g., auth state, theme, culture)

### Dependency Injection
- Use `@inject` directive for service injection in components
- Register services with appropriate lifetimes (Singleton, Scoped, Transient) in `Program.cs`
- Prefer constructor injection in C# classes
- Keep service dependencies minimal and focused

### Rendering and Performance
- Use `@key` directive in `@foreach` loops to maintain DOM element identity
- Minimize unnecessary re-renders by controlling state updates
- Avoid expensive operations in rendering logic
- Use virtualization (`Virtualize<T>` component) for large lists
- Prefer `RenderFragment` over `MarkupString` unless raw HTML is absolutely necessary
- Be cautious with JavaScript interop calls; batch when possible

### State Management
- Keep component state minimal and scoped appropriately
- Use services for shared state across components
- Consider state management patterns (MVVM, MVU) for complex applications
- Use `StateHasChanged()` explicitly only when necessary

## Authentication and Security

### GitHub OAuth Integration
- Never hardcode secrets, API keys, or OAuth client secrets
- Store sensitive configuration in user secrets (development), environment variables, or Azure Key Vault (production)
- Validate tokens server-side before trusting client-side authentication state
- Implement proper authorization policies using ASP.NET Core Authorization
- Use `[Authorize]` attribute to protect pages and components
- Leverage `AuthenticationStateProvider` for accessing user claims

### General Security Best Practices
- Always validate and sanitize user input on the server
- Never rely solely on client-side validation
- Use proper encoding when rendering user-generated content
- Implement CSRF protection (automatically handled by Blazor Server)
- Apply Content Security Policy (CSP) headers
- Keep dependencies updated to patch security vulnerabilities
- Use HTTPS in production environments
- Implement rate limiting for sensitive operations

## Error Handling

- Use structured exception handling with specific exception types
- Implement error boundaries in Blazor components (`ErrorBoundary` component)
- Log errors appropriately using `ILogger<T>`
- Provide user-friendly error messages while logging detailed errors
- Never expose sensitive information in error messages to users
- Handle async exceptions properly with try-catch in async methods

## Testing Guidelines

### Unit Testing
- Use xUnit or NUnit for backend services and logic
- Use bUnit for Blazor component testing
- Mock dependencies using Moq or NSubstitute
- Mock `IJSRuntime` for JavaScript interop in tests
- Write tests that are isolated, repeatable, and fast
- Follow AAA pattern: Arrange, Act, Assert
- Use meaningful test names that describe the scenario and expected outcome

### Integration Testing
- Test critical workflows end-to-end
- Use `WebApplicationFactory<TProgram>` for ASP.NET Core integration tests
- Test authentication and authorization flows
- Validate API integrations with GitHub

### Accessibility Testing
- Validate ARIA attributes and roles in components
- Test keyboard navigation
- Ensure proper semantic HTML structure

## Documentation Standards

- Use XML documentation comments (`///`) for public APIs, classes, and methods
- Include `<summary>`, `<param>`, and `<returns>` tags
- Document complex business logic and non-obvious code
- Keep README.md updated with setup instructions and architecture overview
- Document configuration settings and environment variables
- Maintain inline comments for complex algorithms or workarounds

## Build and Deployment

- Use .NET CLI commands for building (`dotnet build`) and running (`dotnet run`)
- Configure build settings in `.csproj` files
- Use Release configuration for production deployments
- Enable nullable reference types (`<Nullable>enable</Nullable>`)
- Target appropriate framework version in project file
- Include only necessary NuGet packages; remove unused references
- Use central package management for solution-wide version control

## Git and Version Control

- Write clear, concise commit messages in imperative mood
- Keep commits small and focused on single changes
- Use feature branches for new development
- Ensure code builds and tests pass before committing
- Add `.gitignore` entries for build artifacts, user-specific files, and secrets

## Repository Management Features

- Implement repository listing and management using GitHub API
- Use Octokit or GitHub GraphQL API for GitHub interactions
- Implement proper rate limiting and error handling for API calls
- Cache repository data appropriately to minimize API calls
- Handle pagination for large result sets

## Code Organization

- Group related components and services by feature
- Keep component files focused and under 300 lines when possible
- Extract complex logic to separate service classes
- Use partial classes for large component code-behind
- Maintain separation of concerns between UI, business logic, and data access

## Additional Best Practices

- Prefer composition over inheritance
- Follow SOLID principles
- Use dependency injection for loose coupling
- Implement logging strategically for debugging and monitoring
- Use configuration providers for environment-specific settings
- Leverage app settings validation on startup
- Handle disposable resources properly (using statements, IDisposable)
- Avoid blocking calls in async code paths; use async all the way
- Profile and optimize performance-critical paths
- Use DevTools and browser debugging features effectively

## Common Patterns to Follow

### Service Registration
```csharp
builder.Services.AddScoped<IRepositoryService, RepositoryService>();
builder.Services.AddHttpClient<IGitHubApiClient, GitHubApiClient>();
```

### Component Events
```csharp
[Parameter]
public EventCallback<string> OnItemSelected { get; set; }

private async Task HandleSelection(string item)
{
    await OnItemSelected.InvokeAsync(item);
}
```

### Async Patterns
```csharp
protected override async Task OnInitializedAsync()
{
    _repositories = await RepositoryService.GetRepositoriesAsync();
}
```

### Error Boundaries
```razor
<ErrorBoundary>
    <ChildContent>
        @* Component content *@
    </ChildContent>
    <ErrorContent>
        <p>An error occurred. Please try again.</p>
    </ErrorContent>
</ErrorBoundary>
```

## Helpful Resources
- [Blazor Documentation](https://learn.microsoft.com/aspnet/core/blazor/)
- [ASP.NET Core Documentation](https://learn.microsoft.com/aspnet/core/)
- [C# Coding Conventions](https://learn.microsoft.com/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [GitHub API Documentation](https://docs.github.com/rest)
- [bUnit Testing Documentation](https://bunit.dev/)
