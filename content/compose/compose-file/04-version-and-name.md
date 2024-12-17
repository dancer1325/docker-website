---
title: Version and name top-level elements
description: Understand when and if to set the version and name top-level element
keywords: compose, compose specification, services, compose file reference
---

## Version top-level element (optional)

* top-level `version` property
  * defined by the Compose Specification / backward compatibility
    * -> Compose validates whether it can fully parse the Compose file 
  * 👀ONLY informative 👀
    * ❌== NOT used to select an exact schema -- to validate the -- Compose file ❌
      * Reason: 🧠MOST recent schema | it's implemented 🧠

* 👀if SOME fields are unknown (_Example:_ Compose file was written with fields / defined by a NEWER version of the Specification) -> you'll receive a warning message 👀

## Name top-level element

* top-level `name` property
  * if you do NOT specify -- defined by the Specification, as the -- project name

* [other ways to name Compose projects](../project-name.md)

* `COMPOSE_PROJECT_NAME`
  * == environment variable / has the project name (INDEPENDENTLY, if it's the default or specify by you)
  * [interpolation](12-interpolation.md) 
    * _Example:_ 

    ```yml
    services:
      foo:
        image: busybox
        environment:
          - COMPOSE_PROJECT_NAME
        command: echo "I'm running ${COMPOSE_PROJECT_NAME}"
    ```
