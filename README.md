# PRISM Registry - Function Extensions & Documentation

**Centralized registry for PRISM API extensions, function specifications, and version management**

[![License: CC BY SA 4.0](https://img.shields.io/badge/License-CC--BY--SA--4.0%20-blue)](https://creativecommons.org/licenses/by-sa/4.0/)
[![Registry Version](https://img.shields.io/badge/registry-v1.0.1-blue.svg)]()
[![Functions](https://img.shields.io/badge/functions-133-green.svg)]()

## Overview

The **PRISM** Registry serves as the central repository for PRISM API function specifications, extensions, and versioning. It provides standardized function definitions, vendor extensions, and compatibility matrices across different PRISM implementations.

@see [**PRISM**](https://github.com/RoseLeDark/prism/)

## Structure

```
prism-registry/
├── api/                    # Core API specifications
│   ├── prism-1.0.yaml       # Base API v1.0
├── extensions/             # Vendor and community extensions
│   ├── _v256_vendor_extensions/
│   ├── _v256_community_extensions/
│   └── _v256_experimental/
├── specs/                  # Detailed specifications
│   ├── core/              # Core functionality specs as yaml
│   |   ├── enum/
│   |   ├── functions/
│   |   └── types/
│   ├── extensions/        # Extension specifications
```

## Core API Functions

### Current Registry (v1.0.1)

| Category | Functions | Description |
|----------|-----------|-------------|
| **Device Management** | 4 | Connection, initialization, discovery |
| **Vector Operations** | 110 | SIMD arithmetic, logical operations |
| **Data Transfer** | 8 | Register read/write, DMA operations |
| **Extensions** | 1 | Vendor-specific enhancements |

## Function Naming Convention

```c
// Core functions
_v256_[operation]_[vector]_[ui/si]_[v/n/x]([parameters])

// Extensions
_v256_[operation]_[vector]_[ui/si]_[v/n/x]_[vendor]([parameters])  

// Examples:
_v256_set8_uiv(count, device);           // Core function
_v256_sub_ui_v256(device); // _v256_256_extensions
```

## Extension System

### Vendor Extensions

**ESP32 Extensions (_v256_ESP32_extensions)**
```c
// Turbo mode operations
_v256_set_turbomode_esp32(bool enable, prdev_t device);
_v256_addN_turbomode_ui_esp32(count, device);

// spi integration
_v256_spi_store_v_esp32(vector, ssid)
```

**256bit Extensions (_v256_256_extensions)**
```c
// High-precision arithmetic
_v256_add_ui_v256(device);
_v256_mul_ui_v256(device);
_v256_div_ui_v256(device);
_v256_sub_ui_v256(device);

```

### Community Extensions

## API Versioning

### Version Compatibility

| PRISM API | Registry Version | Compatibility | Status |
|-----------|------------------|---------------|---------|
| 1.0.x | v1.0.0 - v1.0.5 | Full | Development |
| 1.1.x |  | Future | Future |

### Feature Detection

```c
// Check API version
uint32_t version = _v256_get_version();
if (_v256_VERSION_MAJOR(version) >= 1 && _v256_VERSION_MINOR(version) >= 2) {
    // Use v1.2+ features
}

// Check extension support
if (_v256_arch_extension_supported("_v256_256_extensions")) {
    // 256bit mode support
}
```

## Adding New Functions

### 1. Function Specification

Create specification in `/specs/extensions/`:

```xml
<extension name="_v256_YOUR_EXTENSION" supported="prism">
    <require>
        <command>
            <proto>void <name>_v256_your_new_function</name>
                <ptype>prdev_t</ptype> <name>device</name>
                <ptype>ui32</ptype> <name>param1</name>
                <ptype>timeout_t</ptype> <name>timeout</name>
            </proto>
        </command>
    </require>
</extension>
```

### 2. Implementation Requirements

```c
// Function signature
void _v256_your_new_function(uint32_t param1, prdev_t device);

// Error handling
typedef enum {
  PR_OK = 0,
  PR_ERR_INVALID_ARGUMENT,
  PR_ERR_OUT_OF_MEMORY,
  PR_ERR_UNSUPPORTED_OPERATION,
  PR_ERR_UNKNOWN
} prism_err;

// Thread safety
// All functions must be thread-safe or clearly documented
```

### 3. Documentation Template

```markdown
## _v256_your_new_function

**Syntax:**
```c
void _v256_your_new_function(uint32_t param1, prdev_t device);
```

**Parameters:**
- `param1`: Description of parameter
- `device`: Target PRISM device

**Description:**
Detailed description of what the function does.

**Return Value:**
Description of return value (if any).

**Example:**
```c
_v256_your_new_function(42, device);
```

**Availability:** PRISM 1.2+, YOUR_EXTENSION
```

### 4. Conformance Tests

```c
// tests/extension_tests/test_your_extension.c
void test_your_new_function() {
    prdev_t device = _v256_dev_hs(0x50, Flankspeed);
    
    // Test normal operation
    prism_err result = _v256_your_new_function(42, device);
    assert(result == _v256_SUCCESS);
    
    // Test error conditions
    result = _v256_your_new_function(0, NULL);
    assert(result == PR_ERR_INVALID_DEVICE);
}
```

### Certification Process

1. **Submit Extension** - Create PR with specification
2. **Review Process** - Community and maintainer review
3. **Implementation** - Reference implementation required
4. **Testing** - Pass conformance tests
5. **Documentation** - Complete documentation
6. **Approval** - Merge into registry

## 🤝 Contributions

### Extension Guidelines

- **Naming**: Use clear, descriptive names
- **Compatibility**: Don't break existing API
- **Performance**: Benchmark critical functions
- **Documentation**: Comprehensive docs required
- **Testing**: Full test coverage

### Submission Process

1. Fork the repository
2. Create extension specification
3. Add conformance tests
4. Update documentation
5. Submit pull request

### Review Criteria

- **Technical Merit**: Does it solve a real problem?
- **API Design**: Consistent with PRISM conventions?
- **Implementation**: Reference implementation available?
- **Documentation**: Clear and complete?
- **Testing**: Adequate test coverage?

## License

The PRISM Registry is licensed under [Creative Commons Attribution-ShareAlike 4.0 International](https://creativecommons.org/licenses/by-sa/4.0/).

- **Specifications**: CC BY-SA 4.0
- **Code Generators**: MIT License  
- **Test Code**: MIT License


## 🌐 Related Repositories

- [prism-api](https://github.com/RoseLeDark/prism-api)
- [prism-api-arduino](https://github.com/RoseLeDark/prism-api-arduino)
- [prism-esp-soft](https://github.com/RoseLeDark/prism-esp-soft)

