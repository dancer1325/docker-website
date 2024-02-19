# Prerequisites
* [Install docker](https://docs.docker.com/get-docker/)
* Create a network
  * `docker network create defaultone`
* `docker run --network=defaultone -itd --name=container1 busybox`
  * Run a container within the network created
* Container2
  * `docker run -itd --name=container2 busybox`
    * Run a container out of the network
  * `docker exec -it container2 sh` & `ping container1`
    * You can NOT ping to container1 -- from -- container2

# Concepts
* `docker network connect [OPTIONS] networkName containerNameOrId`
  * container — connected to — network
    * `docker network connect defaultone container2` or `docker network connect defaultone container2ID` & 
      * `docker inspect -f '{{ .NetworkSettings.Networks}}' container2` to check that you have added to the container network
    * → — can communicate with — other containers in the network
      * `docker exec -it container2 sh` & `ping container1
  * [OPTIONS]
    * TODO:
# Examples
* Check '../../data/engine-cli'