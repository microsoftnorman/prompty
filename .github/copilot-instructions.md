# GitHub Copilot Instructions for Prompty

## Project Overview

Prompty is a production-ready Blazor Server application featuring GitHub OAuth authentication and repository management. This document provides guidelines for contributing to the project and using GitHub Copilot effectively.

## Ask Clarifying Questions

**Before building plans or implementing features, always ask clarifying questions to ensure you understand:**
- The specific requirements and constraints
- The expected behavior and edge cases
- Integration points with existing code
- Security implications, especially for OAuth and authentication flows
- Performance considerations for server-side Blazor

## Coding Standards and Style Guidelines

### C# and .NET Conventions
- Follow [Microsoft's C# Coding Conventions](https://docs.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- Use C# 10+ features where appropriate (file-scoped namespaces, global usings, etc.)
- Target .NET 6.0 or later
- Use `async`/`await` for all I/O operations
- Prefer dependency injection over static dependencies
- Use nullable reference types and handle null cases explicitly

### Naming Conventions
- **PascalCase** for classes, methods, properties, and public members
- **camelCase** for local variables and private fields (prefix private fields with `_`)
- **UPPER_CASE** for constants
- Use descriptive names that clearly indicate purpose

### Code Organization
- One class per file (except for small related classes)
- Group related functionality into logical folders
- Keep methods focused and under 50 lines when possible
- Extract complex logic into well-named private methods

## Blazor-Specific Guidelines

### Component Structure
- Place Blazor components in a `Components` or `Pages` directory
- Use `.razor` extension for components
- Separate code-behind logic into `.razor.cs` files for complex components
- Use partial classes for code-behind files

### Component Best Practices
- Use `@code` blocks for simple component logic
- Implement `IDisposable` when subscribing to events or using resources
- Use `StateHasChanged()` judiciously to trigger UI updates
- Avoid heavy computations in render methods
- Use `OnParametersSetAsync` for parameter validation and initialization

### State Management
- Use scoped services for component-level state
- Consider using cascading parameters for shared state
- Implement proper state management patterns for complex applications
- Be mindful of SignalR connection state in Blazor Server

## Authentication and Security

### GitHub OAuth
- Never commit client secrets or API keys
- Use ASP.NET Core's authentication middleware
- Validate OAuth callbacks properly
- Implement proper CSRF protection
- Use secure cookie settings for production

### Security Best Practices
- Validate all user inputs
- Use parameterized queries or EF Core to prevent SQL injection
- Implement proper authorization checks on all protected resources
- Use HTTPS only in production
- Implement rate limiting for API endpoints
- Follow OWASP guidelines for web application security

## Testing Requirements

### Unit Tests
- Write unit tests for business logic and services
- Use xUnit as the primary testing framework
- Mock external dependencies using Moq or NSubstitute
- Aim for 70%+ code coverage on critical paths
- Name tests clearly: `MethodName_Scenario_ExpectedBehavior`

### Integration Tests
- Test OAuth flows end-to-end
- Test database interactions with test databases
- Use `WebApplicationFactory` for integration testing Blazor Server apps
- Clean up test data after each test

### Test Organization
- Place tests in a separate test project (e.g., `Prompty.Tests`)
- Mirror the source project structure in test projects
- Use test fixtures for shared setup code

## Documentation Expectations

### Code Documentation
- Add XML documentation comments for public APIs
- Document complex algorithms or business rules
- Keep comments up-to-date with code changes
- Avoid obvious comments; code should be self-documenting when possible

### README and Docs
- Keep README.md current with setup instructions
- Document environment variables and configuration
- Provide examples for common use cases
- Document OAuth setup and GitHub App configuration

### API Documentation
- Document all API endpoints (if applicable)
- Include request/response examples
- Document error codes and their meanings
- Keep API versioning strategy documented

## Architecture Patterns

### Dependency Injection
- Register services in `Program.cs` or `Startup.cs`
- Use appropriate service lifetimes:
  - **Singleton**: For stateless services
  - **Scoped**: For per-request services (use carefully in Blazor Server)
  - **Transient**: For lightweight, stateless services

### Repository Pattern
- Use repository pattern for data access if needed
- Keep repositories focused on single entities
- Use generic repositories sparingly; prefer specific implementations

### Service Layer
- Implement business logic in service classes
- Keep controllers/components thin
- Services should be testable and independent of UI concerns

### Error Handling
- Use structured exception handling
- Log exceptions with appropriate severity levels
- Return user-friendly error messages
- Implement global error boundaries in Blazor

## GitHub Integration

### API Best Practices
- Use Octokit.NET for GitHub API interactions
- Implement proper rate limiting handling
- Cache GitHub API responses when appropriate
- Handle API errors gracefully

### Repository Management
- Follow GitHub API pagination for large result sets
- Use GraphQL API for complex queries
- Implement proper token refresh logic

## Performance Considerations

### Blazor Server Specific
- Be mindful of SignalR connection costs
- Minimize render overhead by using `ShouldRender()`
- Use virtualization for large lists
- Implement proper component disposal

### General Performance
- Use async streaming for large data sets
- Implement caching strategies for frequently accessed data
- Use `IMemoryCache` or distributed caching as appropriate
- Profile application regularly under realistic loads

## Development Workflow

### Version Control
- Use feature branches for new work
- Write clear, descriptive commit messages
- Keep commits focused and atomic
- Reference issue numbers in commit messages

### Code Review
- Request reviews for all changes
- Address review comments promptly
- Update documentation affected by changes
- Ensure CI/CD passes before merging

## Configuration Management

### appsettings.json
- Use configuration sections for related settings
- Provide example configuration in `appsettings.Development.json`
- Document all configuration options
- Use User Secrets for local development secrets

### Environment Variables
- Document required environment variables
- Use environment-specific configuration files
- Never commit production configuration values

## Logging

### Logging Standards
- Use `ILogger<T>` for all logging
- Use appropriate log levels:
  - **Trace**: Detailed internal flow
  - **Debug**: Development diagnostics
  - **Information**: General flow
  - **Warning**: Unexpected but handled issues
  - **Error**: Failures and exceptions
  - **Critical**: Application crashes
- Include relevant context in log messages
- Avoid logging sensitive information (tokens, passwords, PII)

## Additional Resources

- [Blazor Documentation](https://docs.microsoft.com/en-us/aspnet/core/blazor/)
- [GitHub API Documentation](https://docs.github.com/en/rest)
- [Octokit.NET Documentation](https://octokitnet.readthedocs.io/)
- [ASP.NET Core Security](https://docs.microsoft.com/en-us/aspnet/core/security/)

---

*These guidelines are living documentation. Update them as the project evolves and new patterns emerge.*
