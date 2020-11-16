
<!-- README.md is generated from README.Rmd. Please edit that file -->

# bigrquerystorage

<!-- badges: start -->

<!-- badges: end -->

The goal of bigrquerystorage is to provide access to the BigQuery
Storage API from R.

The main motivation is to replace `bigrquery::bq_table_download` in some
workload.

Currently it supports v1 version with very basic table mass download
capacity.

## Benefits

BigQuery Storage API is not rate limited nor has per project quota. No
need to manage `bigrquery::bq_table_download` page size anymore.

BigQuery Storage API is based on gRPC. This particular implementation
use a C++ generated client with the excellent `arrow` R package. It
makes it 2 to 4 times faster than `bigrquery::bq_table_download` method.

More insidious; no more truncated results when
`bigrquery::bq_table_download` page size produce `json` files greater
than 10MiB.

## Installation

System requirements:  
\- C++11  
\- [gRPC C++](https://github.com/grpc/grpc/blob/master/BUILDING.md) I
used [this
procedure](https://github.com/grpc/grpc/blob/master/test/distrib/cpp/run_distrib_test_cmake_module_install_pkgconfig.sh)
after cloning the repo.  
\- [protoc
C++](https://github.com/protocolbuffers/protobuf/tree/master/src)

You can install the development version of bigrquerystorage from
[GitHub](https://github.com/meztez/bigrquerystorage) with:

``` r
# install.packages("devtools")
devtools::install_github("meztez/bigrquerystorage")
```

## Example

This is a basic example which shows you how to solve a common problem.
BigQuery Storage API requires a billing project as there is no free tier
to the service.

``` r
## Auth is done automagically using Application Default Credentials
library(bigrquerystorage)
## basic example code
bqs_table_download("bigquery-public-data.usa_names.usa_1910_current", "labo-brunotremblay-253317")
```

## Performance

### Compared to Python Client for BigQuery Storage API

Currently about 25% Slower.  
i.e. Python Client takes 60 seconds, this would take 75 seconds.

### Compared to `bigrquery::bq_table_download`

When `bigrquery::bq_table_download` does not hit a quota or a rate
limit, 2 to 4 times faster. The bigger the table, the faster this will
be compared to the standard API.

## Stability

This is an early release.  
More feature will be added over time.  
Windows support has not been tested yet.  
It crashes when it runs out of memory.
