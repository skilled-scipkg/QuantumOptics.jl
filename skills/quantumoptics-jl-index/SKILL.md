---
name: quantumoptics-jl-index
description: Router for QuantumOptics.jl skills with docs-first flow, explicit escalation into source/tests, and practical simulation start checks.
---

# quantumoptics-jl Skills Index

## Route the request
- `quantumoptics-jl-build-and-install`: environment setup, dependency instantiation, CPU/GPU/JET test runs, benchmark execution, and runtime smoke checks.
- `quantumoptics-jl-license`: time evolution (Schroedinger/master/MCWF), stochastic and semiclassical dynamics, Bloch-Redfield workflows, and time-correlation/spectrum routines.
- `quantumoptics-jl-inputs-and-modeling`: steady-state method selection, eigenspectrum tools, and phase-space workflows.

## Docs first, then source/tests
- Start with `README.md` and `CLAUDE.md`.
- Use external canonical docs/examples when local docs are thin:
  - `https://docs.qojulia.org/`
  - `https://github.com/qojulia/QuantumOptics.jl-documentation`
  - `https://github.com/qojulia/QuantumOptics.jl-examples`
- Escalate to source only for implementation behavior or edge cases, then confirm with matching tests.

## Core source entry points
- `src/QuantumOptics.jl` (module wiring and exports)
- `src/timeevolution_base.jl`, `src/schroedinger.jl`, `src/master.jl`, `src/mcwf.jl`
- `src/stochastic_schroedinger.jl`, `src/stochastic_master.jl`, `src/stochastic_semiclassical.jl`
- `src/semiclassical.jl`, `src/bloch_redfield_master.jl`, `src/timecorrelations.jl`
- `src/steadystate.jl`, `src/steadystate_iterative.jl`, `src/spectralanalysis.jl`, `src/phasespace.jl`

## Core test entry points
- `test/runtests.jl` (global filter + GPU/JET toggles)
- `test/test_timeevolution_*.jl`, `test/test_timecorrelations.jl`
- `test/test_stochastic_*.jl`, `test/test_semiclassical.jl`, `test/test_timeevolution_bloch_redfield.jl`
- `test/test_steadystate.jl`, `test/test_spectralanalysis.jl`, `test/test_phasespace.jl`

## Startup sanity sequence
1. `julia --project=. -e "using Pkg; Pkg.instantiate()"`
2. `julia --project=. -e "using Pkg; Pkg.test()"`
3. `julia --project=. -e "using QuantumOptics; b=SpinBasis(1//2); H=sigmax(b); psi0=spindown(b); T=collect(0.0:0.1:1.0); t, psit = timeevolution.schroedinger(T, psi0, H); @assert length(t)==length(psit); println(\"smoke-ok\")"`

## Escalation workflow
1. Route to one topic skill and follow its playbook.
2. Open the matching topic doc map:
   - `skills/quantumoptics-jl-build-and-install/references/doc_map.md`
   - `skills/quantumoptics-jl-license/references/doc_map.md`
   - `skills/quantumoptics-jl-inputs-and-modeling/references/doc_map.md`
3. Open the matching topic source map:
   - `skills/quantumoptics-jl-build-and-install/references/source_map.md`
   - `skills/quantumoptics-jl-license/references/source_map.md`
   - `skills/quantumoptics-jl-inputs-and-modeling/references/source_map.md`
4. Use targeted symbol search only for unresolved ambiguity: `rg -n "<symbol_or_keyword>" src test`.
