# CloudQA Form Automation Test Suite

C# Selenium automation tests for CloudQA practice form with robust element location strategies.
- 🎥 *Demo Video*: [Watch Demo](https://drive.google.com/file/d/1jn0A-aZJFc_8vY2bCtIh5X-K34sInfmv/view?usp=sharing)
## Features
  
- **Multi-tier locator fallbacks** - Tests survive HTML structure changes
- **Page Object Model** - Clean, maintainable architecture
- **Data-driven testing** - Multiple test scenarios
- **Auto-retry mechanisms** - Handles flaky elements
- **Screenshot capture** - Automatic failure debugging

## Quick Start

```bash
git clone https://github.com/banti8974/CloudQATests.git
cd CloudQATests
dotnet restore
dotnet test
```

## Dependencies

```xml
<PackageReference Include="Selenium.WebDriver" Version="4.15.0" />
<PackageReference Include="Selenium.WebDriver.ChromeDriver" Version="119.0.6045.10500" />
<PackageReference Include="Selenium.Support" Version="4.15.0" />
<PackageReference Include="DotNetSeleniumExtras.WaitHelpers" Version="3.11.0" />
<PackageReference Include="NUnit" Version="3.14.0" />
<PackageReference Include="NUnit3TestAdapter" Version="4.5.0" />
```

## What It Tests

| Field | Type | Locator Strategy |
|-------|------|------------------|
| First Name | Text Input | ID → Name → XPath → CSS |
| State | Dropdown | ID → Name → XPath → CSS |
| Hobby | Checkbox | ID → Value → XPath → CSS |

## Architecture

```
├── CloudQAFormTests.cs          # Main test class
├── CloudQAFormPage.cs           # Page Object Model
├── RobustElementLocator.cs      # Multi-fallback element finder
└── TestLogger.cs                # Logging utility
```

## Key Implementation

**Robust Element Location:**
```csharp
var element = elementLocator.FindElement(
    "First Name Field",
    By.Id("fname"),                           // Primary
    By.Name("First Name"),                    // Secondary  
    By.XPath("//input[@placeholder='Name']"), // Tertiary
    By.CssSelector("input[class*='form-control']:first-of-type") // Fallback
);
```

**Auto-retry with Validation:**
```csharp
for (int attempt = 1; attempt <= MAX_RETRIES; attempt++)
{
    foreach (var locator in locators)
    {
        var element = wait.Until(ExpectedConditions.ElementToBeClickable(locator));
        if (element.Displayed && element.Enabled) return element;
    }
}
```

## Run Tests

```bash
# All tests
dotnet test

# Specific scenario  
dotnet test --filter "TestName~ValidFormSubmission_Scenario1"

# With detailed output
dotnet test --logger "console;verbosity=detailed"
```

## Output

- **Test results** in console
- **Screenshots** in `/Screenshots` (on failure)
- **Execution logs** in `/test-execution.log`

## Requirements

- .NET 6.0+
- Chrome browser
- ChromeDriver (auto-managed by NuGet package)
