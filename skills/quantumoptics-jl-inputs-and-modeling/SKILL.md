---
name: quantumoptics-jl-inputs-and-modeling
description: Core skill for steady-state method routing, spectral/eigen analysis, and phase-space workflows in QuantumOptics.jl.
---

# quantumoptics-jl: Inputs and Modeling

## High-Signal Playbook

### Route conditions
- Use this skill for basis/operator setup that leads to steady-state solving, eigenspectrum analysis, or phase-space diagnostics.
- Route to `quantumoptics-jl-license` for transient trajectories, stochastic solvers, semiclassical propagation, or correlation pipelines.
- Route to `quantumoptics-jl-build-and-install` for setup and test execution issues.

### Triage questions
- Do you need direct steady state or time evolution first?
- Is the problem dense/small or sparse/large?
- Is `rates` absent, vector-valued, or matrix-valued?
- Is solver robustness (`steadystate.master`/`eigenvector`) or throughput (`steadystate.iterative`) the priority?
- Do you need a few low eigenpairs or a full decomposition?
- Are phase-space diagnostics Fock-based (`qfunc`/`wigner`) or spin-based (`qfuncsu2`/`wignersu2`)?

### Canonical workflow
1. Define basis/operators and verify basis compatibility early.
2. Choose steady-state strategy:
   - `steadystate.master` for long-time integration with trace-distance stop.
   - `steadystate.eigenvector` and `steadystate.liouvillianspectrum` for Liouvillian-mode workflows.
   - `steadystate.iterative` or `steadystate.iterative!` for iterative linear solves.
3. For spectral analysis, use `eigenstates`/`eigenenergies`; tune sparse controls (`n`, `krylovdim`, `v0`) when needed.
4. For commuting Hermitian operator sets, use `simdiag`.
5. For phase-space diagnostics, evaluate `qfunc` or `wigner` on a grid and refine resolution until features stabilize.
6. Cross-check at least two methods before final reporting.

### Minimal working example
```julia
using QuantumOptics

b = FockBasis(20)
a = destroy(b)
H = 0.6 * (a + dagger(a))
J = [sqrt(0.4) * a]

_, rho_t = steadystate.master(H, J; tol=1e-4)
rho_e = steadystate.eigenvector(H, J)
rho_i = steadystate.iterative(H, J)

@assert tracedistance(rho_t[end], rho_e) < 5e-3
@assert tracedistance(rho_t[end], rho_i) < 5e-3
```

```julia
using QuantumOptics

b = FockBasis(30)
psi = coherentstate(b, 0.3 + 0.2im)
xs = collect(-2.0:0.2:2.0)
ys = collect(-2.0:0.2:2.0)

Q = qfunc(psi, xs, ys)
W = wigner(psi, xs, ys)
evals, states = eigenstates(dense(number(b)), 5)

@assert size(Q) == (length(xs), length(ys))
@assert size(W) == (length(xs), length(ys))
@assert length(evals) == length(states) == 5
```

### Simulation-readiness command
```bash
julia --project=. -e "using QuantumOptics; b=FockBasis(12); a=destroy(b); H=0.6*(a+dagger(a)); J=[sqrt(0.4)*a]; _, rho_t = steadystate.master(H, J; tol=1e-4); rho_e = steadystate.eigenvector(H, J); @assert tracedistance(rho_t[end], rho_e) < 5e-3; println(\"steadystate-smoke-ok\")"
```

### Pitfalls and fixes
- `steadystate.iterative!` mutates `rho0`; pass a copy when input state reuse matters.
- `steadystate.eigenvector` fails if no near-zero Liouvillian eigenvalue exists; recheck model/rates and numerical settings.
- Sparse eigensolvers can fail for poor `nev`/`which`/`krylovdim`; adjust settings or compare with dense operators.
- Spectral routines on non-Hermitian operators can emit warnings; disable warnings only when intentional.
- `qfunc` and `wigner` require Fock basis workflows; use SU(2) variants for `SpinBasis`.
- Coarse phase-space grids can hide structure or negativity; increase sampling density.
- Validate `tr(rho) approx 1` for steady-state outputs.

### Validation checkpoints
- `steadystate.master`, `steadystate.eigenvector`, and `steadystate.iterative` agree within tolerance.
- Driven-cavity photon number matches analytic reference in `test/test_steadystate.jl`.
- Spectral outputs match dense/sparse expectations in `test/test_spectralanalysis.jl`.
- Phase-space outputs match coherent/Fock/SU(2) references in `test/test_phasespace.jl`.

## Primary documentation references
- `README.md` (project structure and external canonical docs/examples)
- `CLAUDE.md` (module overview and test command patterns)

## Canonical external docs/examples
- `https://docs.qojulia.org/`
- `https://github.com/qojulia/QuantumOptics.jl-documentation`
- `https://github.com/qojulia/QuantumOptics.jl-examples`

## Workflow
- Start with this playbook.
- Use `references/doc_map.md` for local command/context inventory.
- Escalate to mapped source/tests in `references/source_map.md` for behavior-level checks.
