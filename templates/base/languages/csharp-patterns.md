# C# / .NET Coding Patterns

**Purpose:** Language-specific coding patterns for C# and .NET projects.

---

## Code Style

### Naming Conventions

**Classes, Methods, Properties, Events:**
```csharp
// PascalCase
public class UserService { }
public void GetUserById(int id) { }
public string UserName { get; set; }
public event EventHandler DataLoaded;
```

**Local Variables, Parameters:**
```csharp
// camelCase
int userId = 123;
string userName = "John";

public void ProcessData(string inputData, int maxLength)
{
    var result = inputData.Substring(0, maxLength);
}
```

**Private Fields:**
```csharp
// _camelCase (leading underscore)
private string _userName;
private readonly IUserService _userService;
```

**Constants:**
```csharp
// PascalCase
public const int MaxRetries = 3;
private const string ApiBaseUrl = "https://api.example.com";
```

**Interfaces:**
```csharp
// IPascalCase (I prefix)
public interface IUserService { }
public interface IDataRepository { }
```

### Indentation & Formatting
**Standard:** 4 spaces (not tabs)

**Braces:** K&R style (opening brace on same line)
```csharp
public void MyMethod() {
    if (condition) {
        // Code
    }
}
```

**Or Allman style (opening brace on new line):**
```csharp
public void MyMethod()
{
    if (condition)
    {
        // Code
    }
}
```

---

## ASP.NET Core Patterns

### Project Structure
```
MyApp/
├── Controllers/        # API controllers
├── Services/          # Business logic
├── Repositories/      # Data access
├── Models/            # Domain models
├── DTOs/              # Data Transfer Objects
├── Middleware/        # Request pipeline
├── Extensions/        # Helper extensions
├── Program.cs         # Entry point
└── appsettings.json   # Configuration
```

### Controller
```csharp
// Controllers/UsersController.cs
using Microsoft.AspNetCore.Mvc;
using MyApp.Services;
using MyApp.DTOs;

namespace MyApp.Controllers
{
    [ApiController]
    [Route("api/[controller]")]
    public class UsersController : ControllerBase
    {
        private readonly IUserService _userService;
        private readonly ILogger<UsersController> _logger;

        public UsersController(IUserService userService, ILogger<UsersController> logger)
        {
            _userService = userService;
            _logger = logger;
        }

        [HttpGet]
        public async Task<ActionResult<IEnumerable<UserDto>>> GetUsers()
        {
            try
            {
                var users = await _userService.GetAllAsync();
                return Ok(users);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex, "Error retrieving users");
                return StatusCode(500, "Internal server error");
            }
        }

        [HttpGet("{id}")]
        public async Task<ActionResult<UserDto>> GetUser(int id)
        {
            var user = await _userService.GetByIdAsync(id);
            if (user == null)
            {
                return NotFound();
            }
            return Ok(user);
        }

        [HttpPost]
        public async Task<ActionResult<UserDto>> CreateUser([FromBody] CreateUserDto userDto)
        {
            if (!ModelState.IsValid)
            {
                return BadRequest(ModelState);
            }

            var user = await _userService.CreateAsync(userDto);
            return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
        }
    }
}
```

### Service Layer
```csharp
// Services/IUserService.cs
using MyApp.DTOs;

namespace MyApp.Services
{
    public interface IUserService
    {
        Task<IEnumerable<UserDto>> GetAllAsync();
        Task<UserDto?> GetByIdAsync(int id);
        Task<UserDto> CreateAsync(CreateUserDto userDto);
        Task<UserDto?> UpdateAsync(int id, UpdateUserDto userDto);
        Task<bool> DeleteAsync(int id);
    }
}

// Services/UserService.cs
using MyApp.Repositories;
using MyApp.Models;
using MyApp.DTOs;

namespace MyApp.Services
{
    public class UserService : IUserService
    {
        private readonly IUserRepository _userRepository;
        private readonly ILogger<UserService> _logger;

        public UserService(IUserRepository userRepository, ILogger<UserService> logger)
        {
            _userRepository = userRepository;
            _logger = logger;
        }

        public async Task<IEnumerable<UserDto>> GetAllAsync()
        {
            var users = await _userRepository.GetAllAsync();
            return users.Select(u => u.ToDto());
        }

        public async Task<UserDto?> GetByIdAsync(int id)
        {
            var user = await _userRepository.GetByIdAsync(id);
            return user?.ToDto();
        }

        public async Task<UserDto> CreateAsync(CreateUserDto userDto)
        {
            var user = new User
            {
                Email = userDto.Email,
                Name = userDto.Name,
                Role = userDto.Role ?? "user",
                CreatedAt = DateTime.UtcNow
            };

            await _userRepository.AddAsync(user);
            await _userRepository.SaveChangesAsync();

            _logger.LogInformation("Created user {UserId}", user.Id);
            return user.ToDto();
        }
    }
}
```

