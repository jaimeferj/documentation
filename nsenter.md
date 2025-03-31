# nsenter: Executing commands within the context of another process

More than once, I needed to execute a command inside a container, but it does
not have installed the packages I need or just prefer to launch the commands
from the host machine. `nsenter` solves this problem, allowing to execute
commands in the container (container process) context.

## Find the Target Process ID (PID)

For instance, if you're working with a container, you can find its main process
PID. (Even if you're not using Docker, you can target any process.) For Docker
containers, you might run:

```bash
PID=$(docker inspect --format '{{.State.Pid}}' container_name)
```

## Enter the Process's Namespaces

Once you have the PID, you can run:

```bash
nsenter --target $PID --mount --uts --ipc --net --pid /bin/bash
```

This command enters the mount, UTS, IPC, network, and PID namespaces of the
target process, effectively giving you a shell within that process’s context.

### Namespace mounts

Depending which namespaces we want the command to have access to, we should add
or remove them from the parameters:

- \`--mount\`  Isolates the file system mount points, meaning any changes
(mounts/unmounts) in one mount namespace do not affect others.

- \`--uts\`  Isolates the UTS (UNIX Time-sharing System) namespace, which
includes system identifiers like the hostname and domain name.

- \`--ipc\`  Isolates inter-process communication resources, such as System V
IPC and POSIX message queues, ensuring that IPC mechanisms are private to the
namespace.

- \`--net\`  Isolates the network stack. This includes network interfaces, IP
addresses, routing tables, etc., allowing each namespace to have its own
network configuration.

- \`--pid\`  Isolates the process ID number space. Processes in different PID
namespaces can have the same PID without conflicting, and processes outside the
namespace won’t see those inside.
