# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build

```bash
cd algorithms
mkdir -p build && cd build
cmake ..
make
```

Or with Ninja:
```bash
cmake -G Ninja .. && ninja
```

## Running Tests

```bash
# All tests via CTest
cd algorithms/build
ctest --output-on-failure -V

# Direct executable
./algorithms

# Single test or filter
./algorithms --gtest_filter=KnapsackTest.*
```

## Architecture

All algorithm implementations live in `algorithms/` as **header-only files** with tests embedded directly using GoogleTest. `algorithms.cpp` is a minimal entry point that `#include`s each header.

- `make_matrix.h` — 2D array utility using smart pointers (`makeMatrix<T>`, `MatrixUPtr<T>`)
- `shortest_path.h` — Dijkstra's algorithm with a `graph_t` class (adjacency via unordered hash maps)
- `knapsack.h` — 0/1 Knapsack DP with standard and space-optimized variants, plus item recovery

**Adding a new algorithm**: create a `.h` file with both implementation and `TEST(...)` blocks, then `#include` it in `algorithms.cpp` and add it to `CMakeLists.txt`.

GoogleTest is fetched automatically via `FetchContent` at configure time (requires internet access on first build).
