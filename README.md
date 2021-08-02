# tawhiri-container

_tawhiri-container_ is a containised version of [_tawhiri_](https://github.com/cuspaceflight/tawhiri).

## Notes

- The latest version of this container is available from `ghcr.io/projecthorus/tawhiri-container:latest`.
- Downloaded data sets must be placed in `/srv/tawhiri-datasets`. These can be downloaded using [_tawhiri-downloader-container_](https://github.com/projecthorus/tawhiri-downloader-container), and it is recommend that `/srv` is shared between these containers using a method such as a bind mount or volume.
- A timezone must be passed to the container, either by setting the `TZ` environment variable, or by bind mounting `/etc/localtime` in to the container from the host system.
- Elevation data is stored to a file named `/srv/ruaumoko-dataset`, and must be populated using `ruaumoko-download` (including in the container).

## Examples

To download the elevation data:

```sh
docker run --rm -i -t -e TZ=UTC -v /opt/tawhiri:/srv ghcr.io/projecthorus/tawhiri-container:latest ruaumoko-download -v
```

To run the API server with the default configuration of 12 threads, listening on port `8000`:

```sh
docker run --rm -i -t -e TZ=UTC -v /opt/tawhiri:/srv -p 8000:8000 ghcr.io/projecthorus/tawhiri-container:latest
```

To run the API server, with a custom number of threads, listening on a different port:

```sh
docker run --rm -i -t -e TZ=UTC -v /opt/tawhiri:/srv -p 8005:8005 ghcr.io/projecthorus/tawhiri-container:latest gunicorn -b 0.0.0.0:8005 -w 6 tawhiri.api:app
```
