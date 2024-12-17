---
description: Find the latest recommended version of the Docker Compose file format
  for defining multi-container applications.
keywords: docker compose file, docker compose yml, docker compose reference, docker
  compose cmd, docker compose user, docker compose image, yaml spec, docker compose
  syntax, yaml specification, docker compose specification
title: Overview
toc_max: 4
toc_min: 1
grid:
- title: Version and name top-level element
  description: Understand version and name attributes for Compose.
  icon: feed
  link: /compose/compose-file/04-version-and-name/
- title: Services top-level element
  description: Explore all services attributes for Compose.
  icon: construction
  link: /compose/compose-file/05-services/
- title: Networks top-level element
  description: Find all networks attributes for Compose.
  icon: lan
  link: /compose/compose-file/06-networks/
- title: Volumes top-level element
  description: Explore all volumes attributes for Compose.
  icon: database
  link: /compose/compose-file/07-volumes/
- title: Configs top-level element
  description: Find out about configs in Compose.
  icon: settings
  link: /compose/compose-file/08-configs/
- title: Secrets top-level element
  description: Learn about secrets in Compose.
  icon: lock
  link: /compose/compose-file/09-secrets/
aliases:
- /compose/yaml/
- /compose/compose-file/compose-file-v1/
---

* Compose Specification
  * 👀== latest & recommended version of the Compose file format 👀
  * 👀== merging of v2.x + v3.x 👀
  * requirements
    * Docker Compose CLI v1.27.0+ (-- named as -- Compose V2)
  * allows
    * define a [Compose file](../compose-application-model.md) 
  * | Docker documentation, named as Docker Compose implementation
  * 👀if you want to implement your OWN version of the Compose Specification -> see [Compose Specification repository](https://github.com/compose-spec/compose-spec) 👀

* Compose V1
  * NO longer receives updates
  * NOT available | NEW releases of Docker Desktop

* Compose V2
  * included with ALL currently supported versions of Docker Desktop
  * see [Migrate to Compose V2](/compose/migrate)

* see 
  * [key features and use cases of Docker Compose](../intro/features-uses.md)
  * [get started guide](../gettingstarted.md)
