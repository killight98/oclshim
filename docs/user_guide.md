# How to Build and Run for OpenCL Applications

This document describes how to build `libOpenCL.so` (dynamic shared file) and `cliloader`(oclshim tool), also provide some commands for using them.

## Environment

- Cmake
- C++ Compiler

## Build Command

```sh
mkdir build && cd build
cmake .. -DENABLE_HIGH_RESOLUTION_CLOCK=TRUE
make -j<N>
# cmake --build .
# make install
```
Cmake recommends setting `ENABLE_HIGH_RESOLUTION_CLOCK` TRUE, profiling tool would use the `high_resolution_clock` for collecting timestamps instead of the default `steady_clock`, its default value may lead to collect wrong time information.

## Usage Command

```sh
$ CLI_OpenCLFileName=/path/to/real/libOpenCL.so \
    cliloader -at -ccl -cdt -dv ./<application>

$ CLI_OpenCLFileName=/path/to/real/libOpenCL.so \
    mpirun -n <rank_num> cliloader -at -ccl -cdt -dv ./<application>

# --device-timing [-d]             Report Device Execution Time
# --device-timing-verbose [-dv]    Report More Detailed Device Execution Time
# --chrome-call-logging [-ccl]     Record Host API Calls to a JSON Trace File
# --chrome-device-timeline [-cdt]  Record Per-Queue Device Timeline to a JSON Trace File
# --chrome-kernel-timeline [-ckt]  Record Per-Kernel Device Timeline to a JSON Trace File
# --chrome-device-stages [-cds]    Record Device Timeline Stages to a JSON Trace File
# --api-tracing [-at]              Record Both Device and HOST API Calls to a CSV Trace File
# --host-timing [-h]               Report Host API Execution Time
```

`CLI_OpenCLFileName` setup environment variable to tell the Intercept Layer for OpenCL Applications where to find the real ICD loader.

All args and its meaning could be checked by `cliloader --help`, some common used arg shows above, see the [controls documentation](controls.md) for more detail.

Please put `mpirun -n <rank_num>` in front of `cliloader` if execute multi-rank application.
