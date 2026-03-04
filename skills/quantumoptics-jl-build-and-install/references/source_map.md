# quantumoptics-jl source map: Build and Install

Use this map after checking `doc_map.md`.

## Topic query tokens
- `Pkg.test`
- `TestItemRunner`
- `@run_package_tests`
- `CUDA_TEST`
- `AMDGPU_TEST`
- `OpenCL_TEST`
- `JET_TEST`
- `benchmark`

## Function-level source entry points
- `test/runtests.jl` | test filtering (`testfilter`), backend flag parsing, and test dispatch via `@run_package_tests`
- `src/QuantumOptics.jl` | module include/export wiring; confirms runtime load path for all solver modules
- `benchmark/benchmarks.jl` | benchmark constructors: `bench_schroedinger`, `bench_master`, `bench_stochastic_schroedinger`, `bench_stochastic_master`
- `test/gpu/test_platform_CUDA.jl` | CUDA backend tests
- `test/gpu/test_platform_AMDGPU.jl` | AMDGPU backend tests
- `test/gpu/test_platform_OpenCL.jl` | OpenCL backend tests
- `test/test_jet.jl`, `test/test_aqua.jl` | static-analysis gates enabled from `test/runtests.jl`

## Validation test entry points
- `test/test_timeevolution_schroedinger.jl` | baseline deterministic solver behavior
- `test/test_timeevolution_master.jl` | master-equation correctness across rate shapes
- `test/test_timeevolution_mcwf.jl` | trajectory reproducibility and master/MCWF consistency checks
- `test/test_steadystate.jl` | steady-state regression checks
- `test/test_spectralanalysis.jl` | eigen/simdiag solver behavior checks
- `test/test_phasespace.jl` | phase-space function behavior checks

## Fast source navigation
- `rg -n "CUDA_TEST|AMDGPU_TEST|OpenCL_TEST|JET_TEST|@run_package_tests|testfilter" test/runtests.jl`
- `rg -n "bench_(schroedinger|master|stochastic_)" benchmark/benchmarks.jl`
- `rg -n "@testitem" test`

## Quick execution matrix
- `julia --project=. -e "using Pkg; Pkg.test()"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"timeevolution_master\")"`
- `CUDA_TEST=true julia --project=. -e "using Pkg; Pkg.test()"`
- `JET_TEST=true julia --project=. -e "using Pkg; Pkg.test()"`
- `julia --project=benchmark benchmark/benchmarks.jl`
