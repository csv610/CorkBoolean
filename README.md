# Cork Boolean Library

A C++ library for performing Boolean operations on triangle meshes.

## Overview

Cork computes boolean operations (union, difference, intersection, XOR) between watertight triangle meshes using GMP-based exact arithmetic.

## Requirements

- CMake 3.10+
- C++20 compatible compiler (Clang, GCC)
- GMP 5.0+

## Building

```bash
mkdir build
cmake -S . -B build
cmake --build build
```

## Testing

```bash
cd build
ctest --output-on-failure
```

## Usage

### Command Line

```bash
# Boolean operations
./cork_cli -union inputA.off inputB.off output.off
./cork_cli -diff inputA.off inputB.off output.off
./cork_cli -isct inputA.off inputB.off output.off
./cork_cli -xor inputA.off inputB.off output.off

# Validate mesh
./cork_cli -solid mesh.off

# Resolve intersections
./cork_cli -resolve inputA.off inputB.off output.off
```

### C++ API

```cpp
#include "cork.h"

CorkTriMesh result;
computeUnion(meshA, meshB, &result);
freeCorkTriMesh(&result);

bool solid = isSolid(mesh);
```

## Supported Formats

- OFF (Object File Format)
- IFS (Internal Face Set)

## License

See COPYRIGHT file. Licensed under LGPL.