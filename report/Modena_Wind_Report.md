# Is a Small Wind Turbine Worth It in Modena? A Computational Feasibility Study

*Author: Andrea Piciuolo — [International School of Modena / 18/06/2026]*

> **Read me first (delete before submitting).** This is a working draft built from a
> computational model. Two things make it *yours* and must be done before you use it:
> (1) replace the estimated wind inputs with the real **Global Wind Atlas** values you look up
> for Modena, and (2) rewrite the prose in your own voice. Admissions readers value authentic
> writing far more than polish. Spots that need your input are marked **[your turn]**.

## Abstract

Small wind turbines are often installed in places where the wind simply isn't strong enough to
justify them. This project asks a concrete question for one real location — Modena, in the Italian
Po Valley — using a physics-based model built from scratch in Python: *how much electricity could
a small wind turbine realistically produce here, what is the best design, and is wind even the
right clean-energy choice?* Using a Weibull description of the local wind, a blade designed from
aerodynamic theory, and a Blade Element Momentum (BEM) performance model, I find that even an
optimally sized turbine within residential limits would generate only about **793 kWh per year**
(~29% of a typical Italian household), and that a modest rooftop solar array would produce roughly
**five times** more energy at the same site. The honest conclusion is that Modena is a poor wind
site and that solar is the appropriate technology here — a result that demonstrates the value of
feasibility analysis before building.

## 1. Motivation

Wind power scales with the **cube** of wind speed, so a site with half the wind produces only an
eighth of the power. This makes location, not hardware, the dominant factor for small turbines —
yet enthusiasts frequently install them in low-wind suburban settings and are disappointed. I
wanted to test, quantitatively and for my own city, whether a small wind turbine is a sensible
investment, and to practice the full engineering arc: model the resource, design the machine,
evaluate its performance, and compare it honestly against the alternative.

## 2. Site and data

Modena (44.65° N, 10.93° E) sits in the Po Valley, a semi-enclosed basin known for very weak
surface winds and frequent temperature inversions; measured average surface wind speeds are around
**2 m/s** at 10 m. The wind resource is described by a **Weibull distribution** with scale
parameter *A* and shape parameter *k*.

| Input | Value used | Source |
|---|---|---|
| Mean wind speed @10 m | ~2.0 m/s | Local climatology / Global Wind Atlas |
| Weibull scale *A* @10 m | 2.25 m/s | **[your turn: read from Global Wind Atlas]** |
| Weibull shape *k* | 1.8 | **[your turn: read from Global Wind Atlas]** |
| Wind-shear exponent α | 0.25 | Typical suburban terrain |
| Household electricity use | 2,700 kWh/yr | ARERA standard (Italy) |
| Grid carbon intensity | 0.31 kg CO₂/kWh | Italy, ~2024 |
| Solar specific yield | 1,350 kWh/kWp/yr | PVGIS (Modena), conservative |

Data tools: **Global Wind Atlas** (globalwindatlas.info) for the Weibull parameters, **NASA
POWER** (power.larc.nasa.gov) for cross-checking wind speeds, and **PVGIS** (pvgis.com) for the
solar comparison. **[your turn: insert the exact numbers and a screenshot from Global Wind Atlas.]**

## 3. Method

**Energy in the wind.** The available power is P = ½ ρ A v³ C_p, where ρ is air density, A the
rotor swept area, v the wind speed, and C_p the power coefficient (capped by the Betz limit of
0.593). I model a realistic power curve with cut-in (3 m/s), rated (11 m/s) and cut-out (25 m/s)
speeds.

**Annual energy.** Real wind varies, so I integrate the power curve against the Weibull
probability of each wind speed over all 8,760 hours of the year to obtain Annual Energy Production
(AEP). Wind speed is projected to hub height with the power-law wind-shear model
v(h) = v₁₀ (h/10)^α.

