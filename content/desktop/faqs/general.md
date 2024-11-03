---
description: Frequently asked Docker Desktop questions for all platforms
keywords: desktop, mac, windows, faqs
title: General FAQs for Desktop
aliases:
- /mackit/faqs/
- /docker-for-mac/faqs/
- /docker-for-windows/faqs/
- /desktop/faqs/
---

### Can I use Docker Desktop offline?

Yes, you can use Docker Desktop offline. However, you
cannot access features that require an active internet
connection. Additionally, any functionality that requires you to sign in won't work while using Docker Desktop offline or in air-gapped environments.
This includes:

- The resources in the [Learning Center](../use-desktop/index.md)
- Pulling or pushing an image to Docker Hub
- [Image Access Management](../../security/for-developers/access-tokens.md)
- [Static vulnerability scanning](../../docker-hub/vulnerability-scanning.md)
- Viewing remote images in the Docker Dashboard
- Setting up [Dev Environments](../dev-environments/index.md)
- Docker Build when using [BuildKit](../../build/buildkit/index.md#getting-started).
  You can work around this by disabling BuildKit. Run `DOCKER_BUILDKIT=0 docker build .` to disable BuildKit.
- [Kubernetes](../kubernetes.md) (Images are download when you enable Kubernetes for the first time)
- Checking for updates
- [In-app diagnostics](../troubleshoot/overview.md#diagnose-from-the-app) (including the [Self-diagnose tool](../troubleshoot/overview.md#diagnose-from-the-app))
- Sending usage statistics

### How do I connect to the remote Docker Engine API?

* goal
  * connect -- to the -- remote Docker Engine API
* provide the location of the Docker Engine API
  * -- for -- 
    * Docker clients
    * development tools
  * ways
    * | Mac & Windows WSL 2
      * -- via an -- Unix socket `unix:///var/run/docker.sock`
      * if you are working with applications / expect settings for `DOCKER_HOST` & `DOCKER_CERT_PATH` -> specify 

        ```console
        $ export DOCKER_HOST=unix:///var/run/docker.sock
        ```
    * | Docker Desktop Windows
      * -- via -- 
        * **named pipe**: `npipe:////./pipe/docker_engine`, or
        * **TCP socket** `tcp://localhost:2375`
  * see [Docker Engine API](../../engine/api/index.md)

### How do I connect from a container to a service on the host?

The host has a changing IP address, or none if you have no network access.
We recommend that you connect to the special DNS name `host.docker.internal`,
which resolves to the internal IP address used by the host.

For more information and examples, see [how to connect from a container to a service on the host](../networking.md#i-want-to-connect-from-a-container-to-a-service-on-the-host).

### Can I pass through a USB device to a container?

It is not possible to pass through a USB device (or a
serial port) to a container as it requires support at the hypervisor level.
