---
name: quantumoptics-jl-license
description: Core skill for QuantumOptics.jl deterministic and stochastic dynamics, semiclassical and Bloch-Redfield workflows, and time-correlation/spectrum analysis (legacy skill id name retained).
---

# quantumoptics-jl: Time Evolution, Noise, and Correlations (legacy skill id: license)

## High-Signal Playbook

### Route conditions
- Use this skill for `timeevolution` APIs (`schroedinger`, `master`, `mcwf`), dynamic operators, stochastic solvers, semiclassical propagation, Bloch-Redfield workflows, and two-time correlations/spectra.
- Route to `quantumoptics-jl-inputs-and-modeling` for steady-state, eigenspectrum, and phase-space workflows.
- Route to `quantumoptics-jl-build-and-install` for environment and test-runner issues.

### Triage questions
- Is the state pure (`Ket`), mixed (`Operator`), or semiclassical (`semiclassical.State`)?
- Are generators static or time dependent?
- Do you need deterministic (`master`) or trajectory/stochastic behavior (`mcwf`, `stochastic.*`)?
- Do you need reproducible randomness (`seed`, `JumpRNGState`)?
- Are jump rates absent, vector-valued, or matrix-valued?
- Do you need full states or only derived outputs (`fout`)?
- Are you validating against master dynamics, conserved quantities, or correlation/spectrum output?

### Canonical workflow
1. Build basis/operators and initial state.
2. Pick deterministic solver family: `timeevolution.schroedinger`, `timeevolution.master`, `timeevolution.mcwf`.
3. For time-dependent generators, switch to `*_dynamic` solvers.
4. For stochastic noise models, use `stochastic.schroedinger`, `stochastic.master`, or their dynamic variants.
5. For hybrid quantum-classical models, use `semiclassical.schroedinger_dynamic`, `semiclassical.master_dynamic`, `semiclassical.mcwf_dynamic`; use `stochastic.schroedinger_semiclassical` or `stochastic.master_semiclassical` when noise is also required.
6. For Bloch-Redfield models, build `timeevolution.bloch_redfield_tensor` and propagate with `timeevolution.master_bloch_redfield`.
7. For two-time observables, use `timecorrelations.correlation`, `timecorrelations.correlation_dynamic`, `timecorrelations.spectrum`, and `timecorrelations.correlation2spectrum`.
8. Validate behavior with mapped tests before finalizing guidance.

### Minimal working examples
```julia
using QuantumOptics

b = SpinBasis(1//2)
psi0 = spinup(b)
H = 0.7 * sigmax(b) + 0.2 * sigmaz(b)
T = collect(0.0:0.1:2.0)

_, psit = timeevolution.schroedinger(T, psi0, H)
_, rhot = timeevolution.master(T, psi0, H, [])
_, psim = timeevolution.mcwf(T, psi0, H, []; seed=UInt(1))

@assert length(psit) == length(rhot) == length(psim) == length(T)
```

```julia
using QuantumOptics
using StochasticDiffEq

b = SpinBasis(1//2)
gamma = 0.5
H = gamma * (sigmap(b) + sigmam(b))
Hs = [0.5 * gamma * sigmaz(b)]
psi0 = spindown(b)
T = collect(0.0:1.0:10.0)

_, psi_stoch = stochastic.schroedinger(T, psi0, H, Hs; dt=1/30, alg=StochasticDiffEq.EulerHeun())
@assert length(psi_stoch) == length(T)
```

```julia
using QuantumOptics

b = FockBasis(12)
a = destroy(b)
H = number(b)
J = [sqrt(0.3) * a]
rho0 = dm(coherentstate(b, 0.2 + 0.1im))
T = collect(0.0:0.2:4.0)

corr = timecorrelations.correlation(T, rho0, H, J, dagger(a), a)
omega, spec = timecorrelations.correlation2spectrum(T, corr)

@assert length(corr) == length(T)
@assert length(omega) == length(spec)
```

### Simulation-readiness commands
```bash
julia --project=. -e "using QuantumOptics; b=SpinBasis(1//2); psi0=spinup(b); H=sigmax(b); T=collect(0.0:0.1:1.0); _, psit = timeevolution.schroedinger(T, psi0, H); @assert length(psit)==length(T); println(\"timeevolution-smoke-ok\")"
```

```bash
julia --project=. -e "using QuantumOptics; b=FockBasis(8); a=destroy(b); H=number(b); J=[sqrt(0.2)*a]; rho0=dm(coherentstate(b, 0.2)); T=collect(0.0:0.2:2.0); corr = timecorrelations.correlation(T, rho0, H, J, dagger(a), a); @assert length(corr)==length(T); println(\"correlation-smoke-ok\")"
```

### Pitfalls and fixes
- Static solvers on time-dependent generators throw `ArgumentError`; use dynamic solvers.
- In `fout`, solver-owned state objects are transient; treat them as read-only.
- `mcwf` does not accept matrix `rates`; use `diagonaljumps(rates, J)` first.
- Dynamic callbacks must return tuple arity expected by each solver family.
- Stochastic dynamic APIs may require explicit `noise_processes` for nontrivial callback outputs.
- `stochastic.*_semiclassical` requires `noise_prototype_classical` when combining quantum and classical noise.
- `bloch_redfield_tensor` expects `a_ops` pairs of interaction operator and spectral callback function.
- `@skiptimechecks` is for controlled performance tuning only; avoid default use.
- `correlation2spectrum` requires equal lengths and equidistant `tspan`.

### Validation checkpoints
- Deterministic master output matches references in `test/test_timeevolution_master.jl`.
- `mcwf`, `mcwf_h`, and `mcwf_nh` converge to Schr behavior for `J=[]` and agree with master in ensemble checks.
- Dynamic operator paths agree between callback and `TimeDependentSum` forms.
- Stochastic and semiclassical solvers pass invariants in `test/test_stochastic_*.jl` and `test/test_semiclassical.jl`.
- Bloch-Redfield tensor evolution passes checks in `test/test_timeevolution_bloch_redfield.jl`.
- Correlation/spectrum pipelines agree between direct and FFT conversion tests.

## Primary documentation references
- `README.md` (project + external canonical docs/examples)
- `CLAUDE.md` (module/test layout and development commands)

## Canonical external docs/examples
- `https://docs.qojulia.org/`
- `https://github.com/qojulia/QuantumOptics.jl-documentation`
- `https://github.com/qojulia/QuantumOptics.jl-examples`

## Workflow
- Start from this playbook.
- Use `references/doc_map.md` for local document and command anchors.
- Use `references/source_map.md` for function-level source and validation test entry points.
