# SphericalHarmonicExpansions.jl

Julia package for real spherical-harmonic expansions in Cartesian coordinates.
This repository contains version **0.1.5** with the numerical-stability extensions developed for the research project *Numerisch genaue und stabile Erzeugung sphärischer Harmonischer hoher Ordnung*.

The original package authors are listed in `Project.toml`; the original MIT license is preserved in `LICENSE`.

## Numerical-stability extensions

- exact integer combinatorics with `BigInt` before conversion to floating-point arithmetic,
- logarithmic normalization to avoid avoidable overflow in normalization products,
- type-generic construction for `Float64` and `BigFloat`,
- scaling modes `:distributed` and `:adaptive`,
- explicit-precision constructors such as `ylm_bigfloat`,
- measured precision selection through `recommend_precision` and `evaluate_with_precision`.

## Precision-policy scope

The bundled file `data/precision_policy_v1.csv` contains the measured policy for

- `basis = :ylm`,
- `variant = :distributed`,
- `mode = :worst_case`,
- all measured orders `m` and the documented evaluation points.

Recommendations are returned only for measured degrees and tolerances. No interpolation or extrapolation is performed, and the measurements are not a whole-sphere accuracy guarantee.

## Basic use

```julia
using SphericalHarmonicExpansions

@polyvar x y z

p64 = ylm_typed(Float64, 8, 2, x, y, z)
p256 = ylm_bigfloat(8, 2, x, y, z; precision=256)

rec = recommend_precision(l=160, tolerance=1e-8)
```

`evaluate_with_precision` can select the measured arithmetic and evaluate directly.

## Repository layout

- `src/` — package implementation
- `test/` — package tests
- `data/precision_policy_v1.csv` — measured precision policy
- `.github/workflows/ci.yml` — CI workflow

The final project validation recorded **203/203 package tests passed**.
