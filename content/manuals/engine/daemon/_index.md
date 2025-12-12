---
description: Configuring the Docker daemon
keywords: docker, daemon, configuration
title: Docker daemon configuration overview
linkTitle: Daemon
weight: 60
aliases:
  - /articles/chef/
  - /articles/configuring/
  - /articles/dsc/
  - /articles/puppet/
  - /config/thirdparty/
  - /config/thirdparty/ansible/
  - /config/thirdparty/chef/
  - /config/thirdparty/dsc/
  - /config/thirdparty/puppet/
  - /engine/admin/
  - /engine/admin/ansible/
  - /engine/admin/chef/
  - /engine/admin/configuring/
  - /engine/admin/dsc/
  - /engine/admin/puppet/
  - /engine/articles/chef/
  - /engine/articles/configuring/
  - /engine/articles/dsc/
  - /engine/articles/puppet/
  - /engine/userguide/
  - /config/daemon/
---

* `dockerd`
  * == 💡Docker daemon💡 

* requirements
  * ⚠️install Docker Engine MANUALLY⚠️
    * if you're using Docker Desktop -> [here](../../../manuals/desktop/settings-and-maintenance/settings.md#docker-engine)

## Configure the Docker daemon

*👀ways to configure the Docker daemon (⚠️choose 1 option⚠️)👀
  - [JSON configuration file](#---via----configuration-file)
    - 👀recommended one👀
      - Reason:🧠keeps ALL configurations | 1! place🧠
  - [`dockerd` CL's flags](#configuration-using-flags)

* ⚠️if you configure via BOTH ways -> Docker daemon
  * ❌will NOT start❌
  * prints an error message

### -- via -- configuration file

* == default file locationS | Docker daemon expects to find the configuration file

| OS and configuration | Default File location                      |
|----------------------|--------------------------------------------|
| Linux, regular setup | `/etc/docker/daemon.json`                  |
| Linux, rootless mode | `~/.config/docker/daemon.json`             |
| Windows              | `C:\ProgramData\docker\config\daemon.json` |

* | rootless mode,
  * if you set `XDG_CONFIG_HOME` variable -> expected file location: `$XDG_CONFIG_HOME/docker/daemon.json`

* `--config-file` CL's flag
  * == `dockerd --config-file`
  * [MORE](../../../reference/cli/do#daemon-configuration-file)

### -- via -- CL's flags

* requirements
  * start the Docker daemon MANUALLY

Here's an example of how to manually start the Docker daemon, using the same
configurations as shown in the previous JSON configuration:

```console
$ dockerd --debug \
  --tls=true \
  --tlscert=/var/docker/server.pem \
  --tlskey=/var/docker/serverkey.pem \
  --host tcp://192.168.59.3:2376
```

Learn about the available configuration options in the
[dockerd reference docs](/reference/cli/dockerd.md), or by
running:

```console
$ dockerd --help
```

## Daemon data directory

The Docker daemon persists all data in a single directory. This tracks
everything related to Docker, including containers, images, volumes, service
definition, and secrets.

By default the daemon stores data in:

- `/var/lib/docker` on Linux
- `C:\ProgramData\docker` on Windows

When using the [containerd image store](/manuals/engine/storage/containerd.md)
(the default for Docker Engine 29.0 and later on fresh installations), image
contents and container snapshots are stored in `/var/lib/containerd`. Other
daemon data (volumes, configs) remains in `/var/lib/docker`.

When using [classic storage drivers](/manuals/engine/storage/drivers/_index.md)
like `overlay2` (the default for upgraded installations), all data is stored in
`/var/lib/docker`.

### Configure the data directory location

You can configure the Docker daemon to use a different storage directory using
the `data-root` configuration option.

```json
{
  "data-root": "/mnt/docker-data"
}
```

The `data-root` option does not affect image and container data stored in
`/var/lib/containerd` when using the containerd image store. To change the
storage location of containerd snapshotters, use the system containerd
configuration file:

```toml {title="/etc/containerd/config.toml"}
version = 2
root = "/mnt/containerd-data"
```

Make sure you use a dedicated directory for each daemon. If two daemons share
the same directory, for example an NFS share, you will experience errors that
are difficult to troubleshoot.

## Next steps

Many specific configuration options are discussed throughout the Docker
documentation. Some places to go next include:

- [Automatically start containers](/manuals/engine/containers/start-containers-automatically.md)
- [Limit a container's resources](/manuals/engine/containers/resource_constraints.md)
- [Configure storage drivers](/manuals/engine/storage/drivers/select-storage-driver.md)
- [Container security](/manuals/engine/security/_index.md)
- [Configure the Docker daemon to use a proxy](./proxy.md)
