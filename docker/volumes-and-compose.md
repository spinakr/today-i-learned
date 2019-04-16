# Volumes and docker-compose

Applications running in continers, will be recreated from time to time, when deploying new version etc. By default containers storing data to disk, will mount new "volumes" whenevere recreated. To circumvent this we can explisitly define the volume, which then will continue to live across recreation of containers.

The standard postgres image creates a volume by default mapped to `/var/lib/postgresql/data`. To make this persistent we can create volume manually before running the container with `docker volume create postgres-data`. When starting the container we then configure it to use the volume: `docker run -d -v postgres-data://var/lib/postgresql/data postgres`

### Using docker-compose
Volumes can also be create and configured in docker-compose-files, as following:

```yml
version: '3'
services:
mydb:
image: postgres
volumes:
- postgres-data:/var/lib/postgresql/data
 
volumes:
    postgres-data:
```