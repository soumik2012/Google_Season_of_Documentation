# Modules Development

Modules are all written to work with the `develop` branch of the **FLINT** and any actively maintained branches will work with the most recent version (and are broken with old enough builds of the FLINT). The FLINT Application Programming Interface (API) has evolved a bit over time, so some modules might have fallen out of date. None of these modules requires a custom fork of FLINT.

## Background

Some prerequisites for the modules to be covered later are: pools, operations, sparse matrix, state variables, [tiers of modules](https://www.reddcompass.org/mgd-content-v1/dita-webhelp/en/Box1.html) and more. We will discuss the prerequisites for each specific module like GCBM modules and other modules.

What might be helpful though is to think about what a module manifest might look like. **Manifests** are meta-data used by the package managers to describe the compatibility (FLINT versions that are used), dependency (necessary inputs) and the configuration requirements (whether the module uses daily, monthly or annual time steps, for example).

This project describes the core contribution of each module.

## Index

1.  [Empirical forest growth module](empirical-forest-growth-module.md)
2.  [Hybrid forest growth module](hybrid-forest-growth-module.md)
3.  [WOFOST crop growth module](wofost-crop-growth-module.md)
4.  [Turnover module](turnover-module.md)
5.  [Decomposition module](decomposition-module.md)
6.  [RothC soil carbon module](rothc-soil-carbon-module.md)
7.  [Chapman Richards module](chapman-richards-module.md)
8.  [GCBM module](gcbm-module.md)
