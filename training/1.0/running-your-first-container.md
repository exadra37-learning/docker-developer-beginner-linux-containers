# 1.0 Running Your First Container

Remember `docker <command> --help` is our best friend.

For more details than help provides try `man docker <command>`.

The above 2 commands can save us a lot of Googling ;).

#### Checking Docker Version I am using:

```bash
$docker --version
Docker version 1.13.1, build 092cba3
```

Let's see what I have learned from this first Docker Training Session...


# Docker Pull

#### Docker Pull usage signature:

```bash
$ docker pull --help | grep 'Usage:'
Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]
```

#### Pulling the Alpine image:

```bash
docker pull alpine
Using default tag: latest
latest: Pulling from library/alpine
0a8490d0dfd3: Pull complete
Digest: sha256:dfbd4a3a8ebca874ebd2474f044a0b33600d4523d03b0df76e5c5986cb02d7e8
Status: Downloaded newer image for alpine:latest
```

#### What else can we do with Docker Pull:

```bash
docker pull --help

Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Pull an image or a repository from a registry

Options:
  -a, --all-tags                Download all tagged images in the repository
      --disable-content-trust   Skip image verification (default true)
      --help                    Print usage
```

# Docker Images

#### Let's see Docker Images usage signature:

```bash
$ docker images --help | grep 'Usage:'
Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]
```

#### Checking we have the Alpine image:

```bash
$  docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              48b5124b2768        6 weeks ago         1.84 kB
alpine              latest              88e169ea8f46        2 months ago        3.98 MB
```

# Docker Run

#### Let's see Docker Run usage signature:

```bash
 docker run --help | grep 'Usage:'
Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]
```

#### Running the Alpine Container:

```bash
$ docker run alpine ls -l
total 52
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 bin
drwxr-xr-x    5 root     root           340 Feb 28 22:48 dev
drwxr-xr-x   14 root     root          4096 Feb 28 22:48 etc
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 home
drwxr-xr-x    5 root     root          4096 Dec 26 21:32 lib
drwxr-xr-x    5 root     root          4096 Dec 26 21:32 media
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 mnt
dr-xr-xr-x  294 root     root             0 Feb 28 22:48 proc
drwx------    2 root     root          4096 Dec 26 21:32 root
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 run
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 sbin
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 srv
dr-xr-xr-x   13 root     root             0 Feb 28 22:48 sys
drwxrwxrwt    2 root     root          4096 Dec 26 21:32 tmp
drwxr-xr-x    7 root     root          4096 Dec 26 21:32 usr
drwxr-xr-x   12 root     root          4096 Dec 26 21:32 var
```

So we can see the root content inside the Docker Container :).

### Running a Shell Inside the Alpine Container

#### Let's try it:

```bash
$ docker run alpine /bin/sh
```

Nothing happened because we are not in interactive mode...

#### Run in Interactive mode:

```bash
$ docker run -it alpine /bin/sh
/ # ls -l
total 52
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 bin
drwxr-xr-x    5 root     root           360 Feb 28 22:56 dev
drwxr-xr-x   14 root     root          4096 Feb 28 22:56 etc
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 home
drwxr-xr-x    5 root     root          4096 Dec 26 21:32 lib
drwxr-xr-x    5 root     root          4096 Dec 26 21:32 media
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 mnt
dr-xr-xr-x  295 root     root             0 Feb 28 22:56 proc
drwx------    2 root     root          4096 Feb 28 22:56 root
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 run
drwxr-xr-x    2 root     root          4096 Dec 26 21:32 sbin
drwxr-xr-x
2 root     root          4096 Dec 26 21:32 srv
dr-xr-xr-x   13

 root     root             0 Feb 28 22:56 sys
drwxrwxrwt    2 root     root          4096 Dec 26 21:32 tmp
drwxr-xr-x    7 root     root          4096 Dec 26 21:32 usr
drwxr-xr-x   12 root     root          4096 Dec 26 21:32 var
/ # uname -a
Linux 55f528241902 3.16.0-44-generic #59-Ubuntu SMP Tue Jul 7 02:07:39 UTC 2015 x86_64 Linux
/ # exit
```

Cool we can now work inside the Docker Container.


##### What means option -i:

```bash
$ docker run --help | grep '\-i,'
  -i, --interactive                           Keep STDIN open even if not attached
```

##### What means option -t:

