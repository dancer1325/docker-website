# Prerequisites
* [Install docker](https://docs.docker.com/get-docker/)
* Create a network
  * `docker network create default`
    * Problems: "Error response from daemon: operation is not permitted on predefined default network"
      * Solution: choose another name -- `docker network create defaultone`
* `docker run -itd --name=container busybox`
    * Run a container with the default (bridge) network

# 
* Default one
  * `docker network inspect defaultone --format='{{.Driver}}'` returning bridge
* Uses
  * containers are in the same host
    * _Example:_ Running locally
* TODO: