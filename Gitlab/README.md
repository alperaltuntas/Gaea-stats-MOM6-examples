## Scripts used in gitlab pipeline testing MOM6 commits against the head of MOM6-examples

- These scripts are not for general consumption - they are mostly written in gmake so good luck figuring out how they work! :)
  But seriously they are written for a particular sequence of tests assuming a specific setup and so they really will not be much use for general application.
- These tests are not automatically triggered. Tests need to be manually instigated by pushing branches to gitlab.


## Initial setup

```bash
make -f Gitlab/Makefile.clone clone
```
or
```bash
git submodule init
git submodule update --recursive 
(cd MOM6-examples; git submodule init; git submodule update)
(cd MOM6-examples/src/MOM6; git submodule init; git submodule update)
```

## Clone additional components from internal gitlab server

```bash
make -f Gitlab/Makefile.clone clone_gfdl
```

## Build executable

```bash
make -f Gitlab/Makefile.build build_gnu -s -j
make -f Gitlab/Makefile.build build_intel -s -j
make -f Gitlab/Makefile.build build_pgi -s -j
```

## Run model

```bash
make -f Gitlab/Makefile.run gnu_all -s -j
make -f Gitlab/Makefile.run intel_all -s -j
make -f Gitlab/Makefile.run pgi_all -s -j
```

or 
```bash
make -f Gitlab/Makefile.run gnu_all MEMORY=dynamic_symmetric -s -j
make -f Gitlab/Makefile.run intel_all MEMORY=dynamic_symmetric -s -j
make -f Gitlab/Makefile.run pgi_all MEMORY=dynamic_symmetric -s -j
```

or 
```bash
make -f Gitlab/Makefile.run gnu_static_ocean_only MEMORY=static -s -j
make -f Gitlab/Makefile.run intel_static_ocean_only MEMORY=static -s -j
make -f Gitlab/Makefile.run pgi_static_ocean_only MEMORY=static -s -j
```

## Copy results to regressions/
```bash
make -f Gitlab/Makefile.sync -s
```
