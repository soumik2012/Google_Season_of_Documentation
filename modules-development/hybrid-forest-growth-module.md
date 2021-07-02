# Hybrid Forest Growth Module

The Hybrid Forest Growth Module calculates the biomass above and below the ground. These modules use physiological responses to environmental conditions to derive relationships that can be used to predict tree growth. These modules contain equations or series of equations to calculate biomass.

Like in `forestgrowthmodule.cpp`, the equation used is:

```
    double ForestGrowthModule::calculate_above_ground_biomass(double age, double max, double k, double m) {
       return max * pow(1 - exp(-k * age), 1 / (1 - m));
    }
```

The Hybrid Forest Growth module represents a 3-PG model (Physiological Principles in Predicting Growth). 3-PG models require finer detailed data than is typically represented in empirical growth models. 3-PG models are better than the empirical models as they can be used with any landscape as long as its variables are present. The GCBM modules contain one example of an empirical growth module.