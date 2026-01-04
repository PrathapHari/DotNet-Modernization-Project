# EditorConfig Guide for eShopLite.StoreFx

## Overview

The `.editorconfig` file enforces consistent coding standards across the entire development team and all IDEs (Visual Studio, VS Code, Rider, etc.).

---

## What Does It Enforce?

### 🎯 Naming Conventions

| Type | Convention | Example | Severity |
|------|-----------|---------|----------|
| **Interfaces** | `I` + PascalCase | `IStoreService` | ⚠️ Warning |
| **Classes** | PascalCase | `Product`, `StoreService` | ⚠️ Warning |
| **Methods** | PascalCase | `GetProducts()` | ⚠️ Warning |
| **Properties** | PascalCase | `Id`, `Name` | ⚠️ Warning |
| **Private Fields** | `_` + camelCase | `_context`, `_service` | ⚠️ Warning |
| **Public Fields** | PascalCase | `Products` | ⚠️ Warning |
| **Parameters** | camelCase | `context`, `id` | ⚠️ Warning |
| **Local Variables** | camelCase | `productCount` | ⚠️ Warning |
| **Constants** | PascalCase | `MaxRetryCount` | ⚠️ Warning |
| **Type Parameters** | `T` + PascalCase | `TEntity`, `TResult` | ⚠️ Warning |

### 📝 Code Formatting

#### Indentation
- **C# Files:** 4 spaces
- **Razor Files:** 4 spaces
- **JSON Files:** 2 spaces
- **XML/Config Files:** 2 spaces
- **YAML Files:** 2 spaces

#### Using Statements
```csharp
// ✅ CORRECT - System namespaces first, then others
using System;
using System.Collections.Generic;
using System.Text.Json.Serialization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using eShopLite.StoreFx.Data;
using eShopLite.StoreFx.Models;

namespace eShopLite.StoreFx.Services
{
    // ...
}
```

#### Braces
```csharp
// ✅ CORRECT - Braces on new line (Allman style)
public class Product
{
    public int Id { get; set; }
    
    public void DoSomething()
    {
        if (condition)
        {
            // code
        }
    }
}

// ❌ WRONG - Braces on same line
public class Product {
    public int Id { get; set; }
}
```

#### Spacing
```csharp
// ✅ CORRECT
if (x == y)
{
    var result = Calculate(a, b);
}

// ❌ WRONG
if(x==y)
{
    var result = Calculate( a,b );
}
```

### 🔍 Code Style Preferences

#### `var` Usage
```csharp
// ✅ Suggested - Use var for built-in types when type is apparent
var productCount = 10;
var name = "Product Name";
var product = new Product();

// ✅ Also acceptable
int productCount = 10;
string name = "Product Name";
Product product = new Product();
```

#### Expression-Bodied Members
```csharp
// ✅ CORRECT - Properties
public string FullName => $"{FirstName} {LastName}";

// ✅ CORRECT - Methods (full body preferred)
public int Calculate()
{
    return x + y;
}

// ⚠️ Allowed but not preferred for methods
public int Calculate() => x + y;
```

#### Pattern Matching
```csharp
// ✅ CORRECT - Use pattern matching
if (obj is Product product)
{
    // use product
}

// ❌ AVOID - Old style
if (obj is Product)
{
    var product = (Product)obj;
}
```

#### Null Checking
```csharp
// ✅ CORRECT - Null-conditional operator
var length = name?.Length ?? 0;

// ✅ CORRECT - Throw expression
_context = context ?? throw new ArgumentNullException(nameof(context));

// ❌ AVOID
if (name != null)
{
    length = name.Length;
}
else
{
    length = 0;
}
```

---

## Project-Specific Rules

### DbContext Classes
```csharp
// ✅ CORRECT - Ends with DbContext
public class StoreDbContext : DbContext
{
    // ...
}

// ⚠️ Suggestion violation
public class StoreDatabase : DbContext
{
    // ...
}
```

### Controller Classes
```csharp
// ✅ CORRECT - Ends with Controller
public class HomeController : Controller
{
    // ...
}

// ⚠️ Suggestion violation
public class Home : Controller
{
    // ...
}
```

### Service Classes
```csharp
// ✅ CORRECT - Interface with I prefix, implementation ends with Service
public interface IStoreService
{
    // ...
}

public class StoreService : IStoreService
{
    // ...
}
```

---

## How to Use

### Visual Studio 2022
1. **Automatic** - EditorConfig is automatically detected and applied
2. **View Settings:**
   - Tools → Options → Text Editor → C# → Code Style
3. **Format Document:** `Ctrl+K, Ctrl+D`
4. **Warnings:** You'll see green/yellow squiggles for violations

### Visual Studio Code
1. **Install Extension:**
   ```bash
   ext install EditorConfig.EditorConfig
   ```
