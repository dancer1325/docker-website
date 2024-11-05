---
description: Get an in-depth overview of the Docker platform including what it can
  be used for, the architecture it employs, and its underlying technology.
keywords: what is a docker, docker daemon, why use docker, docker architecture, what
  to use docker for, docker client, what is docker for, why docker, uses for docker,
  what is docker container used for, what are docker containers used for
title: Docker overview
aliases:
- /introduction/understanding-docker/
- /engine/userguide/basics/
- /engine/introduction/understanding-docker/
- /engine/understanding-docker/
- /engine/docker-overview/
---

## The Docker platform

* Docker platform
  * allows
    * managing the lifecycle of your containers
      * develop your application + supporting components -- via -- containers
      * deploy your application | your production environment (local data center, cloud provider, or hybrid)

## What can I use Docker for?

### Fast, consistent delivery of your applications

* TODO:

Docker streamlines the development lifecycle by allowing developers to work in
standardized environments using local containers which provide your applications
and services. Containers are great for continuous integration and continuous
delivery (CI/CD) workflows.

Consider the following example scenario:

- Your developers write code locally and share their work with their colleagues
  using Docker containers.
- They use Docker to push their applications into a test environment and run
  automated and manual tests.
- When developers find bugs, they can fix them in the development environment
  and redeploy them to the test environment for testing and validation.
- When testing is complete, getting the fix to the customer is as simple as
  pushing the updated image to the production environment.

### Responsive deployment and scaling

Docker's container-based platform allows for highly portable workloads. Docker
containers can run on a developer's local laptop, on physical or virtual
machines in a data center, on cloud providers, or in a mixture of environments.

Docker's portability and lightweight nature also make it easy to dynamically
manage workloads, scaling up or tearing down applications and services as
business needs dictate, in near real time.

### Running more workloads on the same hardware

Docker is lightweight and fast. It provides a viable, cost-effective alternative
to hypervisor-based virtual machines, so you can use more of your server
capacity to achieve your business goals. Docker is perfect for high density
environments and for small and medium deployments where you need to do more with
fewer resources.

## Docker architecture

* client-server architecture
  * Docker client
    * -- talks, via REST API, to the -- Docker daemon
      * REST API over
        * UNIX sockets or
        * network interface 
    * _Example:_ Docker Compose
  * Docker daemon
    * == server side | this architecture
    * does the heavy lifting, about your Docker containers,
      * building,
      * running,
      * distributing
    * 👀's host vs Docker client's host 👀
      * SAME
      * !=
        * _Example:_ Docker daemon | remote host

    ![Docker Architecture diagram](images/docker-architecture.webp)

### The Docker daemon -- `dockerd` --

* listens for Docker API requests
* manages Docker objects (images, containers, networks, and volumes)
* can communicate with
  * Docker clients
  * 👀other daemons 👀
    * _Example:_ to manage Docker services

### The Docker client -- `docker` -- 

* uses
  * primary way -- to, via Docker API, interact with -- `dockerd`
    * can interact with >= 1 daemon 
* _Example:_ `docker run`

### Docker Desktop

* == application / 
  * easy-to-install
  * available for
    * Mac,
    * Windows
    * Linux
  * allows, about containerized applications, 
    * build
    * share 
  * 👀== (include) `dockerd` + `docker` + Docker Compose + Docker Content Trust + Kubernetes + Credential Helper 👀
* see [Docker Desktop](../desktop/index.md)

### Docker registries

* allows
  * storing Docker images
    * _Example:_ `docker push ...`
  * pulling Docker images
    * _Example:_ `docker pull ...`
* types
  * public
  * private
    * you can run your own
* Docker Hub
  * 👀== public registry 👀
    * public == ANYONE can use
  * uses
    * by default, by Docker, to look for images

### Docker objects

* uses
  * using Docker

#### Images

* := read-only template / 
  * 👀 instructions -- for creating a -- Docker container 👀
  * (normally) -- is based on -- ANOTHER image
    * \+ additional customization
      * _Example:_ image / 
        * -- based on the -- `ubuntu` image
        * \+ installs Apache web server
  * types
    * shared by the community
    * create your own one
      * -- via -- creating a Dockerfile

#### Containers

* container
  * see [index](_index.md#what-is-a-container)
  * == environment /
    * isolated -- from -- other
      * containers &
      * host machines
    * lightweight 
  * uses
    * run applications | it
      * Reason: 🧠contains ALL -- to run the -- application 🧠
    * unit for 
      * distributing your application
      * testing your application
  * allows
    * running simultaneously MANY containers | given host
  * -- managed by -- Docker

##### Example `docker run` command

* 

    ```console 
    $ docker run -i -t ubuntu /bin/bash
    ```

    * let's assume, you are using the default registry configuration
      1. if you do NOT have `ubuntu` image locally -> Docker pulls it -- from your -- configured registry
         1. == run manually `docker pull ubuntu` 
      2. Docker creates a new container
         1. == run manually `docker container create`
      3. Docker -- allocates a -- read-write filesystem | container's final layer
         1. -> running container can create or modify files & directories | its local filesystem
      4. Docker -- creates a -- network interface
         1. -> container -- is connected to the -- default network
         2. == assign a container's IP address
         3. ALTHOUGH, containers -- can connect, via host machine's network connection, to -- external networks 
      5. Docker starts the container & executes `/bin/bash`
      6. container is running interactively 
         1. Reason: 🧠due to the `-i` 🧠
    * if you run `exit` -> terminate the `/bin/bash` command -> container stops

## The underlying technology

* Docker
  * written in [Go programming language](https://golang.org/)
  * -- takes advantage of -- SEVERAL features of the Linux kernel
    * 👀`namespaces` 👀
      * uses
        * provide the isolated workspace -- via -- running the container
          * EACH aspect of a container
            * runs | separated namespace
            * 's access -- is limited to -- that namespace 👀 
      * set of namespaces / container

## Next steps

- [Install Docker](../get-docker.md)
- [Get started with Docker](index.md)
