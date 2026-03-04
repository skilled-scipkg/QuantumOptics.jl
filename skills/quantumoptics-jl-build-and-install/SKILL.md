---
name: quantumoptics-jl-build-and-install
description: Core skill for installing QuantumOptics.jl, running CPU/GPU/JET test matrices, and validating simulation runtime readiness.
---

# quantumoptics-jl: Build and Install

## High-Signal Playbook

### Route conditions
- Use this skill for setup, dependency resolution, test execution strategy, backend toggles, and runtime smoke checks.
- Route to `quantumoptics-jl-license` for dynamics/correlation/stochastic workflow questions.
- Route to `quantumoptics-jl-inputs-and-modeling` for steady-state, spectral, and phase-space modeling questions.

### Triage questions
- Is Julia `1.10+` available and are commands running with `--project=.`?
- Do you need full regression, targeted `@testitem` runs, or backend-specific validation?
- Are GPU backends (`CUDA_TEST`, `AMDGPU_TEST`, `OpenCL_TEST`) required?
- Is static analysis (`JET_TEST=true`) required?
- Do you need a quick simulation smoke check before long jobs?

### Canonical workflow
1. Instantiate dependencies and inspect environment health.
```bash
julia --project=. -e "using Pkg; Pkg.instantiate(); Pkg.status()"
```
2. Run full CPU suite.
```bash
julia --project=. -e "using Pkg; Pkg.test()"
```
3. Run targeted tests for failing areas.
```bash
julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"timeevolution_master\")"
julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"steadystate\")"
```
4. Run optional backend/static-analysis suites only when needed.
```bash
CUDA_TEST=true julia --project=. -e "using Pkg; Pkg.test()"
AMDGPU_TEST=true julia --project=. -e "using Pkg; Pkg.test()"
OpenCL_TEST=true julia --project=. -e "using Pkg; Pkg.test()"
JET_TEST=true julia --project=. -e "using Pkg; Pkg.test()"
```
5. Run benchmarks after correctness is established.
```bash
julia --project=benchmark benchmark/benchmarks.jl
```

### Simulation-readiness smoke check
```bash
julia --project=. -e "using QuantumOptics; b=SpinBasis(1//2); H=sigmax(b); psi0=spindown(b); T=collect(0.0:0.1:1.0); t, psit = timeevolution.schroedinger(T, psi0, H); @assert length(t)==length(T)==length(psit); println(\"smoke-ok\")"
```

### Pitfalls and fixes
- Running without `--project=.` can silently select incompatible dependencies.
- GPU tests are skipped by default and on Windows; set backend flags explicitly on supported platforms.
- `test/runtests.jl` may install backend dependencies at test startup when backend flags are set.
- `JET_TEST=true` changes test filtering behavior; run it separately from baseline CPU checks.
- Benchmarking does not replace correctness checks.

### Validation checkpoints
- `Pkg.instantiate()` completes without resolver errors.
- CPU `Pkg.test()` passes.
- Smoke command prints `smoke-ok`.
- Optional GPU/JET suites pass when required for the target platform.

## Primary documentation references
- `CLAUDE.md`
- `README.md`
- `Project.toml`
- `test/Project.toml`
- `benchmark/Project.toml`

## Workflow
- Start with this playbook and command set.
- Use `references/doc_map.md` for command anchors and scope.
- Use `references/source_map.md` for function-level source and test entry points.
