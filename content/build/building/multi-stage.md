---
title: Multi-stage builds
description: |
  Learn about multi-stage builds and how you can use
  them to improve your builds and get smaller images
keywords: build, best practices
aliases:
- /engine/userguide/eng-image/multistage-build/
- /develop/develop-images/multistage-build/
---

* Multi-stage builds
  * 💡1 `FROM` / EACH stage 💡
    * == MULTIPLE `FROM` | your Dockerfile 
    * `0` -- stage1
  * allows
    * 👀optimizing Dockerfiles / easy to read & maintain 👀

## How to use?

* different base / EACH `FROM` 
* stage1's artifacts -- being copied to -- stage2's artifacts

* _Example:_ Dockerfile / 2 separate stages
  * stage1
    * builds a binary
  * stage2
    * stage1's binary is copied | HERE
    * NOT Go SDK & intermediate artifacts NOT | HERE

    ```dockerfile
    # HERE, stage0 starts
    FROM golang:{{% param "example_go_version" %}}
    WORKDIR /src
    COPY <<EOF ./main.go
    package main
    
    import "fmt"
    
    func main() {
      fmt.Println("hello, world")
    }
    EOF
    RUN go build -o /bin/hello ./main.go
    
    # HERE, stage1 starts
    FROM scratch
    # 0     ==  stage1's built artifact
    COPY --from=0 /bin/hello /bin/hello     
    CMD ["/bin/hello"]
    ```

## How to build?

* `docker build -t imageNameToBuild .`
  * == build as 1! stage

## How to add names | your build stages?

* `FROM .... as nickName`
* _Example:_ 
    ```dockerfile
    # stage 0 / added a name
    FROM golang:{{% param "example_go_version" %}} as build
    WORKDIR /src
    COPY <<EOF /src/main.go
    package main
    
    import "fmt"
    
    func main() {
      fmt.Println("hello, world")
    }
    EOF
    RUN go build -o /bin/hello ./main.go
    
    # stage1 / refers -- via name to the -- stage0 
    FROM scratch
    COPY --from=build /bin/hello /bin/hello
    CMD ["/bin/hello"]
    ```

## Stop | specific build stage

* 💡== | build your image,
  * NOT need to build ALL Dockerfile's stages 💡 
* steps
  * `docker build --target stageNameOrNumberInWhichStop -t buildNameToGive .`
    * == specify a target build stage 
    * _Example:_ | [PREVIOUS example](#how-to-add-names--your-build-stages)

      ```console
      $ docker build --target build -t hello .
      ```
* use cases
  * Debugging a specific build stage
  * Using a `debug` stage / 
    * ALL debugging symbols or tools enabled
    * lean `production` stage
  * MULTIPLE stagges /
    * | `testing` stage,
      * your app gets populated
    * | `production` stage,
      * uses real data

## Use an external image as a stage

* TODO:
When using multi-stage builds, you aren't limited to copying from stages you
created earlier in your Dockerfile. You can use the `COPY --from` instruction to
copy from a separate image, either using the local image name, a tag available
locally or on a Docker registry, or a tag ID. The Docker client pulls the image
if necessary and copies the artifact from there. The syntax is:

```dockerfile
COPY --from=nginx:latest /etc/nginx/nginx.conf /nginx.conf
```

## Use a previous stage as a new stage

You can pick up where a previous stage left off by referring to it when using
the `FROM` directive. For example:

```dockerfile
# syntax=docker/dockerfile:1

FROM alpine:latest AS builder
RUN apk --no-cache add build-base

FROM builder AS build1
COPY source1.cpp source.cpp
RUN g++ -o /binary source.cpp

FROM builder AS build2
COPY source2.cpp source.cpp
RUN g++ -o /binary source.cpp
```

## Differences between legacy builder and BuildKit

The legacy Docker Engine builder processes all stages of a Dockerfile leading
up to the selected `--target`. It will build a stage even if the selected
target doesn't depend on that stage.

[BuildKit](../buildkit/index.md) only builds the stages that the target stage
depends on.

For example, given the following Dockerfile:

```dockerfile
# syntax=docker/dockerfile:1
FROM ubuntu AS base
RUN echo "base"

FROM base AS stage1
RUN echo "stage1"

FROM base AS stage2
RUN echo "stage2"
```

With [BuildKit enabled](../buildkit/index.md#getting-started), building the `stage2` target in this Dockerfile means only `base` and `stage2` are processed.
There is no dependency on `stage1`, so it's skipped.

```console
$ DOCKER_BUILDKIT=1 docker build --no-cache -f Dockerfile --target stage2 .
[+] Building 0.4s (7/7) FINISHED                                                                    
 => [internal] load build definition from Dockerfile                                            0.0s
 => => transferring dockerfile: 36B                                                             0.0s
 => [internal] load .dockerignore                                                               0.0s
 => => transferring context: 2B                                                                 0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                0.0s
 => CACHED [base 1/2] FROM docker.io/library/ubuntu                                             0.0s
 => [base 2/2] RUN echo "base"                                                                  0.1s
 => [stage2 1/1] RUN echo "stage2"                                                              0.2s
 => exporting to image                                                                          0.0s
 => => exporting layers                                                                         0.0s
 => => writing image sha256:f55003b607cef37614f607f0728e6fd4d113a4bf7ef12210da338c716f2cfd15    0.0s
```

On the other hand, building the same target without BuildKit results in all
stages being processed:

```console
$ DOCKER_BUILDKIT=0 docker build --no-cache -f Dockerfile --target stage2 .
Sending build context to Docker daemon  219.1kB
Step 1/6 : FROM ubuntu AS base
 ---> a7870fd478f4
Step 2/6 : RUN echo "base"
 ---> Running in e850d0e42eca
base
Removing intermediate container e850d0e42eca
 ---> d9f69f23cac8
Step 3/6 : FROM base AS stage1
 ---> d9f69f23cac8
Step 4/6 : RUN echo "stage1"
 ---> Running in 758ba6c1a9a3
stage1
Removing intermediate container 758ba6c1a9a3
 ---> 396baa55b8c3
Step 5/6 : FROM base AS stage2
 ---> d9f69f23cac8
Step 6/6 : RUN echo "stage2"
 ---> Running in bbc025b93175
stage2
Removing intermediate container bbc025b93175
 ---> 09fc3770a9c4
Successfully built 09fc3770a9c4
```

The legacy builder processes `stage1`, even if `stage2` doesn't depend on it.
