# Understanding COSMO

## operational settings

- itype_turb = 3

- itype_vdif = -1

- itype_canopy=2

## all physics settings in sfc_terra.f90

- itype_canopy = 2 (type of canopy parameterisation with respect to the surface energy balance)
- itype_eisa = 2 (type of evaporation from impervious surfaces)
- itype_evsl = 4 (type of parametrization of bare soil evaporation)
- itype_heatcond = 3 (type of soil heat conductivity)
- itype_hydbound = (type of soil water transport and ground water runoff)
- itype_hydmod
- itype_interception = 1 ( type of plant interception)
- itype_mire = 0 ( Approach from Alla Yurova et al. 2014 - activate mire param. )
- itype_root = 2 ( type of root distrubution )
- itype_snow = we know about this
- itype_trvg = 2 (type of vegetation transpiration parameterization)
- itype_interception = 1 (type of plant interception)

## some important subroutines

- turbdiff

- turbtran
