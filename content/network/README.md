# Prerequisites
* [Install docker]()
* `docker network create -d bridge my-net`
  * Create a network
* `docker run -itd --name=container busybox`
  * Run a container with the default (bridge) network 

# Overview
* := ability for containers to connect & communicate with
  * other containers
    * Run a containers in the same network
      * `docker run --network=my-net -itd --name=container1 busybox` & `docker run --network=my-net -itd --name=container2 busybox`
    * Via containerIP
      * Get the container1's ip
        * `docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container1` 
      * Connect into container2 and reach container1
        * `docker exec -it container2 sh` & `ping 172.21.0.2`
    * Via containerName
      * Connect into container2 and reach container1
        * `docker exec -it container2 sh` & `ping container2`
  * non-Docker workloads
    * TODO:

# Containers
* By default
  * networking enabled with
    * IP address
      * `docker inspect -f '{{ .NetworkSettings.IPAddress }}' container` 
    * Gateway
      * `docker inspect -f '{{ .NetworkSettings.Networks.bridge.Gateway }}' container`
        * 'bridge' is the default network
    * Routing table
      * `docker exec -it container route`
    * DNS services
      * `docker info | grep -i dns` to check the DNS services configured
        * if the output is empty -> it uses the host's one
      * `docker exec -it container ping google.com` to check that you can reach internet resources
  * You can disable with `none` -- TODO: --
* can make outgoing connections
  * `docker exec -it container ping google.com`
* NO information about
  * kind of network attach to
    * Problems:
      * Problem1: I am getting results `docker exec -it container sh` & `ifconfig`
        * Solution: TODO:
  * if their peers are Docker workloads
    * `docker network create -d bridge my-net` & `docker run --network=my-net -itd --name=container1 busybox` & `docker run --network=my-net -itd --name=container2 busybox`
    * `docker exec -it container1 sh` & `ifconfig` without getting information about the container2's network interface


