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

## to add a new field

- I chose rain_gsp as my template variable. rain_gsp variable is a 2d diagnostic variable. I am copying exactly the same implementation but for 4 new variables
- variables scratch_a, scratch_b, scratch_c, scratch_d
- rain_gsp is used in ( i found it using `grep -i -r 'rain_gsp' . | cut -d':' -f1 | sort | uniq`) :
- [ ] ./acc_global_data.f90
- [ ] ./data_fields.f90
- [ ] ./dfi_initialization.f90 ( not needed for us)
- [ ] ./near_surface.f90 ( not needed for us )
- [ ] ./organize_data.f90 ( only for predefined i/o - not needed for us)
- [ ] ./src_allocation.f90
- [ ] ./src_gridpoints.f90 (not for us)
- [ ] ./src_meanvalues.f90 (not needed for us)
- [ ] ./src_output.f90 (surprisingly, no snowpolino variables )
- [ ] ./src_setup_vartab.f90
- [ ] ./src_sing_local.f90 ( not needed for us)

- On second though - I changed my mind and decided to follow sfhl_s as my template variable - why ? because rain_gsp does not exist as blocks and at the interface level (sfc_interface.f90), everything is like blocks.
- [ ] ./acc_global_data.f90
- [ ] ./data_block_fields.f90
- [ ] ./data_fields.f90
- [ ] ./organize_data.f90 ( not needed only for initial or restart etc )
- [ ] ./sfc_interface.f90
- [ ] ./sfc_tile_approach.f90
- [ ] ./src_allocation.f90
- [ ] ./src_block_fields_org.f90
- [ ] ./src_input.f90
- [ ] ./src_setup_vartab.f90 ( not sure if I have to do this)
- [ ] ./src_slow_tendencies_rk.f90 ( not needed )
- [ ] ./turb_diffusion.f90 ( not needed )
- [ ] ./turb_interface.f90 ( not needed )
- [ ] ./turb_transfer.f90 ( not needed )
- [ ] ./turb_vertdiff.f90( not needed )

## COMPILING COSMO

```
module load python/3.7.4
. /project/g110/spack/user/tsa/spack/share/spack/setup-env.sh
```

```
COSMO_SPEC="cosmo@dev-build%gcc real_type=double cosmo_target=cpu ~cppdycore"
spack devbuildcosmo $COSMO_SPEC
```

```
# get and source env
spack build-env --dump cosmo.env $COSMO_SPEC -- #!one space after --
```

In the sbatch script

```
source cosmo.env
export REAL_TYPE=DOUBLE #FLOAT for float
```

## RUNNING DDT FOR COSMO

1. get an allocation

```
salloc --ntasks=96 --partition=postproc --time=00:30:00
```

2. do things similar to sbatch script

```
 ulimit -s unlimited
 ulimit -c unlimited
 source ./cosmo_cpu_sp.env
 export REAL_TYPE=FLOAT
rm YU* M*

```

3. load and launch ddt

```
module load ddt
ddt
```

4. Within DDT, make sure to

- select MPI , and select SLURM generic

That is it !

## Tips on DDT

- to step through the code, use Step over, step within and step out.
- basically, let the code run a bit, then step out ... keep stepping out untill you reach a reasonable level in the code heirarcy. At that point, keep stepping over.