### Repository Pattern
```csharp
// Repositories/IUserRepository.cs
using MyApp.Models;

namespace MyApp.Repositories
{
    public interface IUserRepository
    {
        Task<IEnumerable<User>> GetAllAsync();
        Task<User?> GetByIdAsync(int id);
        Task AddAsync(User user);
        Task UpdateAsync(User user);
        Task DeleteAsync(int id);
        Task SaveChangesAsync();
    }
}

// Repositories/UserRepository.cs
using Microsoft.EntityFrameworkCore;
using MyApp.Data;
using MyApp.Models;

namespace MyApp.Repositories
{
    public class UserRepository : IUserRepository
    {
        private readonly ApplicationDbContext _context;

        public UserRepository(ApplicationDbContext context)
        {
            _context = context;
        }

        public async Task<IEnumerable<User>> GetAllAsync()
        {
            return await _context.Users.ToListAsync();
        }

        public async Task<User?> GetByIdAsync(int id)
        {
            return await _context.Users.FindAsync(id);
        }

        public async Task AddAsync(User user)
        {
            await _context.Users.AddAsync(user);
        }

        public async Task UpdateAsync(User user)
        {
            _context.Users.Update(user);
        }

        public async Task DeleteAsync(int id)
        {
            var user = await GetByIdAsync(id);
            if (user != null)
            {
                _context.Users.Remove(user);
            }
        }

        public async Task SaveChangesAsync()
        {
            await _context.SaveChangesAsync();
        }
    }
}
```

---

## Models & DTOs

### Domain Model
```csharp
// Models/User.cs
using System.ComponentModel.DataAnnotations;

namespace MyApp.Models
{
    public class User
    {
        public int Id { get; set; }

        [Required]
        [EmailAddress]
        public string Email { get; set; } = string.Empty;

        [Required]
        [MinLength(2)]
        [MaxLength(100)]
        public string Name { get; set; } = string.Empty;

        public string Role { get; set; } = "user";

        public bool IsActive { get; set; } = true;

        public DateTime CreatedAt { get; set; }

        public DateTime? UpdatedAt { get; set; }

        // Navigation properties
        public ICollection<Order> Orders { get; set; } = new List<Order>();
    }
}
```

### Data Transfer Objects (DTOs)
```csharp
// DTOs/UserDto.cs
namespace MyApp.DTOs
{
    public record UserDto
    {
        public int Id { get; init; }
        public string Email { get; init; } = string.Empty;
        public string Name { get; init; } = string.Empty;
        public string Role { get; init; } = string.Empty;
        public bool IsActive { get; init; }
        public DateTime CreatedAt { get; init; }
    }

    public record CreateUserDto
    {
        [Required]
        [EmailAddress]
        public string Email { get; init; } = string.Empty;

        [Required]
        [MinLength(2)]
        [MaxLength(100)]
        public string Name { get; init; } = string.Empty;

        [MinLength(8)]
        public string Password { get; init; } = string.Empty;

        public string? Role { get; init; }
    }

    public record UpdateUserDto
    {
        [EmailAddress]
        public string? Email { get; init; }

        [MinLength(2)]
        [MaxLength(100)]
        public string? Name { get; init; }

        public string? Role { get; init; }

        public bool? IsActive { get; init; }
    }
}
```

### Mapping Extensions
```csharp
// Extensions/UserExtensions.cs
using MyApp.Models;
using MyApp.DTOs;

namespace MyApp.Extensions
{
    public static class UserExtensions
    {
        public static UserDto ToDto(this User user)
        {
            return new UserDto
            {
                Id = user.Id,
                Email = user.Email,
                Name = user.Name,
                Role = user.Role,
                IsActive = user.IsActive,
                CreatedAt = user.CreatedAt
            };
        }
    }
}
```

---

## Entity Framework Core

