# Yanda base

This docker build is in response to [YAN-44](https://jira.skatelescope.org/browse/YAN-44)
and [YAN-46](https://jira.skatelescope.org/browse/YAN-46)..

# Normal
```
docker build -t localhost:9999/yanda/yanda-base .
``` 

## CUDA

``` 
docker build -t localhost:9999/yanda/yanda-base-cuda -f Dockerfile-Cuda .
```
