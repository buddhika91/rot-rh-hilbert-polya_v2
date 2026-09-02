# ROT–RH Hilbert–Pólya Operator

## v643 — Infinite prime colligation, squared-SUSY quotient, and ROT self-dual Ward parity lock

This repository contains the complete current ROT–RH Hilbert–Pólya research
release: the runnable v643 operator, the full v600–v643 analytic chain, the
published numerical certificates, automated tests, and the available legacy
program from v1 through v599.

The central discovery of this release is that a generic positive Ward
correction is **not** enough to repair the Hilbert–Pólya parity obstruction.
The successful finite mechanism is the ROT self-dual, parity-selective
completion

```math
W_{\mathrm{ROT}} = W_L + \Pi_- C_{2,L}\Pi_-
```

where

```math
\Pi_- = \frac{I-\Gamma}{2}
```

projects onto the spatial-odd sector and

```math
C_{2,L}=\sum_{p<e^L}\frac{(\log p)^2}{p}\left(2I-2\operatorname{Re}T_{2\log p}\right)\ge 0
```

is the connected double-winding thermofield Ward energy.

At the deepest published Galerkin level, this correction changes all four
tested parity gaps to large positive values without using known zeta-zero
ordinates and without evaluating zeta or Xi.

> [!IMPORTANT]
> This repository does **not** contain a proof of the Riemann hypothesis.
> It contains an analytic infinite-cutoff scattering construction, exact
> finite operator theorems, obstruction reductions, and strong finite
> numerical evidence for a new ROT-specific completion. The ROT parity lock,
> the infinite parity gap, and preservation of the Xi/Fredholm determinant
> remain to be proved.

---

## Contents

- [Scientific status](#scientific-status)
- [The Hilbert–Pólya target](#the-hilbertpólya-target)
- [The v643 operator construction](#the-v643-operator-construction)
- [The key discovery](#the-key-discovery)
- [Published v643 results](#published-v643-results)
- [The analytic chain](#the-analytic-chain)
- [What has been proved inside the construction](#what-has-been-proved-inside-the-construction)
- [What remains open](#what-remains-open)
- [Repository structure](#repository-structure)
- [Installation](#installation)
- [Running the operator](#running-the-operator)
- [Tests and validation](#tests-and-validation)
- [Legacy research archive](#legacy-research-archive)
- [How to read the project](#how-to-read-the-project)
- [Citation and license status](#citation-and-license-status)

---

## Scientific status

The release should be read at four distinct levels.

| Level | Current status |
|---|---|
| Finite algebra and matrix identities | Exact within the stated finite models |
| Infinite prime-scattering construction | Analytic `S_3` cutoff-independent limit constructed |
| ROT parity completion | Strong finite numerical evidence, conditional on a structural identification |
| Riemann hypothesis | **Not proved** |

The machine-readable v643 certificate explicitly records

`RH_proved = false`,

`full_cutoff_independence = false`,

`zero_ordinates_used = false`,

and

`zeta_xi_used = false`.

This claim boundary is part of the release and is tested automatically.

---

## The Hilbert–Pólya target

The Hilbert–Pólya program seeks a self-adjoint operator `H` whose real spectrum
encodes the imaginary parts of the nontrivial zeros of the Riemann zeta
function. In an ideal realization,

```math
\operatorname{spec}(H)=\left\{\gamma:\xi\!\left(\frac12+i\gamma\right)=0\right\}
```

and an appropriately regularized spectral determinant satisfies

```math
\det_{\mathrm{reg}}(H-E)=N(E)\,\Xi(E)
```

where `N(E)` is explicit and nonzero in the relevant domain.

Self-adjointness would force the spectral parameter `E` to be real. If the
determinant identity were exact and exhaustive, the corresponding zeta zeros
would lie on

```math
\operatorname{Re}(s)=\frac12
```

The difficulty is not producing a finite matrix whose eigenvalues approximate
known zeros. The difficult requirements are simultaneous:

1. construct the operator without inserting the zeros;
2. prove self-adjointness on the correct domain or quotient;
3. remove the arithmetic cutoff analytically;
4. prevent spectral pollution;
5. prove the exact Xi/Fredholm identity;
6. show that any added physical completion preserves that identity.

The ROT–RH program attacks these requirements through arithmetic Weil forms,
recursive dilation, conservative prime scattering, supersymmetric quotient
geometry, and an observer-boundary Ward mechanism.

---

# The v643 operator construction

## 1. Finite observation window

Fix a symmetric observation window

```math
x\in[-L,L]
```

and a Fourier–Galerkin depth `M`. The basis labels are

```math
k=-M,\ldots,M
```

with frequencies

```math
\omega_k=\frac{\pi k}{L}
```

The normalized window basis may be written as

```math
\phi_k(x)=\frac{e^{i\omega_k x}}{\sqrt{2L}}
```

Its real-axis transform in the code is

```math
F_k(r)=\sqrt{2L}\,\operatorname{sinc}\!\left(\frac{(r+\omega_k)L}{\pi}\right)
```

For complex spectral argument `z`, the same transform is evaluated by

```math
F_k(z)=\frac{2\sinh\!\left(i(\omega_k+z)L\right)}{i(\omega_k+z)\sqrt{2L}}
```

with the removable zero handled explicitly.

## 2. Pole-neutral physical space

The completed-zeta architecture carries distinguished pole directions at

```math
z=+\frac{i}{2}
```

and

```math
z=-\frac{i}{2}
```

The finite physical space is restricted by

```math
\widehat f\!\left(+\frac{i}{2}\right)=0
```

```math
\widehat f\!\left(-\frac{i}{2}\right)=0
```

If `C_L` is the two-row matrix of these evaluations, the code computes an
orthonormal null-space matrix `Z_L` satisfying

```math
C_L Z_L=0
```

Every archimedean, arithmetic, reflection, translation, and Ward operator is
then compressed to this pole-neutral space.

For `2M+1` initial Fourier modes, the generic pole-neutral dimension is

```math
\dim E_{L,M}=2M-1
```

This projection is structural. It is not fitted to zeros and it is not a
post-processing removal of unwanted eigenvalues.

## 3. Archimedean Weil contribution

The archimedean symbol is

```math
\Omega(r)=\operatorname{Re}\psi\!\left(\frac14+\frac{ir}{2}\right)-\log\pi
```

where `psi` is the digamma function.

Using Gauss–Legendre quadrature, the uncompressed Galerkin matrix is

```math
G_{jk}=\frac{1}{2\pi}\int \Omega(r)\,\overline{F_j(r)}F_k(r)\,dr
```

The pole-neutral archimedean block is

```math
G_L^{\mathrm{phys}}=Z_L^{*}G_LZ_L
```

The registered v643 calculation uses

`r_max = 320`

and

`r_points = 1200`.

## 4. Arithmetic prime-power contribution

The script constructs every prime-power atom `n=p^m` below the active
finite-window threshold and assigns the von Mangoldt weight

```math
\Lambda(n)=\log p\qquad(n=p^m)
```

The arithmetic coefficient is

```math
a_n=\frac{\Lambda(n)}{\sqrt n}
```

For translation distance

```math
a=\log n
```

let `R_a` denote the finite-window correlation matrix. Only atoms satisfying

```math
\log n<2L
```

can overlap the window and contribute.

The arithmetic block is

```math
Q_L=-2\sum_{\log n<2L}\frac{\Lambda(n)}{\sqrt n}\,Z_L^{*}R_{\log n}Z_L
```

The baseline pole-neutral arithmetic Weil metric is

```math
W_L=G_L^{\mathrm{phys}}+Q_L
```

The implementation explicitly Hermitianizes every physical matrix:

```math
H\longleftarrow\frac{H+H^{*}}{2}
```

## 5. Reflection parity

Spatial reflection acts on the Fourier basis by

```math
\Gamma\phi_k=\phi_{-k}
```

After pole-neutral compression,

```math
\Gamma_L=Z_L^{*}\Gamma Z_L
```

Numerically, the release verifies

```math
\Gamma_L^2=I
```

to floating-point tolerance.

The parity projectors are

```math
\Pi_+=\frac{I+\Gamma_L}{2}
```

```math
\Pi_-=\frac{I-\Gamma_L}{2}
```

For any Hermitian metric `W`, define

```math
\epsilon_+(W)=\lambda_{\min}\!\left(W\vert_{E_+}\right)
```

```math
\epsilon_-(W)=\lambda_{\min}\!\left(W\vert_{E_-}\right)
```

and

```math
\Delta(W)=\epsilon_-(W)-\epsilon_+(W)
```

The sign convention is important:

```math
\Delta(W)>0
```

means the odd sector lies above the even ground state.

## 6. Observer-boundary gauge projection

The v643 script also tests removal of the projected global Fourier zero mode.
If `e_0` denotes the original `k=0` vector, its pole-neutral projection is

```math
g_0=Z_L^{*}e_0
```

The observer complement is

```math
E_{\mathrm{obs}}=g_0^{\perp}
```

This implements the candidate interpretation of the global compact phase as
an observer-boundary gauge mode. Gauge-only and gauge-plus-Ward variants are
recorded separately from the parity lock.

## 7. Connected thermofield Ward energy

The v636–v637 thermofield analysis identifies the controlled connected
double-winding sector. In the finite translation representation its positive
quadratic energy is

```math
C_{2,L}=\sum_{p<e^L}\frac{(\log p)^2}{p}\left(2I-2\operatorname{Re}T_{2\log p}\right)
```

The condition `p<e^L` is equivalent to

```math
2\log p<2L
```

For each prime channel,

```math
2I-2\operatorname{Re}T_a\ge 0
```

so

```math
C_{2,L}\ge 0
```

The coefficient is fixed to

```math
\lambda_{\mathrm{ROT}}=1
```

It is not optimized against the parity gaps and is not fitted to zeta zeros.

## 8. ROT self-dual parity lock

The decisive modelling hypothesis identifies exact two-channel ROT exchange
with the spatial reflection involution `Gamma_L`.

After selecting the exchange-even observer channel, spatial-even states are
exchange matched while spatial-odd states are mismatched. The mismatch pays
the connected Ward energy.

The candidate metric is

```math
W_{\mathrm{ROT}} = W_L + \Pi_- C_{2,L}\Pi_-
```

Because `Pi_-` annihilates the even sector, the correction leaves the even
trial energy unchanged while lifting the dangerous odd sector.

## 9. Operators compared by the implementation

| Name | Operator |
|---|---|
| `baseline` | `W_L` |
| `gauge` | Observer projection of `W_L` |
| `ward` | `W_L+C_(2,L)` |
| `rot_combined` | Observer projection of `W_L+C_(2,L)` |
| `dual_lock` | `W_L+Pi_- C_(2,L) Pi_-` |
| `dual_lock_gauge` | Observer projection of the dual-lock operator |

This comparison exposes the central discovery: the result is not a generic
consequence of adding a positive matrix.

---

# The key discovery

## Ordinary Ward positivity is insufficient

At

```math
L=3.0
```

the baseline gap is

```math
\Delta(W_L)=-1.1515465972426486\times10^{-3}
```

while ordinary Ward addition gives

```math
\Delta(W_L+C_{2,L})=-2.8845446665977548\times10^{-2}
```

The positive Ward term makes the parity gap **more negative**.

Therefore the relevant mechanism is not

```math
\text{positive correction}\;\Longrightarrow\;\text{positive gap}
```

## The symmetry projection is the operative ingredient

For the same calculation, the ROT self-dual completion gives

```math
\Delta\!\left(W_L+\Pi_-C_{2,L}\Pi_-\right)=5.698699481755492
```

The finite data distinguish two statements:

`W_L+C_(2,L)` is not sufficient,

but

```math
W_L+\Pi_-C_{2,L}\Pi_-
```

is strongly positive in every deepest tested window.

This is why v643 is a **symmetry discovery**, not merely a regularization
experiment. The proposed ROT exchange/parity lock targets exactly the odd
sector that v639 identifies as the obstruction to nonnegative
squared-Hamiltonian inertia.

---

# Published v643 results

The registered grid uses

```math
L\in\{2.5,3.0,3.5,4.0\}
```

and

```math
M\in\{24,32,40,48\}
```

At `M=48`:

| `L` | Baseline gap | Ordinary Ward gap | ROT self-dual gap | ROT gain |
|---:|---:|---:|---:|---:|
| 2.5 | `-7.218992836233675e-05` | `+5.2362855648064865e-03` | `+3.300724160605198` | `+3.300796350533560` |
| 3.0 | `-1.1515465972426486e-03` | `-2.8845446665977548e-02` | `+5.698699481755492` | `+5.699851028352735` |
| 3.5 | `-2.1808279242335242e-02` | `+1.5898111380661994e-02` | `+7.928292717108358` | `+7.950100996350693` |
| 4.0 | `+3.7953826127814505e-02` | `+7.953249423854736e-02` | `+11.85058490907122` | `+11.812631082943405` |

The registered finite outcomes are:

- deepest ROT gaps positive: `4/4`;
- deepest ROT gaps improved over baseline: `4/4`;
- known zero ordinates used: `false`;
- zeta/Xi evaluations used: `false`.

## Galerkin-depth stability

| `L` | Full tested relative span | Last-step relative change |
|---:|---:|---:|
| 2.5 | `1.929e-03` | `1.736e-04` |
| 3.0 | `7.979e-02` | `5.622e-03` |
| 3.5 | `6.251e-02` | `2.011e-02` |
| 4.0 | `2.430e-02` | `4.413e-03` |

The preregistered stability gate was

```math
\text{last-step relative change}<0.02
```

Three windows pass. The `L=3.5` value is approximately `0.02011`, slightly
above the gate. The stored verdict is therefore

`ROT_SELF_DUAL_WARD_PARITY_LOCK_REQUIRES_MORE_CONVERGENCE`.

The release preserves this result rather than rounding the borderline case
into a pass.

The calculation does not establish uniform positivity as `M -> infinity`, all
`L`, commuting cutoff limits, the fundamental ROT derivation, Fredholm
protection, or RH.

---

# The analytic chain

## Phase I — Cutoff forcing and the RH-strength invariant: v600–v604

**v600** reduces the preconditioned Selberg forcing exactly to the
prime-minus-smooth discrepancy.

**v601** identifies recursive beta transport as the Legendre dual of the PNT
error exponent.

**v602** establishes, for the defined physical observable,

```math
\text{full physical cutoff zero type}\;\Longleftrightarrow\;\mathrm{RH}
```

This is an equivalence, not an RH proof.

**v603–v604** derive the exact centered Selberg recursion and show that the
needed bilinear square-root estimate is itself RH-strength.

## Phase II — Conservative prime physics: v605–v612

**v605** constructs an exact Selberg Nicolai map and reflected
detailed-balance action.

**v606** obtains exact finite self-dual prime/smooth factorization and finite
passivity.

**v607** gives every finite prime channel an exact inner/passive completion;
the remaining defect is carried by PNT centering.

**v608** proves the canonical prime coupling is Schatten class

```math
U-U_{\mathrm{free}}\in\mathcal S_3
```

**v609** closes the double-winding `m=2` sector away from zero energy.

**v610** proves the primitive prime and smooth quantile pair is relative
Hilbert–Schmidt and admits a `det_2` limit.

**v611** isolates the remaining linear primitive datum as determinant-line
holonomy.

### v612 — Infinite prime colligation

For each prime, set

```math
r_p=p^{-1/2}
```

and

```math
U_p=\begin{pmatrix}r_p&\sqrt{1-r_p^2}\\\sqrt{1-r_p^2}&-r_p\end{pmatrix}
```

The free coupler is

```math
U_0=\begin{pmatrix}0&1\\1&0\end{pmatrix}
```

On the direct sum of prime channels,

```math
U-U_{\mathrm{free}}\in\mathcal S_3
```

The prime delay generator is

```math
L_{\mathrm{delay}}e_p=(\log p)e_p
```

with

```math
Z(E)=e^{iE L_{\mathrm{delay}}}
```

The exact one-prime transfer is

```math
\Theta_p(z)=\frac{z-r_p}{1-r_pz}
```

At

```math
z_p(E)=e^{iE\log p}
```

the relative scattering channel is

```math
S_p(E)=\frac{1-r_pe^{-iE\log p}}{1-r_pe^{+iE\log p}}
```

For real energy,

```math
\lvert S_p(E)\rvert=1
```

The infinite operator obeys

```math
S(E)-I\in\mathcal S_3
```

and

```math
\sup_{E\in\mathbb R}\left\lVert S(E)-S_X(E)\right\rVert_3\longrightarrow0
```

The tail estimate is

```math
\left\lVert S-S_X\right\rVert_3=O\!\left(X^{-1/6}\right)
```

Therefore

```math
\det_3 S(E)
```

exists as a genuine cutoff-independent modified determinant. This is an
analytic infinite scattering object, but not yet the final single
self-adjoint Hilbert–Pólya Hamiltonian.

## Phase III — Self-adjoint dilation and the critical edge: v613–v619

**v613–v614** construct a self-adjoint feedback dilation and prove that finite
channel upgrades and phase-erasing self-dual doubling do not solve the
primitive spectral-shift problem.

**v615** identifies the PNT error with prime-versus-`Li` recursive-clock
spectral flow.

**v616** considers prime quantiles `p_n` and smooth quantiles `q_n` defined by

```math
\operatorname{Li}(q_n)=n
```

For

```math
K_p=\operatorname{diag}\!\left(\frac1{p_n}\right)
```

and

```math
K_0=\operatorname{diag}\!\left(\frac1{q_n}\right)
```

the relative pair satisfies

```math
K_p-K_0\in\mathcal S_1
```

Its Krein spectral shift is the prime-counting discrepancy.

**v617** identifies the fractional trace threshold with the rightmost-zero
exponent. The target

```math
\alpha_c=\frac12
```

is RH-equivalent and remains open.

**v618** introduces the critical shell action

```math
C(Y)=\int_Y^{Y+1}\left\lvert e^{-y/2}\left(\pi(e^y)-\operatorname{Li}(e^y)\right)\right\rvert^2\,dy
```

At exponential scale,

```math
\mathrm{RH}\;\Longleftrightarrow\;C(Y)=e^{o(Y)}
```

Equivalent forms include zero positive Hardy-`L^2` Lyapunov exponent,
square-root Krein-edge regularity, the fractional trace transition at `1/2`,
and zero positive Gaussian/Fock cutoff type.

**v619** rewrites the missing estimate as a half-order bulk/boundary bound.

## Phase IV — Removing disconnected work: v620–v622

**v620** finds a genuine positive localized Selberg contribution of size

```math
\sim Y\,C(Y)
```

but proves that the accompanying `P*P` work has both signs and cannot become a
fixed-positive-metric BPS gradient flow.

**v621** identifies `P*P` as a disconnected cumulant. It disappears from the
connected logarithmic determinant.

**v622** reduces the unresolved content to the one connected statement

```math
C(Y)=e^{o(Y)}
```

This is a major simplification, but the remaining statement is RH-equivalent.

## Phase V — Ward identities and no-go theorems: v623–v635

**v623** proves the connected determinant-line scale Ward identity is exactly
conservative and `L^2`-unitary, but conservation does not force the Hardy
bound.

**v624** supplies a counterexample: trace class, integer spectral flow,
compact holonomy, and Ward structure can coexist with positive Hardy Lyapunov
exponent.

**v625** identifies off-critical zeros with positive dilation resonances of
the connected determinant line.

**v626** proves that an abstract Ward/index principle cannot exclude those
resonances. The missing theorem must be arithmetic or representation specific.

**v627–v629** close local prime passivity and express the Weil form as

```math
\text{archimedean input}-\text{positive prime dwell energy}
```

The remaining issue is collective global passivity.

**v630–v633** obtain the prime Ruelle orbit trace and exact local dilations,
but show that a local Euler factor is an open resonance determinant rather than
a closed self-adjoint secular factor.

**v634–v635** isolate a primitive common-port Hagedorn barrier and show that
semilocal scaling gauge does not remove it.

## Phase VI — Thermofield reconstruction: v636–v638

**v636** reinterprets the primitive amplitude `p^(-1/2)` as thermofield
coherence with summable probability-level fluctuation cost.

**v637** proves that connected thermofield cumulants with

```math
k\ge2
```

are zero type. Only the one-point tadpole remains RH-strength. The controlled
`k=2` sector supplies the positive Ward energy used in v643.

**v638** proves prime and smooth thermofield states are quasi-equivalent while
the primitive detector remains anomalous. State-level cutoff independence is
separated from detector-level cutoff independence.

## Phase VII — Squared-SUSY Hilbert–Pólya theorem: v639

Let `D` be the parity-changing scaling generator and `T` a symmetric Weil
matrix satisfying

```math
[D,T]=\lvert\beta\rangle\langle\eta\rvert-\lvert\eta\rangle\langle\beta\rvert
```

Write

```math
T_+=T\vert_{E_+}
```

and

```math
T_-=T\vert_{E_-}
```

Let `epsilon_+` be the simple even ground energy with eigenvector `xi`
normalized by

```math
\langle\eta,\xi\rangle=1
```

Define

```math
Q_+=T_+-\epsilon_+I
```

```math
A=D^2\vert_{E_+}
```

```math
A'=A-\lvert A\xi\rangle\langle\eta\rvert
```

Then

```math
A'\xi=0
```

and

```math
Q_+A'=A'^{*}Q_+
```

Thus `A'` descends to a self-adjoint quotient operator `A''` on

```math
E_+/\mathbb C\xi
```

The exact determinant identity is

```math
\det(A''-z^2)=(-1)^N\det(D-z)\,\langle\eta,(D-z)^{-1}\xi\rangle
```

Define the corrected supercharge

```math
Sx=Dx-D\xi\langle\eta,x\rangle
```

With

```math
Q_-=T_- -\epsilon_+I
```

the exact quadratic identity is

```math
\langle x,Q_+A'x\rangle=\langle Sx,Q_-Sx\rangle
```

Sylvester inertia gives

```math
n_-(A'')=\#\{\text{odd eigenvalues of }T\text{ below }\epsilon_+\}
```

Therefore

```math
\Delta_-=\lambda_{\min}(T_-)-\epsilon_+>0
```

is sufficient for nonnegative squared-Hamiltonian inertia. The RH obstruction
has become an even/odd Weil parity problem.

## Phase VIII — Prolate parity transfer: v640–v641

For the prolate concentration operator,

```math
\delta_c=\lambda_0(c)-\lambda_1(c)>0
```

Its tunnelling defects satisfy

```math
\frac{1-\lambda_1(c)}{1-\lambda_0(c)}\sim\kappa c
```

The transfer inequality is

```math
\Delta(T)\ge a_c\delta_c-\lVert R_-\rVert-\left\lvert\langle k_c,R_+k_c\rangle\right\rvert
```

A Weil-to-prolate mismatch controlled at the even ground-state defect scale
would therefore force a positive parity gap. That arithmetic remainder
estimate is not proved.

## Phase IX — ROT self-dual Ward completion: v643

v643 introduces the ROT-specific connected Ward energy and locks recursive
channel exchange to spatial parity:

```math
W_{\mathrm{ROT}}=W_L+\Pi_-C_{2,L}\Pi_-
```

This directly targets the odd sector isolated by v639. The finite results are
strong; the structural derivation and infinite/Fredholm consequences remain
open.

---

# What has been proved inside the construction

Within the explicitly defined models and hypotheses, the repository contains
analytic or algebraic proofs that:

1. the infinite relative prime scattering perturbation is `S_3`;
2. finite prime cutoffs converge uniformly in `S_3` norm for real energy;
3. the modified scattering determinant `det_3 S(E)` exists;
4. the double-winding sector is cutoff independent away from zero energy;
5. the primitive prime/smooth pair is Hilbert–Schmidt;
6. the inverse-square relative pair is trace class;
7. the Krein spectral shift encodes prime-counting discrepancy;
8. the critical fractional trace exponent equals the rightmost-zero exponent;
9. disconnected Selberg `P*P` work is not the connected closure mechanism;
10. generic trace-class/Ward/index topology cannot force RH;
11. the v639 finite quotient has an exact determinant identity;
12. quotient negative inertia is exactly the odd-sector obstruction;
13. the prolate model has a strict even/odd gap and a transfer budget;
14. the finite v643 Ward energy is positive semidefinite;
15. the deepest registered v643 ROT gaps are positive in all four windows.

These results do not combine into RH until the open arrows below are closed.

---

# What remains open

## 1. Fundamental ROT derivation

The identification

```math
\text{ROT channel exchange}=\text{spatial reflection }\Gamma
```

must be derived from the covariant ROT action, observer boundary, or an exact
Ward identity. The derivation must explain why the completion is exactly

```math
\Pi_-C_{2,L}\Pi_-
```

with

```math
\lambda_{\mathrm{ROT}}=1
```

## 2. Infinite parity-gap theorem

Finite positive gaps must be promoted to a statement such as

```math
\inf_{L\ge L_0}\Delta(W_{\mathrm{ROT},L})>0
```

or a weaker limit theorem sufficient for v639. This requires control of
`M -> infinity`, `L -> infinity`, the compatibility of the limits, the
pole-neutral quotient, observer gauge, arithmetic tails, and the odd ground
state.

## 3. Fredholm protection

The Ward deformation must preserve the target determinant. A sufficient form
would be

```math
\det_{\mathrm F}(H_{\mathrm{ROT}}-z)=E(z)\,\Xi(z)
```

with

```math
E(z)\ne0
```

throughout the relevant domain.

Without this, the construction may define a positive self-adjoint operator but
not necessarily the Hilbert–Pólya operator for Xi.

## 4. Infinite Hilbert–Pólya closure

The v639 determinant and inertia theorem must pass to the infinite operator
with domain control and no spectral pollution. Only then could one identify
the real spectrum exactly with the nontrivial Xi zeros.

---

# Repository structure

```text
rot-rh-hilbert-polya-v643/
├── README.md
├── CHANGELOG.md
├── CITATION.cff
├── MANIFEST.sha256
├── pyproject.toml
├── requirements.txt
├── requirements-dev.txt
├── .github/workflows/tests.yml
├── src/rot_rh/
│   ├── __init__.py
│   └── operator_v643.py
├── scripts/run_v643.py
├── tests/
│   ├── conftest.py
│   ├── test_cli_smoke.py
│   ├── test_operator_properties.py
│   └── test_published_results.py
├── results/v643/
├── docs/
│   ├── THEORY.md
│   ├── PROOF_STATUS.md
│   ├── REPRODUCIBILITY.md
│   ├── HISTORY.md
│   ├── RELEASE_QA.md
│   ├── CURRENT_CHAIN_INDEX.md
│   ├── LEGACY_INDEX.md
│   └── theorems/
├── research/v600_v643/
└── legacy/
    ├── v001_v099/
    ├── v100_v199/
    ├── v200_v299/
    ├── v300_v399/
    ├── v400_v499/
    ├── v500_v599/
    └── original_archives/
```

## Clean v643 package

`src/rot_rh/operator_v643.py` contains prime generation, prime-power atoms,
Fourier transforms, pole-neutral projection, archimedean quadrature,
finite-window correlations, the baseline Weil metric, `C_2`, parity splitting,
observer-mode removal, all operator comparisons, convergence diagnostics,
certificate generation, and `tqdm` progress bars.

## Current chain

`research/v600_v643/` contains 126 preserved scripts, summaries, theorem notes,
and result artifacts, including positive constructions and no-go results.

## Legacy archive

`legacy/` contains 799 pre-v600 artifacts covering the native recursive
Hilbert–Pólya pipeline, Weyl and arithmetic tails, numerical cutoff removal,
PNT finite parts, dyadic shells, Selberg cascades, Riesz/Abel/heat/Fredholm
regulators, packet Fourier cancellation, Gaussian/Fock/Volterra programs,
contour and zero-density attacks, passivity, and historical releases.

## Published results

`results/v643/` contains the deepest-mode CSV, complete 16-case JSON summary,
generated theorem/status note, and original v643 bundle.

## Documentation

- `docs/THEORY.md` — mathematical construction;
- `docs/PROOF_STATUS.md` — claim-by-claim status;
- `docs/REPRODUCIBILITY.md` — environments and commands;
- `docs/HISTORY.md` — milestone map;
- `docs/RELEASE_QA.md` — compilation and test record;
- `docs/theorems/` — decisive theorem/certificate notes.

---

# Installation

## Linux or macOS

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
python -m pip install -e .
```

## Windows PowerShell

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -e .
```

Dependencies are Python `>=3.10`, NumPy `>=1.24`, SciPy `>=1.10`, and tqdm
`>=4.65`.

---

# Running the operator

## Full registered grid

```bash
rot-rh-v643 \
  --L-values 2.5,3.0,3.5,4.0 \
  --modes-values 24,32,40,48 \
  --rmax 320 \
  --rpoints 1200 \
  --prefix results/local/rot_rh_v643
```

## Fast smoke run

```bash
rot-rh-v643 \
  --L-values 2.5 \
  --modes-values 6,8 \
  --rmax 40 \
  --rpoints 64 \
  --prefix results/local/smoke
```

Each run writes

```text
<prefix>_SUMMARY.json
<prefix>_THEOREM.md
```

The JSON includes all operator variants, parity gaps, ground energies,
involution diagnostics, prime atoms, Ward spectrum, gauge overlap, convergence
statistics, zero-blindness flags, proof-status flags, and runtime.

Passing `--strict` returns a nonzero status unless every convergence gate
passes. The published grid intentionally does not receive the all-window strict
pass.

---

# Tests and validation

```bash
python -m unittest discover -s tests -v
```

The eight tests verify:

1. the small-prime sieve;
2. annihilation of both pole vectors;
3. reflection Hermiticity and involution;
4. positive semidefiniteness of `C_(2,L)`;
5. the parity-gap sign convention;
6. end-to-end CLI output;
7. CSV/JSON result agreement;
8. preservation of the scientific status flags.

All 590 Python files compile successfully. GitHub Actions runs the suite on
Python 3.11.

Verify the release inventory with

```bash
sha256sum -c MANIFEST.sha256
```

The tests validate software and artifact integrity. They are not proofs of the
missing infinite-dimensional statements.

---

# Legacy research archive

The legacy directory is included because negative results constrain the final
theory. Earlier routes often reduce to an RH-equivalent estimate, a finite-only
stability result, a disconnected cumulant, an open-channel determinant, a
generic Ward/topology principle with counterexamples, or a state-level
completion that leaves the primitive detector anomalous.

Preserving those scripts makes the transition to the v639–v643 parity
architecture auditable and prevents closed routes from being repeated.

Historical scripts retain their original command-line conventions and may
require outputs from earlier stages. Three syntax-only archival repairs are
documented in `docs/RELEASE_QA.md`; no numerical formula was changed.

---

# How to read the project

## Mathematical review

1. `docs/PROOF_STATUS.md`
2. `docs/THEORY.md`
3. `docs/theorems/v612_infinite_prime_colligation_s3.md`
4. `docs/theorems/v622_single_connected_obstruction.md`
5. `docs/theorems/v639_susy_squared_scaling_parity_gap.md`
6. `docs/theorems/v640_weil_prolate_transfer.md`
7. `docs/theorems/v641_prolate_parity_amplifier.md`
8. `docs/theorems/v643_rot_self_dual_ward_parity_lock.md`
9. the complete JSON under `results/v643/original_bundle/`

## Software review

1. `src/rot_rh/operator_v643.py`
2. `tests/test_operator_properties.py`
3. `tests/test_published_results.py`
4. `tests/test_cli_smoke.py`
5. `docs/REPRODUCIBILITY.md`
6. `.github/workflows/tests.yml`

## Research history

1. `docs/HISTORY.md`
2. `docs/CURRENT_CHAIN_INDEX.md`
3. `docs/LEGACY_INDEX.md`
4. the individual version certificates.

> [!NOTE]
> Historical files named `*_THEOREM.md` are internal theorem/certificate
> documents. The filename does not imply peer review or external acceptance.

---

# Current research target

The focused analytic target is

```math
\begin{aligned}
\text{ROT action}
&\Longrightarrow \text{exact exchange Ward identity}\\
&\Longrightarrow \Pi_-C_2\Pi_-\text{ with canonical coefficient}\\
&\Longrightarrow \text{infinite positive Weil parity gap}\\
&\Longrightarrow \text{nonnegative v639 squared-SUSY quotient}.
\end{aligned}
```

together with

```math
\det_{\mathrm F}(H_{\mathrm{ROT}}-z)=E(z)\,\Xi(z)
```

and

```math
E(z)\ne0
```

If both chains are proved without importing

```math
C(Y)=e^{o(Y)}
```

as an assumption, the architecture would have the ingredients needed for a
genuine Hilbert–Pólya conclusion.

---

# Citation and license status

Citation metadata is provided in `CITATION.cff` under the title

`ROT–RH Hilbert–Pólya Operator Construction v643`.

No software license is granted by this archive. Add an explicit license before
redistributing the repository or a modified version.

---

## Final status

`infinite cutoff-independent prime scattering: constructed`

`cutoff-independent modified determinant det_3: constructed`

`connected RH-strength obstruction: isolated`

`finite squared-SUSY Hilbert–Pólya quotient: exact`

`finite determinant identity: exact`

`parity obstruction/inertia dictionary: exact`

`prolate parity amplification mechanism: identified`

`finite ROT self-dual parity gaps: positive in 4/4 deepest tested windows`

`fundamental ROT parity-lock derivation: open`

`infinite ROT parity-gap theorem: open`

`Xi/Fredholm protection under ROT completion: open`

`Riemann hypothesis: not proved`
