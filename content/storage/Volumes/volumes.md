---
description: Learn how to create, manage, and use volumes instead of bind mounts for
  persisting data generated and used by Docker.
title: Volumes
keywords: docker compose volumes, docker volumes, docker compose volume, docker volume
  mount, docker mount volume, docker volume create, docker volume location
aliases:
- /userguide/dockervolumes/
- /engine/tutorials/dockervolumes/
- /engine/userguide/dockervolumes/
- /engine/admin/volumes/volumes/
---

* == preferred mechanism -- for -- persisting data /
  * Docker containers
    * generated it
    * used it 
  * vs [bind mounts](bind-mounts.md)
    * dependency
      * bind mounts -- depend on --
        * directory structure
        * OS of the host machine
      * 👁️volumes are completely managed by Docker👁️ 
        * ⚠️== outside the scope of ANY container ⚠️
    * advantages of volumes
      * easier to back up or migrate
      * ways to manage 
        * Docker CLI commands
        * Docker API
      * work | Linux and Windows containers
      * more safely share among multiple containers
      * Volume drivers let 
        * store volumes | remote hosts or cloud providers
        * encrypt the contents
        * add other functionality
      * new volumes content -- can be pre-populated by a -- container
      * performance of volumes | Docker Desktop >> bind mounts from Mac and Windows hosts
  * vs persisting data | container's writable layer
    * better option
      * Reason: 🧠
        * volume does NOT increase the size of the containers
        * volume's contents -- exist outside the -- lifecycle of a given container 
  * use `rprivate` bind propagation
    * bind propagation is NOT configurable

![Volumes on the Docker host](volumes.png)

* if your container generates non-persistent state data -> use [tmpfs mount](tmpfs.md)
  * Reason: 🧠
    * avoid
      * storing the data anywhere permanently
      * writing | container's writable layer -> increase the container's performance

## Choose the -v or --mount flag

* `-v` or `--volume`  vs  `--mount`
  * `--mount` is more
    * explicit
    * verbose
  * syntax
    * `-v`
      * ALL the options -- are combined in -- 1 field
    * `--mount`
      * separates the options
  * if you need to specify volume driver options -> use `--mount`

* `-v` or `--volume`
  * `NameOfVolumes:ContainerPathToMountTheVolume:Option1,Option2,..`
    * `NameOfVolumes`
      * unique / host machine
      * if you use anonymous volumes -> it's omitted
    * `Option1,Option2,..`
      * optional
