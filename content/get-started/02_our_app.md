---
title: Containerize an application
keywords: dockerfile example, Containerize an application, run docker file, running
  docker file, how to run dockerfile, example dockerfile, how to create a docker container,
  create dockerfile, simple dockerfile, creating containers
description: Follow this step-by-step guide to learn how to create and run a containerized
  application using Docker
aliases:
- /get-started/part2/
---

* goal
  * build the app's image
  * start an container / has this image

# Steps
* Follow all in https://github.com/dancer1325/docker-getting-started-app

## Build the app's image

* TODO:
The `CMD` directive specifies the default command to run when starting a container from this image.

   Finally, the `-t` flag tags your image. Think of this as a human-readable name for the final image. Since you named the image `getting-started`, you can refer to that image when you run a container.

   The `.` at the end of the `docker build` command tells Docker that it should look for the `Dockerfile` in the current directory.

## Start an app container

Now that you have an image, you can run the application in a container using the `docker run` command.

1. Run your container using the `docker run` command and specify the name of the image you just created:

   ```console
   $ docker run -dp 127.0.0.1:3000:3000 getting-started
   ```

   The `-d` flag (short for `--detach`) runs the container in the background.
   This means that Docker starts your container and returns you to the terminal
   prompt. You can verify that a container is running by viewing it in Docker
   Dashboard under **Containers**, or by running `docker ps` in the terminal.

   The `-p` flag (short for `--publish`) creates a port mapping between the host
   and the container. The `-p` flag takes a string value in the format of
   `HOST:CONTAINER`, where `HOST` is the address on the host, and `CONTAINER` is
   the port on the container. The command publishes the container's port 3000 to
   `127.0.0.1:3000` (`localhost:3000`) on the host. Without the port mapping,
   you wouldn't be able to access the application from the host.

2. After a few seconds, open your web browser to [http://localhost:3000](http://localhost:3000).
   You should see your app.

   ![Empty todo list](images/todo-list-empty.webp)
   

3. Add an item or two and see that it works as you expect. You can mark items as complete and remove them. Your frontend is successfully storing items in the backend.


At this point, you have a running todo list manager with a few items.

If you take a quick look at your containers, you should see at least one container running that's using the `getting-started` image and on port `3000`. To see your containers, you can use the CLI or Docker Desktop's graphical interface.

{{< tabs >}}
{{< tab name="CLI" >}}

Run the following `docker ps` command in a terminal to list your containers.

```console
$ docker ps
```
Output similar to the following should appear.
```console
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
df784548666d        getting-started     "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes        127.0.0.1:3000->3000/tcp   priceless_mcclintock
```

{{< /tab >}}
{{< tab name="Docker Desktop" >}}

In Docker Desktop, select the **Containers** tab to see a list of your containers.

![Docker Desktop with get-started container running](images/dashboard-two-containers.webp)

{{< /tab >}}
{{< /tabs >}}

## Summary

In this section, you learned the basics about creating a Dockerfile to build an image. Once you built an image, you started a container and saw the running app.

Related information:

 - [Dockerfile reference](../engine/reference/builder.md)
 - [docker CLI reference](/engine/reference/commandline/cli/)
 - [Build with Docker guide](../build/guide/index.md)

## Next steps

Next, you're going to make a modification to your app and learn how to update your running application with a new image. Along the way, you'll learn a few other useful commands.

{{< button text="Update the application" url="03_updating_app.md" >}}
