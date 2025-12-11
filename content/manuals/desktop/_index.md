---
title: Docker Desktop
weight: 10
description: Explore Docker Desktop, what it has to offer, and its key features. Take the next step by downloading or find additional resources
keywords: how to use docker desktop, what is docker desktop used for, what does docker
  desktop do, using docker desktop
params:
  sidebar:
    group: Products
grid:
- title: Install Docker Desktop
  description: |
    Install Docker Desktop on
    [Mac](/desktop/setup/install/mac-install/),
    [Windows](/desktop/setup/install/windows-install/), or
    [Linux](/desktop/setup/install/linux/).
  icon: download
- title: Learn about Docker Desktop
  description: Navigate Docker Desktop.
  icon: feature_search
  link: /desktop/use-desktop/
- title: Explore its key features
  description: |
    Find information about [Networking](/desktop/features/networking/), [Docker VMM](/desktop/features/vmm/), [WSL](/desktop/features/wsl/), and more.
  icon: category
- title: View the release notes
  description: Find out about new features, improvements, and bug fixes.
  icon: note_add
  link: /desktop/release-notes/
- title: Browse common FAQs
  description: Explore general FAQs or FAQs for specific platforms.
  icon: help
  link: /desktop/troubleshoot-and-support/faqs/general/
- title: Give feedback
  description: Provide feedback on Docker Desktop or Docker Desktop features.
  icon: sms
  link: /desktop/troubleshoot-and-support/feedback/
aliases:
- /desktop/opensource/
- /docker-for-mac/dashboard/
- /docker-for-mac/opensource/
- /docker-for-windows/dashboard/
- /docker-for-windows/opensource/
---

* Docker Desktop 
  * := application /
    * available cross platform (Mac, Linux, or Windows)
    * allows
      * about containerized applications and microservices
        * build,
        * share,
        * run
      * taking care of
        * port mappings,
        * file system concerns,
        * other default settings
        * updating bugs fixes
    * provides
      * GUI -- to -- manage your
        * containers,
        * applications,
        * images

  * includes
    - [Docker Engine](../engine/_index.md)
    - Docker CLI client
    - [Docker Scout](../scout/_index.md) (additional subscription may apply)
    - [Docker Build](../build/index.md)
    - [Docker Extensions](extensions/index.md)
    - [Docker Compose](../compose/index.md)
    - [Docker Content Trust](../engine/security/trust/index.md)
    - [Kubernetes](https://github.com/kubernetes/kubernetes/)
    - [Credential Helper](https://github.com/docker/docker-credential-helpers/)

  * key features
    * Ability to 
      * containerize and share any application | 
        * ANY cloud platform
        * MULTIPLE languages & frameworks
      * | Windows machines, you -- can, via WSL 2 -- work natively | Linux
    * Quick installation & setup of a complete Docker development environment.
    * Includes the latest version of Kubernetes.
    * | Windows, ability -- to toggle between -- Linux & Windows containers
    * Fast and reliable performance
      * Reason: 🧠 native Windows Hyper-V virtualization 🧠
    * Volume mounting -- for -- code & data /
      * file change notifications
      * easy access to running containers | localhost network
    * -- access to a -- vast library of certified images & templates | [Docker Hub](https://hub.docker.com/)
    * work | ANY
      * development tool
      * languages
