# Cork Boolean Library

A high-performance C++ library for performing Boolean operations on triangle meshes, originally developed at Google and now maintained as an open-source project.

## What is Cork?

Cork is a robust boolean mesh library designed to compute accurate boolean operations (union, difference, intersection, XOR) between triangle meshes. It was originally developed by Gilbert Bernstein at Google for internal geometric modeling needs.

## Why Cork When CGAL and libigl Exist?

While CGAL and libigl are excellent libraries, Cork offers distinct advantages:

1. **Simplicity**: Cork has a minimal, focused API. No complex configuration or geometric kernel setup required—just include `cork.h` and go.

2. **Standalone**: No external geometric kernel dependencies. Works with raw triangle arrays in a straightforward data structure.

3. **Self-Contained**: Unlike CGAL which requires extensive setup and configuration for robust arithmetic, Cork handles numerical precision internally using its own exact arithmetic based on GMP.

4. **Lightweight**: A single static library link. No template-heavy compile times or large dependency chains.

5. **CLI Tools**: Comes with ready-to-use command-line tools for batch processing mesh files.

However, for complex geometric queries beyond boolean operations, CGAL and libigl remain excellent choices with broader functionality.

## Features

- **Boolean Operations**: Union, Difference, Intersection, XOR
- **Mesh Validation**: Check if meshes are solid/watertight
- **Intersection Resolution**: Make mesh intersections explicit and connected
- **Exact Arithmetic**: Robust handling of numerical precision
- **Cross-Platform**: macOS, Linux, Windows support

## Building

Requirements:
- CMake 3.10+
- C++20 compatible compiler (Clang recommended)
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

# Difference
./cork_cli -diff inputA.off inputB.off output.off

# Intersection
./cork_cli -isct inputA.off inputB.off output.off

# Check if mesh is solid/watertight
./cork_cli -solid mesh.off
```

### C++ API

```cpp
#include "cork.h"

// Create input mesh
CorkTriMesh mesh;
mesh.n_vertices = 4;
mesh.n_triangles = 4;
mesh.vertices = new float[12]{/* vertex positions */};
mesh.triangles = new uint[12]{/* triangle indices */};

// Check if solid
bool solid = isSolid(mesh);

// Boolean operations
CorkTriMesh result;
computeUnion(meshA, meshB, &result);
computeDifference(meshA, meshB, &result);
computeIntersection(meshA, meshB, &result);
computeSymmetricDifference(meshA, meshB, &result);

// Cleanup
freeCorkTriMesh(&result);
```

## Supported Formats

- OFF (Object File Format)
- IFS (Internal Face Set)

## License

This project is a port of the original Google Cork Library. See the COPYRIGHT file for licensing details. Licensed under LGPL with exception for template code.

## acknowledgments

- Original author: Gilbert Bernstein
- Previously at Google Research