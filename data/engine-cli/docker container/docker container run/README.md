

# `-v`
* Check '../storage/volumes'
## Examples
* `docker  run  -v $(pwd):$(pwd) -w $(pwd) -i -t  ubuntu pwd`
  * Check `-w` section
  * Check the output of this command
  * `docker inspect ContainerId | jq '.[0].Mounts'`  -- `docker ps -all` to check the ContainerId -- 
    * host's path (source) & container's path (destination) are the current directory -- -v $(pwd):$(pwd) --
    * volume has NOT name!!
* `docker  run  -v ./folderForExamples:/folderForExamples -w /folderForExamples -i -t  ubuntu pwd`
  * == previous, but for relative paths
* `docker  run  -v ./NoExistingFolderAndCreatedByDocker:/foo -w /foo -i -t  ubuntu pwd`
  * Problems:
    * Problem1: 'ERRO[0000] error waiting for container:'
      * Note: The folder is created automatically by Docker & `docker ps --all` you check that the container has started
      * Reason: `pwd` does NOT wait for to run the command
      * Solution: Run the command again, to get succeed by the command -- https://github.com/docker/docs/issues/19934 --