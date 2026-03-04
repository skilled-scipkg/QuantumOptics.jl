# quantumoptics-jl source map: Inputs and Modeling

Use this map after checking `doc_map.md`.

## Topic query tokens
- `steadystate.master`
- `steadystate.eigenvector`
- `steadystate.liouvillianspectrum`
- `steadystate.iterative`
- `eigenstates`
- `eigenenergies`
- `simdiag`
- `qfunc`
- `wigner`
- `qfuncsu2`
- `wignersu2`

## Function-level source entry points
- `src/steadystate.jl` | `steadystate.master`, `steadystate.liouvillianspectrum`, `steadystate.eigenvector`
- `src/steadystate_iterative.jl` | `steadystate.iterative`, `steadystate.iterative!`, `_linmap_liouvillian`
- `src/spectralanalysis.jl` | `detect_diagstrategy`, `KrylovDiag`, `eigenstates`, `eigenenergies`, `simdiag`
- `src/phasespace.jl` | `qfunc`, `wigner`, `qfuncsu2`, `wignersu2`
- `src/timecorrelations.jl` | `correlation2spectrum` for FFT-based spectral conversion in analysis workflows
- `src/QuantumOptics.jl` | module wiring for `steadystate`, spectral, and phase-space exports

## Validation test entry points
- `test/test_steadystate.jl` | cross-method steady-state agreement, analytic cavity checks, mutation semantics for iterative solvers
- `test/test_spectralanalysis.jl` | dense/sparse eigen behavior, non-Hermitian handling, and `simdiag` correctness
- `test/test_phasespace.jl` | coherent/Fock/SU(2) phase-space references and normalization checks
- `test/test_timecorrelations.jl` | FFT spectrum conversion checks used in analysis post-processing

## Fast source navigation
- `rg -n "function (master|liouvillianspectrum|eigenvector|iterative!?|eigenstates|eigenenergies|simdiag|qfunc|wigner|qfuncsu2|wignersu2)" src`
- `rg -n "@testitem|steadystate|spectralanalysis|phasespace|correlation2spectrum" test`

## Quick validation command set
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"steadystate\")"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"spectralanalysis\")"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"phasespace\")"`
