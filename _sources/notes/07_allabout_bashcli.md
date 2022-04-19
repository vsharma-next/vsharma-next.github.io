# useful bash command line snippets

## grep

- to return the word containing a search string (and not the entire line)

```
grep -i -r -o '\w*itype\w*' ./sfc_terra.f90
```

returns all words with itype in it.

- return only unique values

```
grep -i -r -o '\w*itype\w*' ./sfc_terra.f90 | sort | uniq
```

returns only unique values from the match

## sudo apt

- to suppress output and assume yes to all imports

```
sudo apt-get -yqq instal xxxyyyzzz
```