```bash
$ docker run --help | grep '\-t,'
  -t, --tty                                   Allocate a pseudo-TTY
  ```

#### Lets see Docker Run Help:

```bash
$ docker run --help

Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]

Run a command in a new container

Options:
      --add-host list                         Add a custom host-to-IP mapping (host:ip) (default [])
  -a, --attach list                           Attach to STDIN, STDOUT or STDERR (default [])
      --blkio-weight uint16                   Block IO (relative weight), between 10 and 1000, or 0 to disable (default 0)
      --blkio-weight-device weighted-device   Block IO weight (relative device weight) (default [])
      --cap-add list                          Add Linux capabilities (default [])
      --cap-drop list                         Drop Linux capabilities (default [])
      --cgroup-parent string                  Optional parent cgroup for the container
      --cidfile string                        Write the container ID to the file
      --cpu-count int                         CPU count (Windows only)
      --cpu-percent int                       CPU percent (Windows only)
      --cpu-period int                        Limit CPU CFS (Completely Fair Scheduler) period
      --cpu-quota int                         Limit CPU CFS (Completely Fair Scheduler) quota
      --cpu-rt-period int                     Limit CPU real-time period in microseconds
      --cpu-rt-runtime int                    Limit CPU real-time runtime in microseconds
  -c, --cpu-shares int                        CPU shares (relative weight)
      --cpus decimal                          Number of CPUs (default 0.000)
      --cpuset-cpus string                    CPUs in which to allow execution (0-3, 0,1)
      --cpuset-mems string                    MEMs in which to allow execution (0-3, 0,1)
      --credentialspec string                 Credential spec for managed service account (Windows only)
  -d, --detach                                Run container in background and print container ID
      --detach-keys string                    Override the key sequence for detaching a container
      --device list                           Add a host device to the container (default [])
      --device-read-bps throttled-device      Limit read rate (bytes per second) from a device (default [])
      --device-read-iops throttled-device     Limit read rate (IO per second) from a device (default [])
      --device-write-bps throttled-device     Limit write rate (bytes per second) to a device (default [])
      --device-write-iops throttled-device    Limit write rate (IO per second) to a device (default [])
      --disable-content-trust                 Skip image verification (default true)
      --dns list                              Set custom DNS servers (default [])
      --dns-option list                       Set DNS options (default [])
      --dns-search list                       Set custom DNS search domains (default [])
      --entrypoint string                     Overwrite the default ENTRYPOINT of the image
  -e, --env list                              Set environment variables (default [])
      --env-file list                         Read in a file of environment variables (default [])
      --expose list                           Expose a port or a range of ports (default [])
      --group-add list                        Add additional groups to join (default [])
      --health-cmd string                     Command to run to check health
      --health-interval duration              Time between running the check (ns|us|ms|s|m|h) (default 0s)
      --health-retries int                    Consecutive failures needed to report unhealthy
      --health-timeout duration               Maximum time to allow one check to run (ns|us|ms|s|m|h) (default 0s)
      --help                                  Print usage
  -h, --hostname string                       Container host name
      --init                                  Run an init inside the container that forwards signals and reaps processes
      --init-path string                      Path to the docker-init binary
  -i, --interactive                           Keep STDIN open even if not attached
      --io-maxbandwidth string                Maximum IO bandwidth limit for the system drive (Windows only)
      --io-maxiops uint                       Maximum IOps limit for the system drive (Windows only)
      --ip string                             Container IPv4 address (e.g. 172.30.100.104)
      --ip6 string                            Container IPv6 address (e.g. 2001:db8::33)
      --ipc string                            IPC namespace to use
      --isolation string                      Container isolation technology
      --kernel-memory string                  Kernel memory limit
  -l, --label list                            Set meta data on a container (default [])
      --label-file list                       Read in a line delimited file of labels (default [])
      --link list                             Add link to another container (default [])
      --link-local-ip list                    Container IPv4/IPv6 link-local addresses (default [])
      --log-driver string                     Logging driver for the container
      --log-opt list                          Log driver options (default [])
      --mac-address string                    Container MAC address (e.g. 92:d0:c6:0a:29:33)
  -m, --memory string                         Memory limit
      --memory-reservation string             Memory soft limit
      --memory-swap string                    Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --memory-swappiness int                 Tune container memory swappiness (0 to 100) (default -1)
      --name string                           Assign a name to the container
      --network string                        Connect a container to a network (default "default")
      --network-alias list                    Add network-scoped alias for the container (default [])
      --no-healthcheck                        Disable any container-specified HEALTHCHECK
      --oom-kill-disable                      Disable OOM Killer
      --oom-score-adj int                     Tune host's OOM preferences (-1000 to 1000)
      --pid string                            PID namespace to use
      --pids-limit int                        Tune container pids limit (set -1 for unlimited)
      --privileged                            Give extended privileges to this container
  -p, --publish list                          Publish a container's port(s) to the host (default [])
  -P, --publish-all                           Publish all exposed ports to random ports
      --read-only                             Mount the container's root filesystem as read only
      --restart string                        Restart policy to apply when a container exits (default "no")
      --rm                                    Automatically remove the container when it exits
      --runtime string                        Runtime to use for this container
      --security-opt list                     Security Options (default [])
      --shm-size string                       Size of /dev/shm, default value is 64MB
      --sig-proxy                             Proxy received signals to the process (default true)
      --stop-signal string                    Signal to stop a container, SIGTERM by default (default "SIGTERM")
      --stop-timeout int                      Timeout (in seconds) to stop a container
      --storage-opt list                      Storage driver options for the container (default [])
      --sysctl map                            Sysctl options (default map[])
      --tmpfs list                            Mount a tmpfs directory (default [])
  -t, --tty                                   Allocate a pseudo-TTY
      --ulimit ulimit                         Ulimit options (default [])
  -u, --user string                           Username or UID (format: <name|uid>[:<group|gid>])
      --userns string                         User namespace to use
      --uts string                            UTS namespace to use
  -v, --volume list                           Bind mount a volume (default [])
      --volume-driver string                  Optional volume driver for the container
      --volumes-from list                     Mount volumes from the specified container(s) (default [])
  -w, --workdir string                        Working directory inside the container
```

