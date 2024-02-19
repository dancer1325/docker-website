# Prerequisites
* [Install docker](https://docs.docker.com/get-docker/)

# Docker host network == Container network
* ===
  * NOT network isolation
  * container networking namespace == host’s networking namespace
    * `docker run -it --network=host --name=container busybox` & `ifconfig` gets results -- since it's the host network interface
* → container
  * NO own IP-address
    * `docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container` is empty
  * port mapping does NOT take effect
    * `docker run -d  --network=host -p 8080:8083 nginx:latest` & `netstat -an | grep LISTEN` checking that there is no new port open
* TODO:
 