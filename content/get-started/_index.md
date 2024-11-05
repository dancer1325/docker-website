---
title: Overview of the get started guide
keywords: docker basics, how to start a docker container, container settings, setup
  docker, how to setup docker, setting up docker, docker container guide, how to get
  started with docker
description: Get started with the Docker basics in this comprehensive overview, You'll
  learn about containers, images, and how to containerize your first application.
aliases:
- /engine/getstarted-voting-app/
- /engine/getstarted-voting-app/cleanup/
- /engine/getstarted-voting-app/create-swarm/
- /engine/getstarted-voting-app/customize-app/
- /engine/getstarted-voting-app/deploy-app/
- /engine/getstarted-voting-app/node-setup/
- /engine/getstarted-voting-app/test-drive/
- /engine/getstarted/
- /engine/getstarted/last_page/
- /engine/getstarted/step_five/
- /engine/getstarted/step_four/
- /engine/getstarted/step_one/
- /engine/getstarted/step_six/
- /engine/getstarted/step_three/
- /engine/getstarted/step_two/
- /engine/quickstart/
- /engine/tutorials/
- /engine/tutorials/dockerimages/
- /engine/tutorials/dockerizing/
- /engine/tutorials/usingdocker/
- /engine/userguide/containers/dockerimages/
- /engine/userguide/dockerimages/
- /engine/userguide/intro/
- /get-started/part1/
- /get-started/part5/
- /get-started/part6/
- /getstarted/
- /getting-started/
- /learn/
- /linux/last_page/
- /linux/started/
- /linux/step_four/
- /linux/step_one/
- /linux/step_six/
- /linux/step_three/
- /linux/step_two/
- /mac/last_page/
- /mac/started/
- /mac/step_four/
- /mac/step_one/
- /mac/step_six/
- /mac/step_three/
- /mac/step_two/
- /userguide/dockerimages/
- /userguide/dockerrepos/
- /windows/last_page/
- /windows/started/
- /windows/step_four/
- /windows/step_one/
- /windows/step_six/
- /windows/step_three/
- /windows/step_two/
---

* goal
  - Build and run an image -- as a -- container.
  - Share images -- via -- Docker Hub.
  - Deploy Docker applications / multiple containers -- with a -- DDBB
  - Run applications -- via -- Docker Compose

## What is a container?

* := sandboxed process / 
  * running | host machine
    * can run |
      * local machines
      * VM
      * deployed | cloud
      * ANY OS
  * -- isolated from -- ALL other processes / running | that host machine
    * -> 👀 leverages [kernel namespaces & cgroups](https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504) 👀
* == runnable instance of an image
  * == -- defined by -- image + configuration
  * run its own
    * software
    * binaries
    * configurations
* actions / 
  * can be made | container
    * create,
    * start,
    * stop,
      * -> any changes to its state / NOT stored | persistent storage -> disappear
      * 👀!= delete 👀
    * move,
    * delete
    * -- connect to -- >=1 networks
    * attach storage | it
  * -- via --
    * Docker API
    * Docker CLI
* vs `chroot`
  * 👀container == extended version of `chroot`👀
    * filesystem -- comes from the -- image
    * container adds additional isolation / NOT available using `chroot`

## What is an image?

* running container -- uses an -- isolated filesystem / -- provided by an -- image
* image
  * contains
    * ALL needed -- to run an -- application
      * dependencies,
      * configurations,
      * scripts,
      * binaries,
      * etc. 
    * ALSO, OTHER configurations for the container
      * _Example:_ environment variables, default command to run, and other metadata.
