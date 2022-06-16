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

  - This file is based on ~/movero/config/versions/fieldextra_kenda-2-hc
  - the diff is only to add snow

```bash
# Observation types
# MOVERO_DATABASE=dwh:    'synop', 'surface', 'snow', 'pollen', or 'tnx'
#              file:   path including name of observational files,
#                      may contain placeholders <yyyy>, <yy>,<mm>, <dd>
export MOVERO_OBS_TYPE=snow

```

- through daniel, created a new dwh_retreive extraction template to include snow variables (surface_snowverif)

####
