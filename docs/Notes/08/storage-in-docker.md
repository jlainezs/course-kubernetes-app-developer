# Storage in Docker

Storage is implemented as an additional layer on the running container image. The container image layers are **read only** and its content can't be modified, while this additional layer has read and write enabled and it is used to store files produced by the containerized application or modified files on any image layer. This layer is destroyed when the container is destroyed, so all changes are lost.

## Persistent volumes

How to keep file changes? Docker uses volumes to keep data on different instances.

### Volume mounting

We first create the volume (`docker volume create data_volume`), which is created in the Docker volumes folder (/var/lib/docker/volumes) and it is managed by Docker.

Then we **mount** it when creating the container (`docker run -v data_volume:/var/lib/mysql mysql`).

We don't need to create the volume before using it. Docker allows us to mount unexistent volumes when creating containers. It will create the missing volumes without the need to define them previously.

This is **volume mounting**.

### Bind mount

To mount/create a volume anywhere in the Docker host, we mount a volume by defining a path for the volume name: `docker run -v /my_volumes/mysql mysql`




