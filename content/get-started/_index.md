---
title: Get started
keywords: Docker, get started
description: Get started with Docker
layout: wide
params:
  icon: download
  notoc: true
  get-started:
  - title: Get Docker
    description: Choose the best installation path for your setup.
    link: /get-started/get-docker/
    icon: download
  - title: What is Docker?
    description: Learn about the Docker platform.
    link: /get-started/docker-overview/
    icon: summarize
  get-started2:
  - title: Introduction
    description: Get started with the basics and the benefits of containerizing your applications.
    link: /get-started/introduction/
    icon: rocket
  - title: Docker concepts
    description: Gain a better understanding of foundational Docker concepts.
    link: /get-started/docker-concepts/the-basics/what-is-a-container/
    icon: foundation
  - title: Docker workshop
    description: Get guided through a 45-minute workshop to learn about Docker.
    link: /get-started/workshop/
    icon: desk
aliases:
  - /engine/get-started/
  - /engine/tutorials/usingdocker/
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
