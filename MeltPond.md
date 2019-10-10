# SIS2 melt pond scheme

## General

This melt pond scheme adds a single layer melt pond to each ice thickness
category.  The layer does not have heat capacity.  It is assumed to be at
freshwater freezing temperature and well mixed.  All pond surface fluxes
are communicated directly to its bottom where surface energy balance is
calculated.  The pond layer is advected and redistributed between ice
thickness categories similarly to the snow and ice layers.  As is the case
with snow, pond cannot exist without an ice layer below.  Basic descriptions
of pond sources, sinks, and pond fraction for radiation treatments follow:

## Pond source water

Rain on ice and surface melting of snow and ice are the sources of pond water.
The runoff scheme keys on the total ice covered area.  This is the only
interaction between category variables in the scheme.  The scheme uses r, the
fraction of melt (r)etained given by the Icepack (2019) CESM scheme:
r = r_min + (r_max-r_min)*ice_area.  The controlling namelist
parameters for r_min and r_max are pond_r_min and pond_r_max.

## Freezing sink of pond water

The surface energy budget continues to be performed at the
top of the snow or ice when pond is present but the interface is fixed at
freezing temperature and the residual between the fluxes on either side of the
interface is made up with melt or freezing.  Freezing in this calculation is a
sink of pond water.  The algorithm handles total pond freeze-up within a time
step, freeing the surface temperature to vary after the pond is completely
frozen.

## Porous-ice sink of pond water

The scheme uses the Icepack mushy layer thermodynamics to treat pond drainage
through the ice (Icepack 2019).  ````do_mushy = True```` namelist is required.

## Freeboard sink of pond water

if the pond and snow are sufficiently massive to push the top of the sea ice 
below sea level, pond is dumped into the ocean until either the icetop is brought 
back to sea level or the pond is completely depleted.

## Ridging sink of pond water

All pond is removed from ridged ice.

## Pond fraction and depth for radiation

The pond depth and pond fraction are considered to be proportional as was found 
at the SHEBA site and incorporated into the Icepack (2019) CESM melt pond 
parameterization.  This means that the pond fraction is proportional 
to the square root of the pond volume but with a limit of 100% cover.  The
treatment of overlapping pond and snow is taken from the level-ice pond
scheme (Hunke et al 2013).

## References

Hunke,E.C., D.A. Hebert, and O. Lecomte, 2013:  Level-ice melt ponds in the Los
Alamos sea ice model, CICE, Ocean Modelling 71, 26-42.

[Icepack documentation](https://buildmedia.readthedocs.org/media/pdf/apcraig-icepack/latest/apcraig-icepack.pdf), 2019.
