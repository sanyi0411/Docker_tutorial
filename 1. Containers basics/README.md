
# Containers basics

## VMs vs Containers

```
VM                                  Container
```
```
+-----------------------+
|  App  |  App  |  App  |
|   1   |   2   |   3   |
+-----------------------+          +-----------------------+
| Bin/  | Bin/  | Bin/  |          |  App  |  App  |  App  |
|  Lib  |  Lib  |  Lib  |          |   1   |   2   |   3   |
+-----------------------+          +-----------------------+
|       |       |       |          | Bin/  | Bin/  | Bin/  |
| Guest | Guest | Guest |          |  Lib  |  Lib  |  Lib  |
|  OS   |  OS   |  OS   |          +-----------------------+
|       |       |       |          |  Container engine     |
+-----------------------+          +-----------------------+
|                       |          |                       |
|      Hypervisor       |          |   Operating System    |
|                       |          |                       |
+-----------------------+          +-----------------------+
|                       |          |                       |
|      Hardware         |          |       Hardware        |
|                       |          |                       |
+-----------------------+          +-----------------------+
```
VMs are pets:
- they have names
- we take care of them
- they have long uptimes
- we work on them manually
- SW update:
    - create a maintenance window
    - shut down the VM
    - install the new SW
    - start the VM
    - test
    - hard to rollback if something goes wrong

Containers are cattle:
- they don't have special names, only random generated IDs
- a container with an issue is shut down and new one is started
- most of the work on them are automated
- SW update:
    - build new image
    - start containers with new image
    - stop containers with old image
    - no need for maintenance window
    - change takes seconds
    - easy rollback if something goes wrong, just start containers with old version

## Containers

Containers achieve isolation using the Linux kernel's existing features:
- `namespaces` and 
- `cgroups`

Containers have their own process trees, network, users, file system