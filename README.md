# Cork Boolean Library

A high-performance C++ library for performing Boolean operations on triangle meshes, maintained as an open-source project.

## What is Cork?

Cork is a robust boolean mesh library designed to compute accurate boolean operations (union, difference, intersection, XOR) between triangle meshes. Originally developed by Gilbert Bernstein for geometric modeling applications.

## Why Cork When CGAL and libigl Exist?

While CGAL and libigl are excellent libraries, Cork offers distinct advantages:

1. **Simplicity**: Minimal, focused API. No complex geometric kernel setup—just include `cork.h` and go.

2. **Standalone**: No external geometric kernel dependencies. Works with raw triangle arrays.

3. **Exact Arithmetic**: Handles numerical precision internally using GMP-based exact arithmetic.

4. **Lightweight**: Single static library link. No template-heavy compile times.

5. **CLI Tools**: Ready-to-use command-line tools for batch processing.

For complex geometric queries beyond boolean operations, CGAL and libigl remain excellent choices.

## Features

- **Boolean Operations**: Union, Difference, Intersection, XOR
- **Mesh Validation**: Check if meshes are solid/watertight
- **Intersection Resolution**: Make mesh intersections explicit and connected
- **Cross-Platform**: macOS, Linux, Windows support

## Building

Requirements:
- CMake 3.10+
- C++20 compatible compiler
- GMP (GNU Multi-Precision library)

```bash
mkdir build
cmake -S . -B build
cmake --build build
```

## Running Tests

```bash
cd build
ctest --output-on-failure
```

## Quick Start

### Command Line

```bash
# Union of two meshes
./cork_cli -union inputA.off inputB.off output.off

# Check if solid
./cork_cli -solid mesh.off
```

### C++ API

```cpp
#include "cork.h"

CorkTriMesh result;
computeUnion(meshA, meshB, &result);
freeCorkTriMesh(&result);
```

## Supported Formats

- OFF (Object File Format)
- IFS (Internal Face Set)

## License

See COPYRIGHT file. Licensed under LGPL.

## Author

Gilbert Bernstein