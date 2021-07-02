# Empirical Forest Growth Module

The empirical Forest growth module includes all the operations required to simulate the biomass accumulation. Specifically, it simulates the growth in carbon stock within a forest and its components through time. It is a standard module used in FLINT and is used within Chapman Richards Module. **Biomass** refers to the mass of living organisms, including plants, animals and microorganisms. **Biomass accumulation** is the net change in standing biomass from one time to another (g/m^2).

It runs at the **Monthly Step Interval**. It is based on the yield tables that present the **monthly growth increments** for a forest type. This data comes from research and experimentation and is called **empirical data**, thus giving the name of this module as empirical forest growth module. Yield Tables only include the **aboveground biomass**.

## INPUTS :

-   Time Step (Monthly)
-   Species
-   Age
-   Region
-   Forest type
-   Pool 1: Stemwood
-   Pool 2: Bark
-   Pool 3: Leaves
-   Pool 4: Coarse Root
-   Pool 5: Fine Roots
-   Rainfall in that region
-   Minimum and Maximum Temperature
-   [Vapour Pressure Deficit](https://en.wikipedia.org/wiki/Vapour-pressure_deficit)
-   Soil Water Holding Capacity

## OUTPUTS:

-   Calculated aboveground and belowground Biomass.
