---
title: Deployment and orchestration
keywords: orchestration, deploy, kubernetes, swarm,
description: Get oriented on some basics of Docker and install Docker Desktop.
---

* Containerization
  * allows
    * -- move easier to -- clouds & data centers
      * Reason: 🧠 applications run similarly 🧠
    * scale applications
* orchestrators
  * == tool / help 
    * maintenance of those applications,
    * replacement of failed containers automatically
    * manage lifecycle of containers
      * roll-out of updates
      * reconfigurations of those containers
    * scaling up & down containers
  * _Example:_ Kubernetes and Docker Swarm
    * 👀Docker Desktop -- provides -- development environments for BOTH 👀
    * see 
      1. [Set up and use a Kubernetes environment | your development machine](kube-deploy.md)
      2. [Set up and use a Swarm environment | your development machine](swarm-deploy.md)

## Turn on Kubernetes

* goal
  * run simple containerized workloads | Kubernetes

### Mac 6 Linux

1. Docker Dashboard, **Settings**, select the **Kubernetes** tab
2. Select the checkbox labeled **Enable Kubernetes**, and select **Apply & Restart**.
   1. green light == Kubernetes has been successfully enabled
3. Make simple exercises with Kubernetes
   1. create a text file called `pod.yaml`

       ```yaml
       apiVersion: v1
       kind: Pod
       metadata:
         name: demo
       spec:
         containers:
         - name: testpod
           image: alpine:latest
           command: ["ping", "8.8.8.8"]
       ```

   2. | terminal
      1. path | `pod.yaml`, and run

          ```console
          $ kubectl apply -f pod.yaml
          ```

      2. | ANY PATH

          ```console
          $ kubectl get pods
          
          # output
          NAME      READY     STATUS    RESTARTS   AGE
          demo      1/1       Running   0          4s
          ```

          ```console  
          $ kubectl logs demo
          

          # output 
          PING 8.8.8.8 (8.8.8.8): 56 data bytes
          64 bytes from 8.8.8.8: seq=0 ttl=37 time=21.393 ms
          64 bytes from 8.8.8.8: seq=1 ttl=37 time=15.320 ms
          64 bytes from 8.8.8.8: seq=2 ttl=37 time=11.111 ms
          ...
          ```

          ```console
          $ kubectl delete -f pod.yaml
          ```

{{< /tab >}}
{{< tab name="Windows" >}}

### Windows

* TODO:
1. From the Docker Dashboard, navigate to **Settings**, and select the **Kubernetes** tab.

2. Select the checkbox labeled **Enable Kubernetes**, and select **Apply & Restart**. Docker Desktop automatically sets up Kubernetes for you. You'll know that Kubernetes has been successfully enabled when you see a green light beside 'Kubernetes _running_' in the **Settings** menu.

3. To confirm that Kubernetes is up and running, create a text file called `pod.yaml` with the following content:

    ```yaml
    apiVersion: v1
    kind: Pod
    metadata:
      name: demo
    spec:
      containers:
      - name: testpod
        image: alpine:latest
        command: ["ping", "8.8.8.8"]
    ```

    This describes a pod with a single container, isolating a simple ping to 8.8.8.8.

4. In PowerShell, navigate to where you created `pod.yaml` and create your pod:

    ```console
    $ kubectl apply -f pod.yaml
    ```

5. Check that your pod is up and running:

    ```console
    $ kubectl get pods
    ```

    You should see something like:

    ```shell
    NAME      READY     STATUS    RESTARTS   AGE
    demo      1/1       Running   0          4s
    ```

6. Check that you get the logs you'd expect for a ping process:

    ```console
    $ kubectl logs demo
    ```

    You should see the output of a healthy ping process:

    ```shell
    PING 8.8.8.8 (8.8.8.8): 56 data bytes
    64 bytes from 8.8.8.8: seq=0 ttl=37 time=21.393 ms
    64 bytes from 8.8.8.8: seq=1 ttl=37 time=15.320 ms
    64 bytes from 8.8.8.8: seq=2 ttl=37 time=11.111 ms
    ...
    ```

