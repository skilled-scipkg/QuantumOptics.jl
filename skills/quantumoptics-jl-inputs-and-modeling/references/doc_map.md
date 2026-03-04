# quantumoptics-jl documentation map: Inputs and Modeling

Generated from local documentation roots:
- `.`
- `test`

Total docs grouped in this topic: 5

## File inventory
- `README.md` | purpose: project structure and external docs/examples pointers
- `CLAUDE.md` | purpose: module/test inventory and command snippets
- `test/test_steadystate.jl` | purpose: steady-state method behavior contracts and analytic references
- `test/test_spectralanalysis.jl` | purpose: eigen/simdiag behavior contracts
- `test/test_phasespace.jl` | purpose: qfunc/wigner/SU(2) phase-space behavior contracts

## Operational command anchors
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"steadystate\")"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"spectralanalysis\")"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"phasespace\")"`

## External canonical docs/examples (not vendored in this checkout)
- `https://docs.qojulia.org/`
- `https://github.com/qojulia/QuantumOptics.jl-documentation`
- `https://github.com/qojulia/QuantumOptics.jl-examples`
