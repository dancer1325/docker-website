---
title: Docker Engine
weight: 10
description: Find a comprehensive overview of Docker Engine, including how to install, storage details, networking, and more
keywords: Engine
params:
  sidebar:
    group: Open source
grid:
- title: Install Docker Engine
  description: Learn how to install the open source Docker Engine for your distribution.
  icon: download
  link: /engine/install
- title: Storage
  description: Use persistent data with Docker containers.
  icon: database
  link: /storage
- title: Networking
  description: Manage network connections between containers.
  icon: network_node
  link: /network
- title: Container logs
  description: Learn how to view and read container logs.
  icon: text_snippet
  link: /config/containers/logging/
- title: Prune
  description: Tidy up unused resources.
  icon: content_cut
  link: /config/pruning
- title: Configure the daemon
  description: Delve into the configuration options of the Docker daemon.
  icon: tune
  link: /config/daemon
- title: Rootless mode
  description: Run Docker without root privileges.
  icon: security
  link: /engine/security/rootless
- title: Deprecated features
  description: Find out what features of Docker Engine you should stop using.
  icon: folder_delete
  link: /engine/deprecated/
- title: Release notes
  description: Read the release notes for the latest version.
  icon: note_add
  link: /engine/release-notes
aliases:
- /edge/
- /engine/ce-ee-node-activate/
- /engine/migration/
- /engine/misc/
- /linux/
---

* Docker Engine
  * 👀== open source containerization technology / 👀
    * allows, for you applications,
      * building
      * containerizing
  * ⭐️== client-server application ⭐️/
    * server / contains
      * [`dockerd`](/engine/reference/commandline/dockerd)
        * == long-running daemon process / manage Docker objects (images, volumes, networks, containers)
    * APIs /
      * uses
        * programs can talk to
        * instruct the Docker daemon
          * _Example:_ by the CLI client
      * see [Docker APIs](api/index.md)
    * CLI client
      * see [`docker`](/engine/reference/commandline/cli/)
  * see [Docker Architecture](../get-started/overview.md#docker-architecture).

{{< grid >}}

## Licensing

Commercial use of Docker Engine obtained via Docker Desktop
within larger enterprises (exceeding 250 employees OR with annual revenue surpassing
$10 million USD), requires a [paid subscription](https://www.docker.com/pricing/).
Apache License, Version 2.0. See [LICENSE](https://github.com/moby/moby/blob/master/LICENSE) for the full license.
