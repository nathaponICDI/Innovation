# S-Curve Fitting for Separated and Early-Phase Technology Data

This folder contains a notebook lesson for fitting logistic S-curves to separated mock data:

- [`scurve.ipynb`](./scurve.ipynb)

The notebook shows two related workflows:

1. Fit one S-curve per segment when the data is split into separated observation windows.
2. Diagnose whether very early new-technology data is consistent with the early phase of an S-curve.

## Why This Matters

S-curves are commonly used to describe technology adoption, new-product diffusion, cumulative market penetration, and capacity growth. Early adoption is usually slow, growth accelerates after adoption spreads, and the curve eventually approaches a saturation level.

For mature data, it may be possible to estimate the full curve directly. For very early technology data, this is usually not reliable because the data may not yet contain the fast-growth middle or the final plateau.

Because of that, the notebook treats early technology fitting as a diagnostic question:

> Is the observed data consistent with being in the early phase of a plausible S-curve?

It does not claim that early data can prove the final market size or exact future adoption path.

## Method Summary

The notebook uses a logistic S-curve:

```text
y = baseline + amplitude / (1 + exp(-growth_rate * (x - midpoint)))
```

The main parameters are:

- `baseline`: starting level before growth
- `amplitude`: distance from the baseline to the final level
- `midpoint`: point where growth is fastest
- `growth_rate`: steepness of the curve

For separated mock data, the notebook fits a separate curve for each segment using nonlinear least squares.

For very early technology data, the notebook uses a scenario-based approach:

1. Choose an assumed final adoption level, such as a market-size estimate or total addressable market scenario.
2. Fit the early observations while holding that final level fixed.
3. Check whether the fitted midpoint is still after the latest observation.
4. Check whether current adoption is still a small share of the assumed final level.
5. Compare several final-level scenarios to see how sensitive the conclusion is.

This is useful when the data is too early to estimate the final plateau directly.

## Important Limitation

Early S-curve data can look similar to exponential growth, linear growth, or other sigmoid curves over a short window. If the data does not include the middle and upper parts of the S-curve, the final saturation level is weakly identified.

Use the early-phase section as evidence and scenario analysis, not as a definitive forecast.

## How to Run

Install the required packages in your Jupyter environment:

```python
%pip install numpy pandas matplotlib scipy
```

Then open and run:

```text
S-curve/scurve.ipynb
```

## References

These are the main references behind the notebook's approach.

1. Rogers, E. M. (2003). *Diffusion of Innovations* (5th ed.). Free Press.  
   https://www.simonandschuster.com/books/Diffusion-of-Innovations-5th-Edition/Everett-M-Rogers/9780743222099

2. Bass, F. M. (1969). A New Product Growth for Model Consumer Durables. *Management Science*, 15(5), 215-227.  
   https://doi.org/10.1287/mnsc.15.5.215

3. Mahajan, V., Muller, E., & Bass, F. M. (1990). New Product Diffusion Models in Marketing: A Review and Directions for Research. *Journal of Marketing*, 54(1), 1-26.  
   https://doi.org/10.1177/002224299005400101

4. Geroski, P. A. (2000). Models of Technology Diffusion. *Research Policy*, 29(4-5), 603-625.  
   https://doi.org/10.1016/S0048-7333(99)00092-X

5. Meade, N., & Islam, T. (2006). Modelling and Forecasting the Diffusion of Innovation: A 25-Year Review. *International Journal of Forecasting*, 22(3), 519-545.  
   https://doi.org/10.1016/j.ijforecast.2006.01.005

6. Verhulst, P. F. (1838). Notice sur la loi que la population poursuit dans son accroissement. *Correspondance Mathematique et Physique*, 10, 113-121.  
   Background on the logistic growth model.

## Recommended Use

When using this notebook with real data, report results as scenarios:

- conservative final adoption level
- base-case final adoption level
- optimistic final adoption level

If the early-phase conclusion changes sharply across scenarios, the data is probably too early for a strong claim.
