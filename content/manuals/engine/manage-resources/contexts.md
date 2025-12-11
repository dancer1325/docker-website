---
title: Docker contexts
description: Learn about managing multiple daemons from a single client with contexts
keywords: engine, context, cli, daemons, remote
aliases:
  - /engine/context/working-with-contexts/
---

* goal
  * Docker contexts

## Introduction

* contexts
  * allows you to
    * manage Docker daemons | 1! client
      * _Examples:_ 2 contexts | 1! Docker client
        * default context / run locally
        * remote, shared context
  * == ALL information / required to manage resources | daemon
    * == 💡name and description + endpoint configuration + TLS info💡

* `docker context`
  * allows you to
    * configure these contexts

* `docker context use <context-name>` 
  * allows you to,
    * switch between contexts

## Prerequisites

- Docker client / supports the top-level `context` command
  - `docker context`
    - check that your Docker client supports contexts

## The anatomy of a context

* `docker context ls` OR `docker context inspect`
  * list AVAILABLE contexts
  * _Example:_ 

    ```console
    $ docker context ls
    NAME        DESCRIPTION                               DOCKER ENDPOINT               ERROR
    default *                                             unix:///var/run/docker.sock
    ```
    ```console
    $ docker context inspect default
    [
        {
            "Name": "default",
            "Metadata": {},
            "Endpoints": {
                "docker": {
                    "Host": "unix:///var/run/docker.sock",
                    "SkipTLSVerify": false
                }
            },
            "TLSMaterial": {},
            "Storage": {
                "MetadataPath": "\u003cIN MEMORY\u003e",
                "TLSPath": "\u003cIN MEMORY\u003e"
            }
        }
    ]
    ```

* `NAME`
  * `*`
    * == ACTIVE context
* `DOCKER ENDPOINT`
  * == way to talk -- to a -- daemon
  * 👀ways to override it -- via -👀
    * environment variables
      * `DOCKER_HOST`
      * `DOCKER_CONTEXT`
    * CL flags
      * `--context`
      * `--host`

### Create a new context -- `docker context create` --

* _Example:_ 

    ```console
    $ docker context create docker-test --docker host=tcp://docker:2375
    docker-test
    Successfully created context "docker-test"
    ```
* TODO:
The new context is stored in a `meta.json` file below `~/.docker/contexts/`.
Each new context you create gets its own `meta.json` stored in a dedicated sub-directory of `~/.docker/contexts/`.

You can view the new context with `docker context ls` and `docker context inspect <context-name>`.

```console
$ docker context ls
NAME          DESCRIPTION                             DOCKER ENDPOINT               ERROR
default *                                             unix:///var/run/docker.sock
docker-test                                           tcp://docker:2375
```

The current context is indicated with an asterisk ("\*").

## Use a different context -- `docker context use` --

* _Example:_ 
    ```console
    $ docker context use docker-test
    docker-test
    Current context is now "docker-test"
    ```

    ```console
    # check the ACTIVE context
    $ docker context ls
    NAME            DESCRIPTION                           DOCKER ENDPOINT               ERROR
    default                                               unix:///var/run/docker.sock
    docker-test *                                         tcp://docker:2375
    ```

You can also set the current context using the `DOCKER_CONTEXT` environment variable.
The environment variable overrides the context set with `docker context use`.

Use the appropriate command below to set the context to `docker-test` using an environment variable.

{{< tabs >}}
{{< tab name="PowerShell" >}}

```ps
> $env:DOCKER_CONTEXT='docker-test'
```

{{< /tab >}}
{{< tab name="Bash" >}}

```console
$ export DOCKER_CONTEXT=docker-test
```

{{< /tab >}}
{{< /tabs >}}

Run `docker context ls` to verify that the `docker-test` context is now the
active context.

You can also use the global `--context` flag to override the context.
The following command uses a context called `production`.

```console
$ docker --context production container ls
```

## Exporting and importing Docker contexts

You can use the `docker context export` and `docker context import` commands
to export and import contexts on different hosts.

The `docker context export` command exports an existing context to a file.
The file can be imported on any host that has the `docker` client installed.

### Exporting and importing a context

The following example exports an existing context called `docker-test`.
It will be written to a file called `docker-test.dockercontext`.

```console
$ docker context export docker-test
Written file "docker-test.dockercontext"
```

Check the contents of the export file.

```console
$ cat docker-test.dockercontext
```

Import this file on another host using `docker context import`
to create context with the same configuration.

```console
$ docker context import docker-test docker-test.dockercontext
docker-test
Successfully imported context "docker-test"
```

You can verify that the context was imported with `docker context ls`.

The format of the import command is `docker context import <context-name> <context-file>`.

## Update context´s fields -- `docker context update` --

* _Example:_ 
  ```console
  $ docker context update docker-test --description "Test context"
  docker-test
  Successfully updated context "docker-test"
  ```