7. Finally, tear down your test pod:

    ```console
    $ kubectl delete -f pod.yaml
    ```

{{< /tab >}}
{{< /tabs >}}

## Enable Docker Swarm

* goal
  * run simple containerized workloads | Docker Swarm

### Mac

1. | terminal

    ```console
    # initialize Docker Swarm mode
    $ docker swarm init
    
    # if ALL goes well -> Output
    Swarm initialized: current node (tjjggogqpnpj2phbfbz8jd5oq) is now a manager.

    To add a worker to this swarm, run the following command:

        docker swarm join --token SWMTKN-1-3e0hh0jd5t4yjg209f4g5qpowbsczfahv2dea9a1ay2l8787cf-2h4ly330d0j917ocvzw30j5x9 192.168.65.3:2377

    To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
    ```

    ```console
    # Run a simple Docker service
    # 1. uses an alpine-based filesystem 
    # 2. isolates a ping to 8.8.8.8
    $ docker service create --name demo alpine:latest ping 8.8.8.8
    ```

    ```console
    # Check / your service created 1 running container 
    $ docker service ps demo
    
    # sample output
    ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
    463j2s3y4b5o        demo.1              alpine:latest       docker-desktop      Running             Running 8 seconds ago
    ```

    ```console
    $ docker service logs demo
    
    # sample output
    demo.1.463j2s3y4b5o@docker-desktop    | PING 8.8.8.8 (8.8.8.8): 56 data bytes
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=0 ttl=37 time=13.005 ms
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=1 ttl=37 time=13.847 ms
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=2 ttl=37 time=41.296 ms
    ...
    ```

    ```console
    # tear down test service
    $ docker service rm demo
    ```
   
    ```console
    # kill the node for swarm
    $ docker swarm leave --force
    ```

### Windows

1. Open a PowerShell, and initialize Docker Swarm mode:

    ```console
    $ docker swarm init
    ```

    If all goes well, you should see a message similar to the following:

    ```shell
    Swarm initialized: current node (tjjggogqpnpj2phbfbz8jd5oq) is now a manager.

    To add a worker to this swarm, run the following command:

        docker swarm join --token SWMTKN-1-3e0hh0jd5t4yjg209f4g5qpowbsczfahv2dea9a1ay2l8787cf-2h4ly330d0j917ocvzw30j5x9 192.168.65.3:2377

    To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.
    ```

2. Run a simple Docker service that uses an alpine-based filesystem, and isolates a ping to 8.8.8.8:

    ```console
    $ docker service create --name demo alpine:latest ping 8.8.8.8
    ```

3. Check that your service created one running container:

    ```console
    $ docker service ps demo
    ```

    You should see something like:

    ```shell
    ID                  NAME                IMAGE               NODE                DESIRED STATE       CURRENT STATE           ERROR               PORTS
    463j2s3y4b5o        demo.1              alpine:latest       docker-desktop      Running             Running 8 seconds ago
    ```

4. Check that you get the logs you'd expect for a ping process:

    ```console
    $ docker service logs demo
    ```

    You should see the output of a healthy ping process:

    ```shell
    demo.1.463j2s3y4b5o@docker-desktop    | PING 8.8.8.8 (8.8.8.8): 56 data bytes
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=0 ttl=37 time=13.005 ms
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=1 ttl=37 time=13.847 ms
    demo.1.463j2s3y4b5o@docker-desktop    | 64 bytes from 8.8.8.8: seq=2 ttl=37 time=41.296 ms
    ...
    ```

5. Finally, tear down your test service:

    ```console
    $ docker service rm demo
    ```

{{< /tab >}}
{{< /tabs >}}

## CLI references

Further documentation for all CLI commands used in this article are available here:

- [`kubectl apply`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply)
- [`kubectl get`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get)
- [`kubectl logs`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs)
- [`kubectl delete`](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete)
- [`docker swarm init`](/engine/reference/commandline/swarm_init/)
- [`docker service *`](/engine/reference/commandline/service/)