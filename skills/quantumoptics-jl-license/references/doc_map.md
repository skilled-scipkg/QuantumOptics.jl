# quantumoptics-jl documentation map: Time Evolution, Noise, and Correlations

Generated from local documentation roots:
- `.`
- `test`

Total docs grouped in this topic: 8

## File inventory
- `README.md` | purpose: project overview; canonical external docs/examples pointers
- `CLAUDE.md` | purpose: module map and development/test command patterns
- `test/test_timeevolution_master.jl` | purpose: deterministic Lindblad behavior contracts
- `test/test_timeevolution_mcwf.jl` | purpose: trajectory consistency and reproducibility checks
- `test/test_timeevolution_tdops.jl` | purpose: static-vs-dynamic operator guardrails
- `test/test_timecorrelations.jl` | purpose: two-time correlation and spectrum conversion checks
- `test/test_stochastic_schroedinger.jl`, `test/test_stochastic_master.jl` | purpose: stochastic solver behavior contracts
- `test/test_semiclassical.jl`, `test/test_stochastic_semiclassical.jl`, `test/test_timeevolution_bloch_redfield.jl` | purpose: advanced simulation branches

## Operational command anchors
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"timeevolution\")"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"timecorrelations\")"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"stochastic\")"`
- `julia --project=. -e "using TestItemRunner; @run_package_tests filter=ti->contains(string(ti.name), \"bloch_redfield\")"`

## External canonical docs/examples (not vendored in this checkout)
- `https://docs.qojulia.org/`
- `https://github.com/qojulia/QuantumOptics.jl-documentation`
- `https://github.com/qojulia/QuantumOptics.jl-examples`
