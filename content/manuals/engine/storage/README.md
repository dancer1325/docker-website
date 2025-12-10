* files / created inside a container → 👁️stored on writable container layer 👁️
  * ==
    * once the container does NOT exist → data NOT persisted
      * → difficult to use out of the container
    * container’s writable layer — tightly coupled to — host machine where container is running
    * if you want to write into container’s writable layer → you need a Storage Drivers
      * performance using Volumes  > performance using Storage Drivers

# Ways for containers to store files / although container stops → files are persisted
* on the host machine
  * Volumes
  * Bind mounts
* on the host’s system memory
  * (Docker on Linux) tmpfs mounts
  * (Docker on Windows) named pipes

# Volumes vs Bind mounts vs tmpfs mounts
* independently the type
  * data looks the same from within the container
    * directory OR
    * file
* based on where the data lives -- Check 'images/types-of-mounts'
  * Volumes
    * 👁️concrete area in the host filesystem, managed by Docker 👁️
      * For Linux -- '/var/lib/docker/volumes/'
      * For Mac -- TODO: --
    * non-Docker processes should NOT modify it
  * Bind mounts
    * anywhere on the host syste
    * non-Docker processes can modify it
  * `tmpfs` mounts
    * ONLY in host system’s memory
    * NEVER written to host system’s filesystem
* How to mount?
  * `-v` / `--volume`
    * volumes mounted in containers
    * bind mounts mounted in containers
  * `--tmpfs`
    * `tmpfs` mounts mounted in containers
  * `--mount`
    * mounting in containers and services
      * volumes
      * bind mounts
      * `tmpfs` mounts

TODO: Rest of section & prove all
