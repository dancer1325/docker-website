# `ENV`
* Syntax

`
    ENV
    key1=value1
    key2=value2
    ...
`

`
    ENV key1=value1
    ENV key2=value2
    ...
`

* allows
  * set environment variables / 👁️persist when the container runs 👁️
* if you do NOT scape `“`  → removed
* wrapping under `“` & `\` → include spaces
* if Multi-stage builds & set `ENV` for ancestors → environment variables are inherited -- TODO: Check --
* TODO:
## How to test all?
* `docker build -t env .`
* `docker run env`
  * `docker run env --env GIRL_NAME=Noelia`  to override the defined in the Dockerfile
    * Problems:
      * Problem1: "Error response from daemon: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "--env": executable file not found in $PATH: unknown."
        * Solution: TODO:
* ways to check 
  * `docker ps` & `docker exec -it ContainerId bash` & `printenv` OR
  * `docker inspect env | jq '.[0].Config.Env'`
