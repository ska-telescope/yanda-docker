# INGEST

This docker build is in response to [YAN-46](https://jira.skatelescope.org/browse/YAN-46).

```
docker build . --build-arg COMPILE_JOBS=4
``` 

| Arg | Value |
|---|---|
| COMPILE_JOBS | The number of parallel compile specified via make -j  |
