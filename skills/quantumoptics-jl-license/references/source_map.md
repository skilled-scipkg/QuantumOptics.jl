# quantumoptics-jl source map: Time Evolution, Noise, and Correlations

Use this map after checking `doc_map.md`.

## Topic query tokens
- `schroedinger`
- `master`
- `mcwf`
- `*_dynamic`
- `stochastic`
- `semiclassical`
- `bloch_redfield`
- `correlation`
- `spectrum`
- `correlation2spectrum`
- `skiptimechecks`

## Function-level source entry points
- `src/schroedinger.jl` | `timeevolution.schroedinger`, `timeevolution.schroedinger_dynamic`, `dschroedinger!`
- `src/master.jl` | `timeevolution.master_h`, `timeevolution.master_nh`, `timeevolution.master`, `timeevolution.master_dynamic`, `timeevolution.master_nh_dynamic`
- `src/mcwf.jl` | `timeevolution.mcwf_h`, `timeevolution.mcwf_nh`, `timeevolution.mcwf`, `timeevolution.mcwf_dynamic`, `timeevolution.mcwf_nh_dynamic`, `diagonaljumps`
- `src/timeevolution_base.jl` | `integrate`, `_check_const`, `@skiptimechecks`, `QO_CHECKS`
- `src/time_dependent_operators.jl` | `TimeDependentSum` support and operator adaptation helpers
- `src/stochastic_schroedinger.jl` | `stochastic.schroedinger`, `stochastic.schroedinger_dynamic`
- `src/stochastic_master.jl` | `stochastic.master`, `stochastic.master_dynamic`
- `src/semiclassical.jl` | `semiclassical.schroedinger_dynamic`, `semiclassical.master_dynamic`, `semiclassical.mcwf_dynamic`
- `src/stochastic_semiclassical.jl` | `stochastic.schroedinger_semiclassical`, `stochastic.master_semiclassical`
- `src/bloch_redfield_master.jl` | `timeevolution.bloch_redfield_tensor`, `timeevolution.master_bloch_redfield`
- `src/timecorrelations.jl` | `timecorrelations.correlation`, `timecorrelations.correlation_dynamic`, `timecorrelations.spectrum`, `timecorrelations.correlation2spectrum`
- `src/QuantumOptics.jl` | module export and include graph for `timeevolution`, `stochastic`, and `semiclassical`

## Validation test entry points
- `test/test_timeevolution_schroedinger.jl` | deterministic Schrodinger evolution behavior
- `test/test_timeevolution_master.jl` | Lindblad master behavior and rate handling
- `test/test_timeevolution_mcwf.jl` | trajectory dynamics, seeding, and ensemble checks
- `test/test_timeevolution_tdops.jl` | dynamic-operator route correctness and guardrails
- `test/test_timeevolution_pumpedcavity.jl` | end-to-end driven cavity validations, including `@skiptimechecks`
- `test/test_timecorrelations.jl` | correlation/spectrum consistency and `correlation2spectrum` error checks
- `test/test_stochastic_schroedinger.jl`, `test/test_stochastic_master.jl` | stochastic solver behavior
- `test/test_semiclassical.jl`, `test/test_stochastic_semiclassical.jl` | hybrid quantum-classical dynamics
- `test/test_timeevolution_bloch_redfield.jl` | Bloch-Redfield tensor and propagation checks

## Fast source navigation
- `rg -n "function (schroedinger|master|mcwf|correlation|spectrum|correlation2spectrum|bloch_redfield_tensor|master_bloch_redfield)" src`
- `rg -n "@skiptimechecks|QO_CHECKS|diagonaljumps|noise_processes|noise_prototype_classical" src test`
- `rg -n "@testitem" test/test_timeevolution_*.jl test/test_timecorrelations.jl test/test_stochastic_*.jl test/test_semiclassical.jl`

## Quick validation command set
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"timeevolution\")"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"timecorrelations\")"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"stochastic\")"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"bloch_redfield\")"`