That a we too much options to memorize, so always remember that `--help` option is one of our best friends in command line. If we prefer more detail we can always go for `man docker run` ;).

# Docker Ps

Lets see how we can list Docker Containers...

#### Let's see Usage:

```bash
docker ps --help | grep 'Usage:'
Usage:  docker ps [OPTIONS]
```

#### List Only Running Containers:

```bash
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

Whoops, but is correct because we are not running any container...


#### List Running and Inactive Containers:

```bash
docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                      PORTS               NAMES
55f528241902        alpine              "/bin/sh"           6 minutes ago       Exited (0) 6 minutes ago                        quizzical_leavitt
72899ca3311d        alpine              "/in/sh"            7 minutes ago       Created                                         peaceful_almeida
61fb540ee163        alpine              "ls -l"             14 minutes ago      Exited (0) 14 minutes ago                       heuristic_wilson
c712f4876c3e        hello-world         "/hello"            54 minutes ago      Exited (0) 54 minutes ago                       tender_edison
```

No running Containers, but we can see all stopped ones.

So the list has all Container we already run...


##### What means option `-a`

```bash
docker ps --help | grep '\-a,'
  -a, --all             Show all containers (default shows just running)
```

#### What else can we do with Docker Ps command:

```bash
docker ps --help

Usage:  docker ps [OPTIONS]

List containers

Options:
  -a, --all             Show all containers (default shows just running)
  -f, --filter filter   Filter output based on conditions provided
      --format string   Pretty-print containers using a Go template
      --help            Print usage
  -n, --last int        Show n last created containers (includes all states) (default -1)
  -l, --latest          Show the latest created container (includes all states)
      --no-trunc        Don't truncate output
  -q, --quiet           Only display numeric IDs
  -s, --size            Display total file sizes
```
# Docker Terminology

Let's learn some of the common terminology used by Docker.

#### Docker Images

They represent the file system and configuration to be used by Docker when creating the Containers for our applications.

#### Docker Containers

The Container is an instance of a Docker Image and is the one that will run our application with all their direct dependencies.

It runs as an isolated process in the Host machine, using the Host Kernel.

For security purposes it MUST be invoked always as the normal user, never as the root user of the Host machine.

#### Docker Daemon

It is a service running in the background of the Host machine, that manages building, running and distribute Docker Containers.


#### Docker Client

This is CLI tool, that allows us to talk with the Docker Daemon, like when we execute any Docker command `docker --help`

#### Docker Hub

A place where we can store all our Docker Images and from where we can pull them.

Is like a kind of Github, but for Docker Images.
