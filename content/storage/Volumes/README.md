# Prerequisites
* [Install docker](https://docs.docker.com/get-docker/)


# Create and manage volumes
* TODO:

# Start a container with a volume
* if the volume defined for the container does NOT exist -> Docker create the volumes for you
* Via
  * `-v` / `--v`
    * `docker run -d --name devtest -v myvol2:/app nginx:latest`
      * volume mounted 1. name 'myvol2' 2. container's path '/app'
        * `docker inspect devtest | jq '.[0].Mounts'` to check the volume mounted
        * `docker exec -it 152adfeb8ca7 sh` & `ls` checking that '/app' directory exist
      * check 'Notes' section
  * `--mount`
    * `docker run -d --name devtest --mount source=myvol2,target=/app nginx:latest`
      * same as previous one

# TODO:

# Notes
* After each command execution -> you should
  * `docker kill containerId` & `docker rm containerId` -- kill the container 
  * `docker volume rm volumeName` -- remove the volume 
