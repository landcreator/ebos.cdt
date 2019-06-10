# BOSCore.CDT (Contract Development Toolkit)

## BOSCore.CDT Version: v3.0.1
## Basic EOSIO.CDT Version: 1.6.1

## Use BOSCore CDT docker image to build smart conrtacts 
**In order to speed up the progress of contracts development, BOSCore supply complied docker image.**  
**By CDT docker image, you can start build you contracts without build BOSCore.CDT firstly. Please check [README](docker/README.md).**

### Guided Installation (Building from Scratch)
```sh
$ git clone --recursive https://github.com/boscore/bos.cdt
$ cd bos.cdt
$ ./build.sh
$ sudo ./install.sh
```

### Installed Tools
---
* eosio-cpp
* eosio-cc
* eosio-ld
* eosio-init
* eosio-abidiff
* eosio-wasm2wast
* eosio-wast2wasm
* eosio-ranlib
* eosio-ar
* eosio-objdump
* eosio-readelf
