# Rendering tiles

# TL;DR

After troubleshooting:

```
sudo docker run --shm-size 14g -p 80:80 -it -v openstreetmap-data:/var/lib/postgresql/10/main -v /home/ubuntu/osm:/etc/postgresql/10/main/conf.d a-hahn/openstreetmap-tile-server:latest run
```

# Troubleshooting

The renderer caused problems that were not immediately obvious. Tiles returned '404' responses and it seemed that the rendering process came to halt.
For troubleshooting i added a 'debug'-mode to run.sh.

## Renderer running 'out-of-memory'

*ERROR:  could not resize shared memory segment "/PostgreSQL.2088012784" to 50438144 bytes: No space left on device*

This problem was caused by a [postgres running out of Shared memory](https://github.com/docker-library/postgres/issues/416)
and increasing the containers shared memory size.
**Note** that the 14g may be a bit oversized.

## Tiles not rendered; Returning '404'

See the modifications in *apache.conf*

# Other useful links

* The original author Alexander Overvoorde 'dockerized' the recipe from
[Switch2Osm](https://switch2osm.org/manually-building-a-tile-server-18-04-lts/)

* OpenStreetMap on [Github](https://github.com/openstreetmap)

* Mod_tile [Parameters](https://github.com/openstreetmap/mod_tile/blob/master/mod_tile.conf)

* Mod_tile 'Non-osm' [information](https://github.com/SomeoneElseOSM/mod_tile)

# Docker rendering command

## Debug (modified path for europe)

Using a volume for the rendered tiles

```
sudo docker run --shm-size 14g -e THREADS=24 -p 80:80 -it -v osm-europe-tiles:/var/lib/mod_tile -v osm-europe-data:/var/lib/postgresql/10/main -v /home/ubuntu/osm:/etc/postgresql/10/main/conf.d a-hahn/openstreetmap-tile-server:latest debug
```
Enter a shell with *sudo docker exec -it <container_id> /bin/bash* or *sudo docker attach <container_id>* to exceute 

## Run mode

(Note: same es 'debug' except for the ver last 'run' at line end)
```
sudo docker run --shm-size 14g -p 80:80 -e THREADS=24 -it -v osm-europe-tiles:/var/lib/mod_tile -v osm-europe-data:/var/lib/postgresql/10/main -v /home/ubuntu/osm:/etc/postgresql/10/main/conf.d a-hahn/openstreetmap-tile-server:latest run
```
