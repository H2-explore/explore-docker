# Explore Docker

Creating a mini Docker system in attempt to understand how Docker actually works. \
plus learning some GO ;)

# How does Docker work?
What is docker actually? 🤔

## Docker run - how container process starts
In one sentence, `docker run` launches a process inside an **isolated** Linux environment on **your system**, with its own filesystem, process IDs, and optionally resource limits, so it behaves like it’s running on a separate machine.

`docker run`, in its essence, is mostly:
- Linux namespaces $\rightarrow$ process isolation
- cgroups $\rightarrow$ resource limits
- chroot / pivot_root $\rightarrow$ filesystem isolation
- exec $\rightarrow$ start the process

Let's break that down a little bit
### Linux Namespaces (process isolation)
Namespaces allocates processes only visible to the container to run your application, hence making them isolated.
| Namespace | What it isolates                                |
| --------- | ----------------------------------------------- |
| `pid`     | Process IDs (`ps` inside container starts at 1) |
| `mnt`     | Filesystem mount points (`/` is container root) |
| `uts`     | Hostname and domain info                        |
| `net`     | Network stack (interfaces, IPs)                 |
| `ipc`     | Inter-process communication                     |
| `user`    | User IDs (UID/GID mapping)                      |

### Cgroups (resource limits)
Cgroups restrict how much CPU, memory, or I/O a container can use.
Example:
- `docker run --memory=256m ...` $\rightarrow$ a memory cgroup is created
- The process inside sees /proc/meminfo limited
- Outside processes see the real host memory

### Chroot / pivot_root (filesystem isolation)
Combined with namespaces, making the container thinks it has its own root filesystem separate from the host

### Exec (start the process)
After the isolation setup, Docker executes the command inside the container:
```
execve("/bin/sh", args, env)
```


# Go commands
| Command                                              | What it does                                    |
| ---------------------------------------------------- | ----------------------------------------------- |
| `go run main.go`                                     | Runs that file immediately                      |
| `go run .`                                           | Runs all Go files in the current folder/package |
| `go build -o mydocker`                               | Compiles your project into a binary `mydocker`  |
| `./mydocker`                                         | Runs the compiled binary                        |
| `GOOS=linux GOARCH=amd64 go build -o mydocker-linux` | Builds Linux binary on macOS                    |
