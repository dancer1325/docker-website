- files / created inside a container → stored on writable conteainer layer
    - ==
        - once the container does NOT exist → data NOT persisted
            - → difficult to use out of the container
        - container’s writable layer — tightly coupled to — host machine where container is running
        - if you want to write into container’s writable layer → you need a Storage Drivers
            - performance using Volumes  > performance using Storage Drivers

TODO: Rest of section & prove all