### DbContext
```csharp
// Data/ApplicationDbContext.cs
using Microsoft.EntityFrameworkCore;
using MyApp.Models;

namespace MyApp.Data
{
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        public DbSet<User> Users { get; set; }
        public DbSet<Order> Orders { get; set; }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            base.OnModelCreating(modelBuilder);

            modelBuilder.Entity<User>(entity =>
            {
                entity.ToTable("users");
                entity.HasKey(e => e.Id);
                entity.HasIndex(e => e.Email).IsUnique();

                entity.Property(e => e.Email).IsRequired().HasMaxLength(255);
                entity.Property(e => e.Name).IsRequired().HasMaxLength(100);
                entity.Property(e => e.Role).HasMaxLength(20).HasDefaultValue("user");
                entity.Property(e => e.CreatedAt).HasDefaultValueSql("GETUTCDATE()");
            });
        }
    }
}
```

---

## Async/Await Patterns

### Async Methods
```csharp
// Suffix async methods with "Async"
public async Task<User> GetUserAsync(int id)
{
    return await _context.Users.FindAsync(id);
}

// ConfigureAwait(false) for library code
public async Task<string> FetchDataAsync(string url)
{
    using var client = new HttpClient();
    var response = await client.GetAsync(url).ConfigureAwait(false);
    return await response.Content.ReadAsStringAsync().ConfigureAwait(false);
}
```

### Parallel Operations
```csharp
public async Task<IEnumerable<UserDto>> GetMultipleUsersAsync(int[] userIds)
{
    var tasks = userIds.Select(id => _userService.GetByIdAsync(id));
    var users = await Task.WhenAll(tasks);
    return users.Where(u => u != null).Cast<UserDto>();
}
```

---

## Error Handling

### Custom Exceptions
```csharp
// Exceptions/NotFoundException.cs
namespace MyApp.Exceptions
{
    public class NotFoundException : Exception
    {
        public NotFoundException(string message) : base(message)
        {
        }

        public NotFoundException(string entity, int id)
            : base($"{entity} with ID {id} not found")
        {
        }
    }

    public class ValidationException : Exception
    {
        public ValidationException(string message) : base(message)
        {
        }

        public IDictionary<string, string[]> Errors { get; set; } = new Dictionary<string, string[]>();
    }
}
```

### Global Exception Handler
```csharp
// Middleware/ExceptionHandlerMiddleware.cs
using System.Net;
using System.Text.Json;
using MyApp.Exceptions;

namespace MyApp.Middleware
{
    public class ExceptionHandlerMiddleware
    {
        private readonly RequestDelegate _next;
        private readonly ILogger<ExceptionHandlerMiddleware> _logger;

        public ExceptionHandlerMiddleware(RequestDelegate next, ILogger<ExceptionHandlerMiddleware> logger)
        {
            _next = next;
            _logger = logger;
        }

        public async Task InvokeAsync(HttpContext context)
        {
            try
            {
                await _next(context);
            }
            catch (Exception ex)
            {
                await HandleExceptionAsync(context, ex);
            }
        }

        private async Task HandleExceptionAsync(HttpContext context, Exception exception)
        {
            context.Response.ContentType = "application/json";

            var (statusCode, message) = exception switch
            {
                NotFoundException => ((int)HttpStatusCode.NotFound, exception.Message),
                ValidationException => ((int)HttpStatusCode.BadRequest, exception.Message),
                _ => ((int)HttpStatusCode.InternalServerError, "Internal server error")
            };

            context.Response.StatusCode = statusCode;

            _logger.LogError(exception, "Error occurred: {Message}", exception.Message);

            var response = new
            {
                error = new
                {
                    message,
                    statusCode
                }
            };

            await context.Response.WriteAsync(JsonSerializer.Serialize(response));
        }
    }
}
```

---

## Dependency Injection

### Registration in Program.cs
```csharp
// Program.cs
using Microsoft.EntityFrameworkCore;
using MyApp.Data;
using MyApp.Services;
using MyApp.Repositories;
using MyApp.Middleware;

var builder = WebApplication.CreateBuilder(args);

// Add services to DI container
builder.Services.AddControllers();
builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

// Register services
builder.Services.AddScoped<IUserService, UserService>();
builder.Services.AddScoped<IUserRepository, UserRepository>();

// Add logging
builder.Services.AddLogging();

var app = builder.Build();

// Configure middleware pipeline
app.UseMiddleware<ExceptionHandlerMiddleware>();
app.UseHttpsRedirection();
app.UseAuthorization();
app.MapControllers();

app.Run();
```

---

## Testing (xUnit)

