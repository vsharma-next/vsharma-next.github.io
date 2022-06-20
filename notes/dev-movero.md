# Developing Movero (MCH)

## Definitions:

- Movero : MCH's verification software (https://github.com/MeteoSwiss-APN/movero)
- DWH Service: the big database of all observational data at MCH

## What ?:

- The current functionality of movero needs to be extended to include snow variables: Snow temperature and Snow height

## Why ?:

- Currently movero does NOT include snow variables and me and sascha have essentially 'rolled your own' solutions - not a very durable approach.
- This development will bring a systematic approach to evaluating snowpolino.
- This work is happening now because DWH service has uploaded imis stations to it's database and therefore, it becomes possible to use movero's extensive framework for this purpose.

## LOG :

#### 2022-06-16

- setting up this dev doc
- created a new exp file in ~/movero/config/versions/snow_test.exp.

  - This file is based on ~/movero/config/versions/kenda-2-hc_ch.exp
  - the diff is only to add snow

```bash
# Observation types
# MOVERO_DATABASE=dwh:    'synop', 'surface', 'snow', 'pollen', or 'tnx'
#              file:   path including name of observational files,
#                      may contain placeholders <yyyy>, <yy>,<mm>, <dd>
export MOVERO_OBS_TYPE=snow

```

- through daniel, created a new dwh_retreive extraction template to include snow variables (surface_snowverif)

```bash
dwh_retrieve -s surface_snowverif -t 202112150000-202112160000 --fmt ascii
```

#### 2022-06-20

- just verified that there is data in /store/s83/vsharma/EXP_TST/3xx - there are three folders 301,302, 304
- the command to run movero is

```bash
batchPP -p pp-serial -t 120 "/users/vsharma/movero/bin/movero-verify --force=get_mod --show-log --base-time=2020092512,2020092612 --base-hour=12 --category=Test-suites --directory=seasons --config=kenda-2-hc_ch.exp 302"
```

- modified ./scripts/movero_get_obs to add snow as an analysis type

```ruby
# THIS IS THE SAME AS THE ORIGINAL
                pollen)
                    # DWH pollen data
                    # 0-0 UTC mean ALNU24, AMBR24, BETU24, POAC24 = ?,4861,4839,4870
                    # 6-6 UTC mean ALNU24, AMBR24, BETU24, POAC24 = 1315,1424,1323,1469
                    # 2 h mean     ALNU2,  AMBR2,  BETU2,  POAC2  = ?,4490,4494,4520
                    # hourly mean  ALNU,   AMBR,   BETU,   POAC   = ?,?,?,?
                    cat > $obs_retrieval.tmp <<EOF
$dwh_retrieve_cmd $dwh_env_opt -t ${day}0000-${day}2359 -s surface_stations \
              -p 1323,1424,1469,1315 \
              -i nat_abbr,PBS,PBE,PBU,PDS,PGE,PCF,PLS,PLO,PLU,PLZ,PMU,PNE,PVI,PZH \
              -f $format -o $obs_dir/$obs_file_day
EOF
                    ;;
# ADDED BY ME
                snow)

                    # Parameters for CH surface data
                    param_list=162,3567
                    #         T_SNOW , H_SNOW

                    # Write retrieval command
                    # Getting snow data (newly added SLF stations for example) from DWH services
                    # Using a newly setup dwh_retrieve type called surface_snowverif
                    cat > $obs_retrieval.tmp <<EOF
$dwh_retrieve_cmd $dwh_env_opt -t ${day}0000-${day}2359 -s surface_snowverif \
              -f $format -o $obs_dir/$obs_file_day
EOF

                    ;;

```

**This makes it possible to get OBSERVATION data for movero to work with. Next step: make sure model data is also attracted **

- To get model data into movero with snow:

  - created folder /users/vsharma/movero/config/fieldextra_snow_test
  - essentially copied the template of the folder fieldextra_kenda-2-hc
    - created the 3 HEADER files (fieldextra_HEADER_XXX)
    - created 2 files fieldextra-T_SNOW & fieldextra-H_SNOW

- **Important: RE-INSTALL movero ! **