**Blade design.** Rather than assume an efficiency, I designed a blade. Using Betz-optimum theory
(including wake rotation) I computed the ideal chord and twist distribution along a three-bladed
rotor for a design tip-speed ratio of 7, then evaluated its real performance with a **Blade Element
Momentum** solver including aerodynamic drag and Prandtl tip-loss corrections.

**Optimisation.** I swept rotor diameter (1–6 m) and tower height (10–30 m), computing AEP for
each, and selected the best design subject to residential constraints (diameter ≤ 4 m, height
≤ 24 m).

**Alternative.** Finally I compared the best turbine against rooftop solar PV sized to a typical
home, using Modena's solar yield.

## 4. Results

The designed blade reaches a peak power coefficient of **C_p = 0.428 at a tip-speed ratio of
≈6.8** — well below the Betz limit, as expected once drag and tip losses are included, and typical
of a good small turbine. Using this efficiency, a 3 m rotor on an 18 m tower would produce about
347 kWh/yr in Modena (versus 284 kWh/yr under a generic C_p = 0.35 assumption).

The optimisation pushed the design to the edge of both residential limits, giving a recommended
rotor diameter of **4 m** on a **24 m** tower and an annual output of about **793 kWh/yr** — roughly
**29% of a typical Italian household's electricity**, avoiding ~246 kg of CO₂ per year. Crucially,
the optimum sits *on* the constraints rather than at an interior peak, which means the design is
**resource-limited**: the wind, not the machine, is the bottleneck.

| Result | Value |
|---|---|
| Peak C_p of designed blade | 0.428 (λ ≈ 6.8) |
| Best feasible design | 4 m rotor, 24 m tower |
| Annual energy (best) | ~793 kWh/yr |
| Share of one household | ~29% |
| CO₂ avoided | ~246 kg/yr |
| Rooftop solar, 3 kWp | ~4,050 kWh/yr |
| Solar advantage | ~5× the best turbine |

## 5. Recommendation

For a household in Modena, a small wind turbine is **not** a cost-effective clean-energy choice:
the local wind resource is too weak, and even an optimally sized turbine within practical limits
is out-produced roughly fivefold by a modest rooftop solar array that costs less and has no moving
parts. **Solar is the appropriate technology for this site.** A small wind turbine would only make
engineering sense at a substantially windier location — a coast, a ridge, or an open plain with
mean speeds above ~5–6 m/s, where the same machine would produce several thousand kWh per year.

## 6. Limitations

This is a model, not a field measurement, and its honesty depends on its assumptions. The wind
resource is represented by a single Weibull distribution rather than measured time series; the
airfoil is a simple analytic model rather than wind-tunnel data; the BEM solver omits dynamic and
3-D effects; the turbine is treated as operating at its optimal tip-speed ratio (variable-speed);
and I did not model turbulence, local obstacles, noise, permitting, or detailed cost and payback.
Each of these would refine the numbers but none would change the central finding, which is driven
by the dominant cube-law dependence on a weak wind resource. **[your turn: add one limitation you
personally find most important, and why.]**

## 7. Next steps

Natural extensions include measuring wind on-site for a few weeks and comparing to the model;
adding a cost model and payback-period analysis; running the same pipeline for several Italian
locations to map where small wind *does* pay off; and cross-validating the blade in the free
QBlade software. **[your turn: pick one and say why it interests you.]**

## Appendix — reproducibility

All results come from two Python notebooks that run free in Google Colab with no installation:
`Wind_Turbine_Modena_Day1.ipynb` (wind resource and energy model) and
`Wind_Turbine_Modena_Part2.ipynb` (blade aerodynamics, optimisation, solar comparison). Code and
figures are available at **[your turn: your GitHub link]**.

*Data: Global Wind Atlas (DTU/World Bank); NASA POWER (NASA Langley); PVGIS (EU JRC). Household and
grid figures: ARERA; Italian grid carbon intensity ~2024.*
