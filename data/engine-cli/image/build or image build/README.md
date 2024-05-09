# Prerequisites
* [Install docker](https://docs.docker.com/get-docker/)

# Syntax
* `docker image build [OPTIONS] PATH | URL | -`
  * Note: 👁️`|`  == OR 👁️
  * Check https://github.com/dancer1325/docker-getting-started-app

# Dockerfile + context -> build an image
## context
* := set of files located in the specified PATH or URL
* build process — can refer to — any of the files in the context
  * Check `COPY` in https://github.com/dancer1325/docker-getting-started-app/blob/main/Dockerfile
### URL
* can refer to
#### GIT repositories
* & its submodules — are recursively fetched by the — system
  * way
    * 🧠Docker pulls the repository — into a → localhost’s temporary directory — sends as →  context to the @Daemon CLI (dockerd) 🧠 
  * if `URL` contains a fragment which recursively clones the repository + submodules → use `git clone --recursive`
    * Problems
      * Problem1: `--recursive`? https://github.com/docker/docs/issues/19502
* commit history NOT preserved
  * `docker history getting-started` which does NOT show git history of the repo
* local COPY of user credentials or VPNs — gives you the ability to access to — private repositories
  * _Example:_ `COPY ./config/credentials.json /app/config/credentials.json into a Dockerfile
* valid references
  * `#mytag`
    * `docker build -t withtag github.com/dancer1325/docker-getting-started-app.git#tagtobuild`
  * `#mybranch`
    * `docker build -t withbranch github.com/dancer1325/docker-getting-started-app.git#branchtobuild`
  * `#pull/NumberOfPR/head`
    * `docker build -t withpr github.com/dancer1325/docker-getting-started-app.git#pull/1/head`
    * Note: If you do NOT add `/head` -> it fails
  * `#:folderName`
    * `docker build -t withfolder github.com/dancer1325/docker-getting-started-app.git#:src`
      * Problems:
        * Problem1: "ERROR: failed to solve: failed to read dockerfile:"
        * Solution: Create a Dockerfile in the folder that you specify
  * `#master:folderName`
    * == approach to the last one
  * `#tagName:folderName`
    * == approach to the last one
  * `#branchName:folderName`
    * `docker build -t withbranchandfolder github.com/dancer1325/docker-getting-started-app.git#dockerfileInAnotherFolder:dockerFiles`
#### pre-packaged tarball contexts
* TODO:
#### plain .txt
* TODO:


### PATH
* TODO:

## build process — can refer to — any of the files in the context
* Check '../reference/Dockerfile', `COPY`

# ===
* `docker image build [OPTIONS] PATH | URL | -`
  * `docker image build -t getstartedimagebuild https://github.com/dancer1325/docker-getting-started-app`
    * Problems:
      * Problem1: "ERROR: failed to solve: dockerfile parse error on line 7"
        * Attempt1: `docker image build -t getstartedimagebuil https://github.com/dancer1325/docker-getting-started-app/blob/main/Dockerfile`
        * Solution: `docker image build -t getstartedimagebuild github.com/dancer1325/docker-getting-started-app`
* `docker buildx build [OPTIONS] PATH | URL | -`
  * `docker buildx build -t getstartedbuildx github.com/dancer1325/docker-getting-started-app`
* `docker builder build [OPTIONS] PATH | URL | -`
  * `docker builder build -t getstartedbuilder github.com/dancer1325/docker-getting-started-app`
* `docker build [OPTIONS] PATH | URL | -`


# Examples
# TODO:
# Build with `-`
* `docker build - < Dockerfile`
  * Create a docker image
    * without passing context -- `-` --
    * passing as input Dockerfile -- `< Dockerfile` --
  * `docker image list` to check that a <none> image has built
* Dockerfile `ADD` only works if it refers to a remote URL
  * create '/target' & add the Dockerfile
  * `tar -czf context.tar.gz target`
  * `docker build - < context.tar.gz`
# TODO:
## `-f DockerFileName` OR `--file DockerFileName`
* allows
  * specifying the location of the Dockerfile
* `docker build -f target/Dockerfile .` 

# TODO: