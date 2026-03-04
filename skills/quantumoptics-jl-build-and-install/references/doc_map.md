# quantumoptics-jl documentation map: Build and Install

Generated from local documentation roots:
- `.`
- `test`
- `benchmark`

Total docs grouped in this topic: 5

## File inventory
- `CLAUDE.md` | purpose: development commands, test matrix notes, backend toggles
- `README.md` | purpose: repository layout and canonical external doc/example pointers
- `Project.toml` | purpose: root package dependencies and Julia compatibility bounds
- `test/Project.toml` | purpose: test-only dependency constraints
- `benchmark/Project.toml` | purpose: benchmark harness dependencies

## Operational command anchors
- `julia --project=. -e "using Pkg; Pkg.instantiate(); Pkg.test()"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=..."`
- `CUDA_TEST=true|AMDGPU_TEST=true|OpenCL_TEST=true|JET_TEST=true ...`
- `julia --project=benchmark benchmark/benchmarks.jl`

## External canonical docs/examples (not vendored in this checkout)
- `https://docs.qojulia.org/`
- `https://github.com/qojulia/QuantumOptics.jl-documentation`
- `https://github.com/qojulia/QuantumOptics.jl-examples`
