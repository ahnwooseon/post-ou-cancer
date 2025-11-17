# Package Licenses and Legal Considerations

This document outlines the licenses of third-party packages used in the Post Ou Cancer platform and any legal considerations.

## License Overview

**All packages used in this project are open source** with permissive licenses:
- **MIT License**: Most packages (ClosedXML, Mediator, Refit, Scrutor, etc.)
- **Apache 2.0**: FastEndpoints, FluentValidation, MassTransit, Dapper, OpenTelemetry, xUnit
- **BSD-3-Clause**: Polly, NSubstitute, Shouldly
- **LGPL**: Hangfire

**No commercial licenses required** - all packages are free for commercial use.

## Critical License Considerations

### ClosedXML (Excel Processing)

**Package**: `ClosedXML` Version 0.104.0

**License**: **MIT License** (free for commercial use)

**Status**: ✅ Open source, no licensing concerns

**Usage Example**:
```csharp
using ClosedXML.Excel;
var workbook = new XLWorkbook();
var worksheet = workbook.Worksheets.Add("Sheet1");
worksheet.Cell(1, 1).Value = "Hello";
workbook.SaveAs("output.xlsx");
```

## Package License Summary

| Package | License | Commercial Use | Notes |
|---------|---------|----------------|-------|
| FastEndpoints | Apache 2.0 | ✅ Yes | Open source |
| FluentValidation | Apache 2.0 | ✅ Yes | Open source |
| MassTransit | Apache 2.0 | ✅ Yes | Open source |
| Mediator | MIT | ✅ Yes | Open source |
| Dapper | Apache 2.0 | ✅ Yes | Open source |
| EF Core | MIT | ✅ Yes | Open source |
| Hangfire | LGPL | ✅ Yes | Open source |
| Refit | MIT | ✅ Yes | Open source |
| Polly | BSD-3-Clause | ✅ Yes | Open source |
| Scrutor | MIT | ✅ Yes | Open source |
| NSubstitute | BSD-3-Clause | ✅ Yes | Open source |
| Bogus | MIT | ✅ Yes | Open source |
| ClosedXML | MIT | ✅ Yes | Open source |
| MudBlazor | MIT | ✅ Yes | Open source |
| YARP | MIT | ✅ Yes | Open source |
| OpenTelemetry | Apache 2.0 | ✅ Yes | Open source |
| Testcontainers | MIT | ✅ Yes | Open source |
| xUnit | Apache 2.0 | ✅ Yes | Open source |
| Shouldly | BSD-3-Clause | ✅ Yes | Open source |

## License Compliance Checklist

- [x] All open-source packages reviewed
- [x] All packages use open-source licenses (MIT, Apache 2.0, BSD, LGPL)
- [ ] License file generated (if required)
- [ ] Third-party notices file created
- [ ] License compliance verified for production

## Recommendations

1. **Before Production**: All packages are open source, no licensing concerns
2. **License File**: Generate `LICENSE-THIRD-PARTY.txt` with all third-party licenses
3. **Regular Review**: Review licenses when adding new packages (ensure open source only)
4. **Documentation**: Keep this document updated with license changes

## Generating Third-Party Licenses

**Status**: ⚠️ To be implemented

Use tools like:
- `dotnet list package --include-transitive --format json` to list all packages
- License scanning tools (e.g., `dotnet-project-licenses`)
- Manual review of NuGet package licenses

