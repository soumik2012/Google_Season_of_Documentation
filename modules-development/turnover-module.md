# Turnover Module

Turnover is the movement from a living carbon pool to a dead carbon pool. This is due to the natural senescence of plant components . For example: A deciduous forest will have near 100 per cent turnover of leaves across a year – or an annual turnover rate of 0.99. The turnover rates will vary with vegetation types and the plant components. For example: A tree stem may have zero (0) annual turnover, while the leaves have one (1).

To determine the volume of turnover through time, a calculation similar to the one below is carried out:

```
    Turnover= 1 - (1 - turnover_rate) ^ time
```

Within the FLINT, the maximum decay rate is set at 0.99 999 999 998, and the minimum decay rate is 0.000001. These restrictions are in place to ensure there are no invalid calculations (Example: dividing by zero) occurs in the system.