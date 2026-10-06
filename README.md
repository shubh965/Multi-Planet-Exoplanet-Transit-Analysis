# Multi-Planet Exoplanet Transit Analysis: Kepler-10

An end-to-end photometric pipeline using **Lightkurve** and Astropy's **Box Least Squares (BLS)** to detect, detrend, refine, and characterize multiple transiting exoplanets from Kepler Long-Cadence (~30 min) and Short-Cadence (~1 min) data.

## Overview

The notebook analyzes the **Kepler-10** system the first Kepler star confirmed to host a rocky exoplanet and blindly recovers both of its planets from raw archival photometry without any prior orbital knowledge.

**Pipeline stages:**
1. Query and download Kepler MAST archive data (long and short cadence)
2. Stitch quarters, remove outliers, and flatten stellar variability (Savitzky-Golay)
3. Run BLS on long-cadence data for a coarse period detection
4. Refine using 1-minute short-cadence data with a dense parameter grid
5. Mask Kepler-10b transits; repeat search on residuals for Kepler-10c
6. Phase-fold and overlay BLS transit models for both planets

## Results

Parameters recovered via blind BLS detection, compared against official literature values (Batalha et al. 2011; Fressin et al. 2011):

| Parameter | 10b (Calculated) | 10b (Literature) | 10c (Calculated) | 10c (Literature) |
|---|---|---|---|---|
| **Orbital Period** | 0.837491 d | 0.837495 d | 45.294248 d | 45.29485 d |
| **Transit Duration** | 0.073 d (1.75 h) | 0.0754 d (1.81 h) | 0.268 d (6.43 h) | 0.286 d (6.86 h) |
| **Transit Depth** | 168 ppm | ~152-168 ppm | 380 ppm | ~376 ppm |

Period agreement is better than **0.001%** for both planets. Duration and depth discrepancies are expected given the box-shaped BLS model approximation vs. the physical limb-darkened transit profile used in discovery papers.

> **Note on depth errors:** BLS models a flat-bottomed box and cannot account for limb darkening or impact parameter, so some systematic underestimation of depth is expected. The recovered depth for Kepler-10c (~380 ppm) aligns closely with the ~376 ppm value reported by Fressin et al. (2011), while the NASA Exoplanet Archive's geometric estimate (~543 ppm from $R_p/R_\star$) represents a different quantity. Duration errors (~4-7%) are consistent with BLS grid resolution and the coarse box-model approximation of ingress/egress shape.

## Requirements

- Python 3.8+
- `lightkurve`
- `astropy`
- `numpy`
- `matplotlib`

```bash
pip install lightkurve astropy numpy matplotlib
```

## Usage

Open and run `main.ipynb` top to bottom. The notebook queries the MAST archive directly no manual data download required.

To adapt for a different target, modify the search string in the archive query cells and adjust the BLS period/duration search bounds accordingly.

## References

- Batalha, N. M. et al. (2011): [Kepler's First Rocky Planet: Characterization of Kepler-10b](https://doi.org/10.1088/0004-637X/729/1/27)
- Fressin, F. et al. (2011): [Kepler-10c, a 2.2 Earth Radius Transiting Planet in a Multiple System](https://doi.org/10.1088/0067-0049/197/1/5)
- [NASA Exoplanet Archive: Kepler-10 system](https://exoplanetarchive.ipac.caltech.edu/)
- [Lightkurve](https://lightkurve.github.io)
