# Cork Boolean Library

Cork is a C++ library for performing Boolean operations (union, difference, intersection, XOR) on triangle meshes.

## Dependencies

- C++20 compatible compiler (Clang recommended)
- GMP (GNU Multi-Precision arithmetic library)
- CMake 3.10+

## Building

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

## Usage

The `cork_cli` executable provides command-line Boolean operations:

```bash
./build/cork_cli -union inputA.off inputB.off output.off
./build/cork_cli -diff inputA.off inputB.off output.off
./build/cork_cli -isct inputA.off inputB.off output.off
./build/cork_cli -xor inputA.off inputB.off output.off
./build/cork_cli -resolve inputA.off inputB.off output.off
./build/cork_cli -solid mesh.off    # Check if mesh is solid (watertight)
```

## Library API

```cpp
#include "cork.h"

CorkTriMesh mesh = {/* initialize mesh */};

bool solid = isSolid(mesh);

CorkTriMesh result;
computeUnion(meshA, meshB, &result);
computeDifference(meshA, meshB, &result);
computeIntersection(meshA, meshB, &result);
computeSymmetricDifference(meshA, meshB, &result);
resolveIntersections(meshA, meshB, &result);

freeCorkTriMesh(&result);
```

## License

See COPYRIGHT file for details. Licensed under LGPL.