* `--mount`
  * `<key1>=<value1>,<key2>=<value2>,...`
    * order of each key is NOT important
  * allowed `type`S
    * [`bind`](bind-mounts.md)
    * `volume` or
      * the one related / discussed here
    * [`tmpfs`](tmpfs.md)
  * `source` of the mount
    * for named volumes -> name of the volume
    * for anonymous volumes -> field is omitted
    * ways to specify it
      * `source`
      * `src`
  * `destination`
    * == PathInTheContainerToMountTheFileOrDirectory
    * allowed names for this option
      * `destination`
      * `dst`
      * `target`
  * `readonly`
    * optional
      * if present -> bind mount is [mounted | container as read-only](#use-a-read-only-volume)
    * allowed names for this option
      * `readonly`
      * `ro`
  * `volume-opt`
    * \>=1 can be specified
    * `optionName=optionValue`

* TODO:
> **Warning**
>
> If your volume driver accepts a comma-separated list as an option,
> you must escape the value from the outer CSV parser. To escape a `volume-opt`,
> surround it with double quotes (`"`) and surround the entire mount parameter
> with single quotes (`'`).
>
> For example, the `local` driver accepts mount options as a comma-separated
> list in the `o` parameter. This example shows the correct way to escape the list.
>
> ```console
> $ docker service create \
>  --mount 'type=volume,src=<VOLUME-NAME>,dst=<CONTAINER-PATH>,volume-driver=local,volume-opt=type=nfs,volume-opt=device=<nfs-server>:<nfs-path>,"volume-opt=o=addr=<nfs-address>,vers=4,soft,timeo=180,bg,tcp,rw"'
>  --name myservice \
>  <IMAGE>
> ```
{ .warning }

The examples below show both the `--mount` and `-v` syntax where possible, with
`--mount` first.

### Differences between `-v` and `--mount` behavior 

As opposed to bind mounts, all options for volumes are available for both
`--mount` and `-v` flags.

Volumes used with services, only support `--mount`.

## Create and manage volumes

* 
    ```console
    $ docker volume create my-vol
    # proof that it's out of the scope of a container
    ```
    * create a volume    
* 
    ```console
    $ docker volume ls
    ```
    * list the volumes

*
    ```console
    $ docker volume inspect my-vol
    ```
    * inspect a volume 
*
    ```console
    $ docker volume rm my-vol
    ```
    * remove a volume

## Start a container with a volume

* if the volume defined for the container does NOT exist -> Docker create the volumes for you
* Via
  * `-v` / `--v`
    * 
        ```
        docker run -d --name devtest -v myvol2:/app nginx:latest
        # myvol2    name of the volume
        # /app      container path to mount the volume
        ``` 
      * `docker inspect devtest | jq '.[0].Mounts'`
        * check the volume mounted
      * `docker exec -it podId sh` & `ls` checking that '/app' directory exist
  * `--mount`
    * 
      ```
      docker run -d --name devtest --mount source=myvol2,target=/app nginx:latest
      ```
      * SAME as previous one
* clean ALL afterward
  ```
  $ docker container stop devtest
  $ docker container rm devtest
  $ docker volume rm myvol2
  ```

## Use a volume with Docker Compose

* options
  * create the volume -- via -- Docker compose

    ```yaml
    services:
      frontend:
        image: node:lts
        volumes:
          - myapp:/home/node/app
    volumes:
      myapp:
    ```

    * `docker compose up`
      * first time / you run it -> creates a volume
      * if you run subsequently -> Docker reuses the volume
  * 👁️create the volume directly OUTSIDE of Compose (`docker volume create`) & reference it inside `compose.yaml` afterward 👁️	

    ```yaml
    services:
      frontend:
        image: node:lts
        volumes:
          - myapp:/home/node/app
    volumes:
      myapp:
        external: true
    ```

* check [Volumes](../compose/compose-file/07-volumes.md)

### Start a service with volumes

When you start a service and define a volume, each service container uses its own
local volume. None of the containers can share this data if you use the `local`
volume driver. However, some volume drivers do support shared storage.

The following example starts an `nginx` service with four replicas, each of which
uses a local volume called `myvol2`.

```console
$ docker service create -d \
  --replicas=4 \
  --name devtest-service \
  --mount source=myvol2,target=/app \
  nginx:latest
```

Use `docker service ps devtest-service` to verify that the service is running:

```console
$ docker service ps devtest-service

ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE            ERROR               PORTS
4d7oz1j85wwn        devtest-service.1   nginx:latest        moby                Running             Running 14 seconds ago
```

You can remove the service to stop the running tasks:

```console
$ docker service rm devtest-service
```

Removing the service doesn't remove any volumes created by the service.
Volume removal is a separate step.

#### Syntax differences for services

The `docker service create` command doesn't support the `-v` or `--volume` flag.
When mounting a volume into a service's containers, you must use the `--mount`
flag.

### Populate a volume using a container

If you start a container which creates a new volume, and the container
has files or directories in the directory to be mounted such as `/app/`,
Docker copies the directory's contents into the volume. The container then
mounts and uses the volume, and other containers which use the volume also
have access to the pre-populated content.

To show this, the following example starts an `nginx` container and
populates the new volume `nginx-vol` with the contents of the container's
`/usr/share/nginx/html` directory. This is where Nginx stores its default HTML
content.

The `--mount` and `-v` examples have the same end result.

{{< tabs >}}
{{< tab name="`--mount`" >}}

```console
$ docker run -d \
  --name=nginxtest \
  --mount source=nginx-vol,destination=/usr/share/nginx/html \
  nginx:latest
```

{{< /tab >}}
{{< tab name="`-v`" >}}

```console
$ docker run -d \
  --name=nginxtest \
  -v nginx-vol:/usr/share/nginx/html \
  nginx:latest
```

{{< /tab >}}
{{< /tabs >}}

After running either of these examples, run the following commands to clean up
the containers and volumes. Note volume removal is a separate step.

```console
$ docker container stop nginxtest

$ docker container rm nginxtest

$ docker volume rm nginx-vol
```

## Use a read-only volume

For some development applications, the container needs to write into the bind
mount so that changes are propagated back to the Docker host. At other times,
the container only needs read access to the data. Multiple
containers can mount the same volume. You can simultaneously mount a
single volume as `read-write` for some containers and as `read-only` for others.

The following example changes the one above. It mounts the directory as a read-only
volume, by adding `ro` to the (empty by default) list of options, after the
mount point within the container. Where multiple options are present, you can separate
them using commas.

The `--mount` and `-v` examples have the same result.

{{< tabs >}}
{{< tab name="`--mount`" >}}

```console
$ docker run -d \
  --name=nginxtest \
  --mount source=nginx-vol,destination=/usr/share/nginx/html,readonly \
  nginx:latest
```

{{< /tab >}}
{{< tab name="`-v`" >}}

```console
$ docker run -d \
  --name=nginxtest \
  -v nginx-vol:/usr/share/nginx/html:ro \
  nginx:latest
```

{{< /tab >}}
{{< /tabs >}}

Use `docker inspect nginxtest` to verify that Docker created the read-only mount
correctly. Look for the `Mounts` section:

```json
"Mounts": [
    {
        "Type": "volume",
        "Name": "nginx-vol",
        "Source": "/var/lib/docker/volumes/nginx-vol/_data",
        "Destination": "/usr/share/nginx/html",
        "Driver": "local",
        "Mode": "",
        "RW": false,
        "Propagation": ""
    }
],
```

Stop and remove the container, and remove the volume. Volume removal is a
separate step.

```console
$ docker container stop nginxtest

$ docker container rm nginxtest

$ docker volume rm nginx-vol
```

## Share data between machines

When building fault-tolerant applications, you may need to configure multiple
replicas of the same service to have access to the same files.

![shared storage](images/volumes-shared-storage.webp)

There are several ways to achieve this when developing your applications.
One is to add logic to your application to store files on a cloud object
storage system like Amazon S3. Another is to create volumes with a driver that
supports writing files to an external storage system like NFS or Amazon S3.

Volume drivers allow you to abstract the underlying storage system from the
application logic. For example, if your services use a volume with an NFS
driver, you can update the services to use a different driver. For example, to
store data in the cloud, without changing the application logic.

## Use a volume driver

When you create a volume using `docker volume create`, or when you start a
container which uses a not-yet-created volume, you can specify a volume driver.
The following examples use the `vieux/sshfs` volume driver, first when creating
a standalone volume, and then when starting a container which creates a new
volume.

### Initial setup

The following example assumes that you have two nodes, the first of which is a Docker
host and can connect to the second node using SSH.

On the Docker host, install the `vieux/sshfs` plugin:

```console
$ docker plugin install --grant-all-permissions vieux/sshfs
```

### Create a volume using a volume driver

This example specifies an SSH password, but if the two hosts have shared keys
configured, you can exclude the password. Each volume driver may have zero or more
configurable options, you specify each of them using an `-o` flag.

```console
$ docker volume create --driver vieux/sshfs \
  -o sshcmd=test@node2:/home/test \
  -o password=testpassword \
  sshvolume
```

### Start a container which creates a volume using a volume driver

The following example specifies an SSH password. However, if the two hosts have
shared keys configured, you can exclude the password.
Each volume driver may have zero or more configurable options.

> **Note**
>
> If the volume driver requires you to pass any options,
> you must use the `--mount` flag to mount the volume, and not `-v`.

```console
$ docker run -d \
  --name sshfs-container \
  --volume-driver vieux/sshfs \
  --mount src=sshvolume,target=/app,volume-opt=sshcmd=test@node2:/home/test,volume-opt=password=testpassword \
  nginx:latest
```

### Create a service which creates an NFS volume

The following example shows how you can create an NFS volume when creating a service.
It uses `10.0.0.10` as the NFS server and `/var/docker-nfs` as the exported directory on the NFS server.
Note that the volume driver specified is `local`.

#### NFSv3

```console
$ docker service create -d \
  --name nfs-service \
  --mount 'type=volume,source=nfsvolume,target=/app,volume-driver=local,volume-opt=type=nfs,volume-opt=device=:/var/docker-nfs,volume-opt=o=addr=10.0.0.10' \
  nginx:latest
```

#### NFSv4

```console
$ docker service create -d \
    --name nfs-service \
    --mount 'type=volume,source=nfsvolume,target=/app,volume-driver=local,volume-opt=type=nfs,volume-opt=device=:/var/docker-nfs,"volume-opt=o=addr=10.0.0.10,rw,nfsvers=4,async"' \
    nginx:latest
```

### Create CIFS/Samba volumes

You can mount a Samba share directly in Docker without configuring a mount point on your host.

```console
$ docker volume create \
	--driver local \
	--opt type=cifs \
	--opt device=//uxxxxx.your-server.de/backup \
	--opt o=addr=uxxxxx.your-server.de,username=uxxxxxxx,password=*****,file_mode=0777,dir_mode=0777 \
	--name cif-volume
```

The `addr` option is required if you specify a hostname instead of an IP.
This lets Docker perform the hostname lookup.

### Block storage devices

You can mount a block storage device, such as an external drive or a drive partition, to a container.
The following example shows how to create and use a file as a block storage device,
and how to mount the block device as a container volume.

> **Important**
>
> The following procedure is only an example.
> The solution illustrated here isn't recommended as a general practice.
> Don't attempt this approach unless you're confident about what you're doing.
{ .important }

#### How mounting block devices works

Under the hood, the `--mount` flag using the `local` storage driver invokes the
Linux `mount` syscall and forwards the options you pass to it unaltered.
Docker doesn't implement any additional functionality on top of the native mount features supported by the Linux kernel.

If you're familiar with the
[Linux `mount` command](https://man7.org/linux/man-pages/man8/mount.8.html),
you can think of the `--mount` options as forwarded to the `mount` command in the following manner:

```console
$ mount -t <mount.volume-opt.type> <mount.volume-opt.device> <mount.dst> -o <mount.volume-opts.o>
```

To explain this further, consider the following `mount` command example.
This command mounts the `/dev/loop5` device to the path `/external-drive` on the system. 

```console
$ mount -t ext4 /dev/loop5 /external-drive
```

The following `docker run` command achieves a similar result, from the point of view of the container being run.
Running a container with this `--mount` option sets up the mount in the same way as if you had executed the
`mount` command from the previous example.

```console
$ docker run \
  --mount='type=volume,dst=/external-drive,volume-driver=local,volume-opt=device=/dev/loop5,volume-opt=type=ext4'
```

You can't run the `mount` command inside the container directly,
because the container is unable to access the `/dev/loop5` device.
That's why the `docker run` command uses the `--mount` option.

#### Example: Mounting a block device in a container

The following steps create an `ext4` filesystem and mounts it into a container.
The filesystem support of your system depends on the version of the Linux kernel you are using.

1. Create a file and allocate some space to it:

   ```console
   $ fallocate -l 1G disk.raw
   ```

2. Build a filesystem onto the `disk.raw` file:

   ```console
   $ mkfs.ext4 disk.raw
   ```

3. Create a loop device:

   ```console
   $ losetup -f --show disk.raw
   /dev/loop5
   ```

   > **Note**
   >
   > `losetup` creates an ephemeral loop device that's removed after
   > system reboot, or manually removed with `losetup -d`.

4. Run a container that mounts the loop device as a volume:

   ```console
   $ docker run -it --rm \
     --mount='type=volume,dst=/external-drive,volume-driver=local,volume-opt=device=/dev/loop5,volume-opt=type=ext4' \
     ubuntu bash
   ```

   When the container starts, the path `/external-drive` mounts the
   `disk.raw` file from the host filesystem as a block device.

5. When you're done, and the device is unmounted from the container,
   detach the loop device to remove the device from the host system:

   ```console
   $ losetup -d /dev/loop5
   ```

## Back up, restore, or migrate data volumes

Volumes are useful for backups, restores, and migrations.
Use the `--volumes-from` flag to create a new container that mounts that volume.

### Back up a volume

For example, create a new container named `dbstore`:

```console
$ docker run -v /dbdata --name dbstore ubuntu /bin/bash
```

In the next command:

- Launch a new container and mount the volume from the `dbstore` container
- Mount a local host directory as `/backup`
- Pass a command that tars the contents of the `dbdata` volume to a `backup.tar` file inside our `/backup` directory.

```console
$ docker run --rm --volumes-from dbstore -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /dbdata
```

When the command completes and the container stops, it creates a backup of
the `dbdata` volume.

### Restore volume from a backup

With the backup just created, you can restore it to the same container,
or to another container that you created elsewhere.

For example, create a new container named `dbstore2`:

```console
$ docker run -v /dbdata --name dbstore2 ubuntu /bin/bash
```

Then, un-tar the backup file in the new container’s data volume:

```console
$ docker run --rm --volumes-from dbstore2 -v $(pwd):/backup ubuntu bash -c "cd /dbdata && tar xvf /backup/backup.tar --strip 1"
```

You can use the techniques above to automate backup, migration, and restore
testing using your preferred tools.

## Remove volumes

A Docker data volume persists after you delete a container. There are two types
of volumes to consider:

- Named volumes have a specific source from outside the container, for example, `awesome:/bar`.
- Anonymous volumes have no specific source. Therefore, when the container is deleted, you can instruct the Docker Engine daemon to remove them.

### Remove anonymous volumes

To automatically remove anonymous volumes, use the `--rm` option. For example,
this command creates an anonymous `/foo` volume. When you remove the container,
the Docker Engine removes the `/foo` volume but not the `awesome` volume.

```console
$ docker run --rm -v /foo -v awesome:/bar busybox top
```

> **Note**
>
> If another container binds the volumes with
> `--volumes-from`, the volume definitions are _copied_ and the
> anonymous volume also stays after the first container is removed.

### Remove all volumes

To remove all unused volumes and free up space:

```console
$ docker volume prune
```

## Next steps

- Learn about [bind mounts](bind-mounts.md).
- Learn about [tmpfs mounts](tmpfs.md).
- Learn about [storage drivers](/storage/storagedriver/).
- Learn about [third-party volume driver plugins](/engine/extend/legacy_plugins/).
