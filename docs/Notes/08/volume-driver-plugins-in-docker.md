# Volume driver plugins in docker

Volume drivers allows Docker to use different storage systems.

By default Docker uses `local` driver which allows to create and use volumes in the Docker host.

If we need to use external resources (Azure, Digital Ocean, Google Cloud Persistent Disk, ...) we need to use a specific volume driver.
