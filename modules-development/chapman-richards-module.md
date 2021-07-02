# Chapman Richards Module

The Chapman Richards module is a tier-3 module. It is used for the estimation of greenhouse gas emissions and removals due to changes in biomass carbon for forests using Chapman Richards equation. We can find aboveground and belowground biomass using this module.

```
    AGC = * [ 1 -  e ^ { -k *Age } ] ^ { ( 1 / (1-m) }
```

-   **AGC**: Above-ground stand carbon (Tonnes carbon per hectare).
-   **Max**: Asymptote – maximum peak carbon yield (Tonnes carbon per hectare).
-   **k**: Parameter used in modelling tree growth (dimensionless).
-   **Age**: Age of the forest (years).
-   **m**: Parameter used in modelling tree growth (dimensionless).

This module provides us the estimation of Aboveground biomass on the basis of the age of forests. Belowground biomass is calculated as a ratio from aboveground biomass. This equation is also used within the Forest Growth module. There is a separate repository for the Chapman Richards module [here](https://github.com/moja-global/FLINT.Module.Chapman_Richards). It works on a monthly time step.

## INPUT :

-   Time Step (Monthly)
-   Dead Organic Matter
-   Soil organic matter
-   Pool 1 : Non-CO2 gases (The selection of carbon pools or non-CO2 gases for estimation will depend on the significance of the pool and tier selected for each land-use category)
-   Forest Age
-   Forest Type
-   Forest Cover
-   Soil Carbon
-   Spatial Location of Forest

## OUTPUT :

-   Above-ground biomass
-   Below-ground biomass

For Forest Land complete documentation refer to [this](https://www.ipcc-nggip.iges.or.jp/public/2006gl/pdf/4_Volume4/V4_04_Ch4_Forest_Land.pdf).