2. **Format Document:** `Shift+Alt+F`
3. **Automatic Formatting:** Enable format on save
   ```json
   {
     "editor.formatOnSave": true
   }
   ```

### JetBrains Rider
1. **Automatic** - EditorConfig is automatically detected
2. **Format Code:** `Ctrl+Alt+L`
3. **Code Inspections:** Settings → Editor → Code Style

---

## Severity Levels

| Severity | Meaning | Visual Indicator |
|----------|---------|------------------|
| **none** | Disabled | None |
| **suggestion** | Informational | ℹ️ Info (gray dots) |
| **warning** | Should be fixed | ⚠️ Warning (green/yellow squiggle) |
| **error** | Must be fixed | ❌ Error (red squiggle) |

---

## Common Violations and Fixes

### ❌ Violation: Private field not using underscore prefix
```csharp
// ❌ WRONG
private StoreDbContext context;

// ✅ CORRECT
private readonly StoreDbContext _context;
```

### ❌ Violation: Interface not starting with I
```csharp
// ❌ WRONG
public interface StoreService
{
    // ...
}

// ✅ CORRECT
public interface IStoreService
{
    // ...
}
```

### ❌ Violation: Method not using PascalCase
```csharp
// ❌ WRONG
public void getProducts()
{
    // ...
}

// ✅ CORRECT
public void GetProducts()
{
    // ...
}
```

### ❌ Violation: Parameter not using camelCase
```csharp
// ❌ WRONG
public HomeController(IStoreService Service)
{
    _service = Service;
}

// ✅ CORRECT
public HomeController(IStoreService service)
{
    _service = service;
}
```

---

## Disabling Rules

### For Specific Lines
```csharp
#pragma warning disable CA1031 // Do not catch general exception types
try
{
    // code
}
catch (Exception ex)
{
    // handle
}
#pragma warning restore CA1031
```

### For Entire File
```csharp
// At top of file
#pragma warning disable CA1031

// ... rest of file
```

### In EditorConfig
```ini
# In .editorconfig
dotnet_diagnostic.CA1031.severity = none
```

---

## Testing the Configuration

### Quick Test Commands

**1. Check if EditorConfig is loaded:**
- In Visual Studio: Tools → Options → Text Editor → C# → Code Style
- Look for "EditorConfig" indicators

**2. Test formatting:**
```csharp
// Type this incorrectly formatted code
public class test{public int id{get;set;}}

// Press Ctrl+K, Ctrl+D (Format Document)

// Should become:
public class test
{
    public int id { get; set; }
}
```

**3. Test naming warnings:**
```csharp
// This should show a warning (green squiggle)
private StoreDbContext context; // Missing underscore

// This should be correct
private readonly StoreDbContext _context;
```

---

## Team Adoption

### For New Team Members
1. Clone the repository
2. Open solution in IDE
3. EditorConfig is automatically applied
4. No manual configuration needed!

### Code Reviews
- EditorConfig violations will show as warnings
- CI/CD can enforce these rules
- Consistent code across all developers

### Benefits
✅ Consistent code style across team  
✅ Reduced code review friction  
✅ Automatic formatting on save  
✅ Works across all major IDEs  
✅ No manual style guide needed  
✅ Enforced at IDE level  

---

## Continuous Integration

### Add to CI/CD Pipeline

**GitHub Actions:**
```yaml
- name: Check Code Style
  run: dotnet format --verify-no-changes --verbosity diagnostic
```

**Azure DevOps:**
```yaml
- task: DotNetCoreCLI@2
  displayName: 'Check Code Style'
  inputs:
    command: 'custom'
    custom: 'format'
    arguments: '--verify-no-changes --verbosity diagnostic'
```

---

## Troubleshooting

### EditorConfig Not Working?

**1. Check IDE Version:**
- Visual Studio 2022: Full support
- VS Code: Requires EditorConfig extension
- Rider: Built-in support

**2. Restart IDE:**
- Sometimes a restart is needed after adding/modifying .editorconfig

**3. Check File Location:**
- Must be named `.editorconfig` exactly
- Must be in solution root or parent directory
- Case-sensitive on Linux/Mac

**4. Clear IDE Cache:**
- Visual Studio: Delete `.vs` folder
- VS Code: Reload window (`Ctrl+Shift+P` → "Reload Window")
- Rider: File → Invalidate Caches / Restart

---

## Additional Resources

- [EditorConfig Documentation](https://editorconfig.org/)
- [.NET Code Style Rules](https://learn.microsoft.com/en-us/dotnet/fundamentals/code-analysis/style-rules/)
- [C# Coding Conventions](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/coding-style/coding-conventions)
- [Visual Studio EditorConfig](https://learn.microsoft.com/en-us/visualstudio/ide/create-portable-custom-editor-options)

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | Jan 2025 | Initial version for eShopLite.StoreFx |

---

**Maintained By:** Development Team  
**Last Updated:** January 2025  
**Questions?** Open an issue or contact the team lead
