---
title: Registry
description: The Docker Hub registry implementation 
keywords: registry, distribution, docker hub, spec, schema, api, manifest, auth
aliases:
  - /registry/compatibility/
  - /registry/configuration/
  - /registry/deploying/
  - /registry/deprecated/
  - /registry/garbage-collection/
  - /registry/help/
  - /registry/insecure/
  - /registry/introduction/
  - /registry/notifications/
  - /registry/recipes/
  - /registry/recipes/apache/
  - /registry/recipes/nginx/
  - /registry/recipes/osx-setup-guide/
  - /registry/spec/
  - /registry/spec/api/
  - /registry/spec/auth/
  - /registry/spec/auth/jwt/
  - /registry/spec/auth/oauth/
  - /registry/spec/auth/scope/
  - /registry/spec/auth/token/
  - /registry/spec/deprecated-schema-v1/
  - /registry/spec/implementations/
  - /registry/spec/json/
  - /registry/spec/manifest-v2-1/
  - /registry/spec/manifest-v2-2/
  - /registry/spec/menu/
  - /registry/storage-drivers/
  - /registry/storage-drivers/azure/
  - /registry/storage-drivers/filesystem/
  - /registry/storage-drivers/gcs/
  - /registry/storage-drivers/inmemory/
  - /registry/storage-drivers/oss/
  - /registry/storage-drivers/s3/
  - /registry/storage-drivers/swift/
---

* Registry
  * == open source implementation /
    * allows,
      * about container images & other content
        * storing
        * distributing container
    * 👀-- was donated to the -- CNCF 👀
      * see [distribution/distribution] 

* Docker Hub registry implementation
  * 👀-- based on -- Distribution 👀 /
    * -- implements -- v1.0.1 OCI distribution [specification]

## Supported media types

* supported image manifests -- by -- Docker Hub
  * for pulling images
    - [OCI image manifest]
    - [Docker image manifest v2, schema 2]
    - Docker image manifest v2, schema 1
    - Docker image manifest v1
  * for pushing images
    - [OCI image manifest]
    - [Docker image manifest version 2, schema 2]
* supported OCI artifacts -- by -- Docker Hub
  * see [OCI artifacts]

## Authentication

* Docker Hub registry authentication
  - [Token authentication specification][token]
  - [OAuth 2.0 token authentication][oauth2]
  - [JWT authentication][jwt]
  - [Token scope and access][scope]

<!-- links -->

[distribution/distribution]: https://distribution.github.io/distribution/
[specification]: https://github.com/opencontainers/distribution-spec/blob/v1.0.1/spec.md
[OCI image manifest]: https://github.com/opencontainers/image-spec/blob/main/manifest.md
[Docker image manifest version 2, schema 2]: https://distribution.github.io/distribution/spec/manifest-v2-2/
[OCI artifacts]: /docker-hub/oci-artifacts/
[oauth2]: https://distribution.github.io/distribution/spec/auth/oauth/
[jwt]: https://distribution.github.io/distribution/spec/auth/jwt/
[token]: https://distribution.github.io/distribution/spec/auth/token/
[scope]: https://distribution.github.io/distribution/spec/auth/scope/