### Unit Test Example
```csharp
// Tests/Services/UserServiceTests.cs
using Xunit;
using Moq;
using MyApp.Services;
using MyApp.Repositories;
using MyApp.Models;
using MyApp.DTOs;

namespace MyApp.Tests.Services
{
    public class UserServiceTests
    {
        private readonly Mock<IUserRepository> _mockRepository;
        private readonly Mock<ILogger<UserService>> _mockLogger;
        private readonly UserService _service;

        public UserServiceTests()
        {
            _mockRepository = new Mock<IUserRepository>();
            _mockLogger = new Mock<ILogger<UserService>>();
            _service = new UserService(_mockRepository.Object, _mockLogger.Object);
        }

        [Fact]
        public async Task GetByIdAsync_ExistingUser_ReturnsUser()
        {
            // Arrange
            var userId = 1;
            var user = new User { Id = userId, Email = "test@example.com", Name = "Test User" };
            _mockRepository.Setup(r => r.GetByIdAsync(userId)).ReturnsAsync(user);

            // Act
            var result = await _service.GetByIdAsync(userId);

            // Assert
            Assert.NotNull(result);
            Assert.Equal(userId, result.Id);
            Assert.Equal("test@example.com", result.Email);
        }

        [Fact]
        public async Task GetByIdAsync_NonExistingUser_ReturnsNull()
        {
            // Arrange
            _mockRepository.Setup(r => r.GetByIdAsync(It.IsAny<int>())).ReturnsAsync((User?)null);

            // Act
            var result = await _service.GetByIdAsync(999);

            // Assert
            Assert.Null(result);
        }
    }
}
```

---

## LINQ Patterns

### Query Syntax
```csharp
var users = from u in _context.Users
            where u.IsActive && u.Role == "admin"
            orderby u.CreatedAt descending
            select u;
```

### Method Syntax (Preferred)
```csharp
var users = _context.Users
    .Where(u => u.IsActive && u.Role == "admin")
    .OrderByDescending(u => u.CreatedAt)
    .ToListAsync();
```

### Complex Queries
```csharp
var userStats = await _context.Users
    .Where(u => u.IsActive)
    .GroupBy(u => u.Role)
    .Select(g => new
    {
        Role = g.Key,
        Count = g.Count(),
        AvgAge = g.Average(u => u.Age)
    })
    .ToListAsync();
```

---

## Configuration Management

### appsettings.json
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=MyApp;Trusted_Connection=True;"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "Jwt": {
    "Key": "your-secret-key",
    "Issuer": "MyApp",
    "Audience": "MyApp",
    "ExpirationInMinutes": 60
  }
}
```

### Strongly-Typed Configuration
```csharp
// Options/JwtOptions.cs
namespace MyApp.Options
{
    public class JwtOptions
    {
        public string Key { get; set; } = string.Empty;
        public string Issuer { get; set; } = string.Empty;
        public string Audience { get; set; } = string.Empty;
        public int ExpirationInMinutes { get; set; }
    }
}

// Program.cs
builder.Services.Configure<JwtOptions>(builder.Configuration.GetSection("Jwt"));

// Usage in service
public class AuthService
{
    private readonly JwtOptions _jwtOptions;

    public AuthService(IOptions<JwtOptions> jwtOptions)
    {
        _jwtOptions = jwtOptions.Value;
    }
}
```

---

## Documentation (XML Comments)

### XML Documentation
```csharp
/// <summary>
/// Retrieves a user by their unique identifier.
/// </summary>
/// <param name="id">The user's ID.</param>
/// <returns>The user data transfer object, or null if not found.</returns>
/// <exception cref="ArgumentException">Thrown when ID is less than or equal to zero.</exception>
/// <example>
/// <code>
/// var user = await userService.GetByIdAsync(123);
/// </code>
/// </example>
public async Task<UserDto?> GetByIdAsync(int id)
{
    if (id <= 0)
    {
        throw new ArgumentException("ID must be greater than zero", nameof(id));
    }

    var user = await _userRepository.GetByIdAsync(id);
    return user?.ToDto();
}
```

---

## Key Principles

1. **PascalCase for classes, methods, properties**
2. **camelCase for local variables, parameters**
3. **_camelCase for private fields** (leading underscore)
4. **Async suffix** for async methods
5. **Dependency Injection** for services
6. **Repository pattern** for data access
7. **DTOs** for API contracts
8. **XML comments** on public APIs
9. **ConfigureAwait(false)** in library code
10. **Unit testing** with xUnit and Moq

---

**This template should be used for all C# / .NET projects (ASP.NET Core, console apps, services, etc.).**
