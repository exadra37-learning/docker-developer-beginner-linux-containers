# Web Apps With Docker

Following along my Docker training to chapter `2.0`, from https://training.docker.com.

This training session will show us how to use a *Container* to run a static website with a *Nginx* server.


# Run a Static Website in a Container

## Docker Run

So when we run a server like *Apache* or *Nginx* to serve our sites, we do it in the background, therefore a Container should be able to run also in the background.

#### Time for first call of the day to --help:

```bash
$ docker run --help | grep -i background
  -d, --detach                                Run container in background and print container ID
```

Cool, it seems we can also run *Containers* in the background or as *Docker* prefers, in `detach` mode.

#### Lets's see how:

```bash
$ docker run -d seqvence/static-site
Unable to find image 'seqvence/static-site:latest' locally
latest: Pulling from seqvence/static-site
fdd5d7827f33: Pull complete
a3ed95caeb02: Pull complete
716f7a5f3082: Pull complete
7b10f03a0309: Pull complete
aff3ab7e9c39: Pull complete
Digest: sha256:41b286105f913fb7a5fbdce28d48bc80f1c77e3c4ce1b8280f28129ae0e94e9e
Status: Downloaded newer image for seqvence/static-site:latest
2bbe68a3a4c92b3192d963d195fc382574c263e4485f5650a58838179167ff36
```

So what happened here... I have not used `docker pull <image-name>`, but it seems an image was downloaded!!!

*Docker* will pull for me any image I require when using `docker run`, that I don't have locally. So behind the scenes is running `docker pull` for me ;).

Wait but this *Container* is supposed to run a static website... how can I access it now???

If I now try [localhost](http://localhost) or [127.0.0.1](http://localhost) I can't see the static site... Ok it seems that I need to explicit tell what ports in the *Host* I want to bind to the *Container*.


#### Remember --help is our best friend:

```bash
$ docker run --help | grep -i port
      --expose list                           Expose a port or a range of ports (default [])
      --health-retries int                    Consecutive failures needed to report unhealthy
  -p, --publish list                          Publish a container's port(s) to the host (default [])
  -P, --publish-all                           Publish all exposed ports to random ports
```

Hey option `-P` seems to be the most automated one...

#### So let's try again:

```bash
$ docker run -P -d seqvence/static-site
560fd95b594d607d2ae64df6d912e84c3ec9b1fcbd08b368e46c9f5ca85eab1c
```

Hummm... an hash is not that useful to tell me what ports in Host were assigned to the Container, unless we have some command to see that!!!

## Docker Port

So let's see what *Docker* help can do for us...

```bash
$ docker --help | grep -i port
  export      Export a container's filesystem as a tar archive
  import      Import the contents from a tarball to create a filesystem image
  port        List port mappings or a specific mapping for the container
```

Hey it seems we have a command for that `docker port`...

```bash
$ docker port --help

Usage:  docker port CONTAINER [PRIVATE_PORT[/PROTO]]

List port mappings or a specific mapping for the container

Options:
      --help   Print usage
```

It seems pretty straight forward to use, so let's give it a try:

```bash
$ docker port 560fd95b594d607d2ae64df6d912e84c3ec9b1fcbd08b368e46c9f5ca85eab1c
443/tcp -> 0.0.0.0:32769
80/tcp -> 0.0.0.0:32770
```

Bingo... I have now my ports, so let's try again to visit [localhost:32770](http://localhost:32770) or [127.0.0.1:32770](http://localhost:32770).

Eureka it works :)

## Docker Run - Advanced Usage

### Specify Container Name

Using the Container Id hash to identify the *Container* in other *Docker Command*, can become a little cumbersome, but gladly we can also use the *Container* name.

So each time we create a *Container* with `docker run` a random name for it is generated, as we can check in the below output of `docker ps` at the last column `NAMES`.

```bash
$ docker ps
[sudo] password for exadra7:
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                                           NAMES
560fd95b594d        seqvence/static-site   "/bin/sh -c 'cd /u..."   5 hours ago         Up 5 hours          0.0.0.0:32770->80/tcp, 0.0.0.0:32769->443/tcp   upbeat_wozniak
9bd88528f950        seqvence/static-site   "/bin/sh -c 'cd /u..."   5 hours ago         Up 5 hours          80/tcp, 443/tcp                                 vibrant_lewin
```

Is easier to write a name like `upbeat_wozniak` than a hash like `560fd95b594d`, but still not the ideal solution, once we still need to resort to `docker ps` command to know the *Container* name.

So let's check if *Docker* allow us to specify the *Container* name at creation time:

```bash
$ docker run --help | grep -i name
[sudo] password for exadra7:
  -h, --hostname string                       Container host name
      --ipc string                            IPC namespace to use
      --name string                           Assign a name to the container
      --pid string                            PID namespace to use
  -u, --user string                           Username or UID (format: <name|uid>[:<group|gid>])
      --userns string                         User namespace to use
      --uts string                            UTS namespace to use
```

Nice, they have the option `--name` to cover our needs, so let's give it a try:

```bash
$  docker run --name static-site -P -d seqvence/static-site
c3df3fb3b30d8670213fd0695e229d3ea218acd7c77412641ddeb7a50e6cf17f
```

It works... Let's check the *Container* ports using this time the name instead of the hash:

```bash
$ docker port static-site
443/tcp -> 0.0.0.0:32771
80/tcp -> 0.0.0.0:32772
```

Isn't this so cool... I can now personalize each created *Container* with a memorable name :).


### Pass Environment Variables To The Container

Inside a *Container* may exist environment variables that we would like to set or override at creation time... is that possible at all???

```bash
$ docker run --help | grep -i environment
  -e, --env list                              Set environment variables (default [])
      --env-file list                         Read in a file of environment variables (default [])
```

WOW, is not this amazing!!!

*Docker* as us covered once more...

So when we visit the static site a `Hello Docker` greeting is displayed, where the word `Docker` comes from a environment variable `AUTHOR`.

```
Hello Docker!
This is being served from a docker
container running Nginx.
```

Let's gonna try to set it when we create the *Container*:

```bash
$ docker run --name set-author -e AUTHOR="Exadra37" -P -d seqvence/static-site
aac6bb42ceb8fa5a464f53d9f298f16813571cfbccb121222c0e3a23bbf34839
```

Checking the ports:

```bash
$ docker port set-author
443/tcp -> 0.0.0.0:32773
80/tcp -> 0.0.0.0:32774
```

So if I visit localhost on port `32774` I get my personalized greeting:

```
Hello Exadra37!
This is being served from a docker
container running Nginx.
```

This rocks!!!


### Specify Host Ports to Bind into the Container

Each time we create a *Container* we need to use `docker port` command to check the assigned ports  in the *Host*, but we must be able to specify the exact port number we want to map.

```bash
$ docker run --help | grep -i port
      --expose list                           Expose a port or a range of ports (default [])
      --health-retries int                    Consecutive failures needed to report unhealthy
  -p, --publish list                          Publish a container's port(s) to the host (default [])
  -P, --publish-all                           Publish all exposed ports to random ports
```

Until now we have use option `-P`, but let's try the lower case version `-p`:

```bash
$ docker run --name specific-port -p 8000:80 -d seqvence/static-site
dd4a2c43cd6c3659706695e835fb287833ce44dbcfa4102496678baa60fa492ai
```

So if we now visit in [localhost:8000](http://localhost:8000) we can see that it works fine and we don't need any more to use `docker port` command.

>**NOTE**:
>
> Using a specific port number, makes our responsibility to ensure that we don't use a port already assigned in the Host.
> If we do so *Docker* will throw an error:
> ```bash
> $ docker run -p 8000:80 -d seqvence/static-site
> 58deaf580f752a2beb1202ba03d8302ec5f572880806b84cad5a2281a1ff4f2f
> docker: Error response from daemon: driver failed programming external connectivity on endpoint > dazzling_raman (d115e117b458d1a286b9aef6e6d26fa479043fa6e4a534b88b55e0fb668c7f86): Bind for > 0.0.0.0:8000 failed: port is
> already allocated.
> ```


### Run Several Web Servers at Same Time

You may have not realized yet, but we already achieved this... what???

Let's take a look to our running *Containers*:

```bash
$ docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                                           NAMES
dd4a2c43cd6c        seqvence/static-site   "/bin/sh -c 'cd /u..."   17 minutes ago      Up 17 minutes       443/tcp, 0.0.0.0:8000->80/tcp                   specific-port
aac6bb42ceb8        seqvence/static-site   "/bin/sh -c 'cd /u..."   36 minutes ago      Up 36 minutes       0.0.0.0:32774->80/tcp, 0.0.0.0:32773->443/tcp   set-author
c3df3fb3b30d        seqvence/static-site   "/bin/sh -c 'cd /u..."   54 minutes ago      Up 54 minutes       0.0.0.0:32772->80/tcp, 0.0.0.0:32771->443/tcp   static-site
560fd95b594d        seqvence/static-site   "/bin/sh -c 'cd /u..."   6 hours ago         Up 6 hours          0.0.0.0:32770->80/tcp, 0.0.0.0:32769->443/tcp   upbeat_wozniak
```

By taking a look to the column `PORTS`, in my case I can see that I have *Web Servers* running in ports `8000`, `32774`, `32772` and `32770`. So this is all *Containers* I have created during this training session.

If I visit `http://localhost:port` for each one I can easily confirm that I am already running several *Web Servers* from same *Docker Image*.

Oh man, this is like *Servers on Steroids* ;) 

Why not firing another one...

```bash
$ docker run --name web-servers -e AUTHOR="Web Servers on Steroids" -p 8001:80 -d seqvence/static-site
37334f0ab883434e52ef34437025d00fe0cb1e1d7e68b14e5250c37e6d6b4370
```

If we now visit [localhost:8001](http://localhost:8001) we see:

```
Hello Web Servers on Steroids!
This is being served from a docker
container running Nginx.
```

## Docker Stop 

In previous topics of this training session we have created several *Containers* that are still running, but we must have a way to terminate them...

```bash
$ docker --help | grep -i stop
  start       Start one or more stopped containers
  stop        Stop one or more running containers
  wait        Block until one or more containers stop, then print their exit codes
```

The *Docker* command to use seems to be `docker stop`, so now how I just need to know how to use it...

```bash
$ docker stop --help

Usage:  docker stop [OPTIONS] CONTAINER [CONTAINER...]

Stop one or more running containers

Options:
      --help       Print usage
  -t, --time int   Seconds to wait for stop before killing it (default 10)
```

Check what *Containers* are running:

```bash 
$ docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                                           NAMES
37334f0ab883        seqvence/static-site   "/bin/sh -c 'cd /u..."   7 minutes ago       Up 7 minutes        443/tcp, 0.0.0.0:8001->80/tcp                   web-servers
dd4a2c43cd6c        seqvence/static-site   "/bin/sh -c 'cd /u..."   37 minutes ago      Up 37 minutes       443/tcp, 0.0.0.0:8000->80/tcp                   specific-port
aac6bb42ceb8        seqvence/static-site   "/bin/sh -c 'cd /u..."   56 minutes ago      Up 56 minutes       0.0.0.0:32774->80/tcp, 0.0.0.0:32773->443/tcp   set-author
c3df3fb3b30d        seqvence/static-site   "/bin/sh -c 'cd /u..."   About an hour ago   Up About an hour    0.0.0.0:32772->80/tcp, 0.0.0.0:32771->443/tcp   static-site
9bd88528f950        seqvence/static-site   "/bin/sh -c 'cd /u..."   7 hours ago         Up 7 hours          80/tcp, 443/tcp                                 vibrant_lewin
```

So let's grab the first *Container* in the above list, `web-servers`, as is named in columns `NAMES`.

Now we can stop it:

```bash
$ docker stop web-servers
web-servers
```

Checking running *Containers* again:

```bash
$ docker ps
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                                           NAMES
dd4a2c43cd6c        seqvence/static-site   "/bin/sh -c 'cd /u..."   40 minutes ago      Up 40 minutes       443/tcp, 0.0.0.0:8000->80/tcp                   specific-port
aac6bb42ceb8        seqvence/static-site   "/bin/sh -c 'cd /u..."   59 minutes ago      Up 59 minutes       0.0.0.0:32774->80/tcp, 0.0.0.0:32773->443/tcp   set-author
c3df3fb3b30d        seqvence/static-site   "/bin/sh -c 'cd /u..."   About an hour ago   Up About an hour    0.0.0.0:32772->80/tcp, 0.0.0.0:32771->443/tcp   static-site
560fd95b594d        seqvence/static-site   "/bin/sh -c 'cd /u..."   6 hours ago         Up 6 hours          0.0.0.0:32770->80/tcp, 0.0.0.0:32769->443/tcp   upbeat_wozniak
```

As we can see the `web-servers` *Container* is not any more in the list.


## Docker Rm

We can stop *Containers* with `docker stop` command, but should be also possible to completely remove them... Do you wan bet that `docker --help` has the solution for us???

```bash
$ docker --help | grep -i remove
  rm          Remove one or more containers
  rmi         Remove one or more images
```
Aha, there it is `docker rm` command... Sorry dude but if you have bet against, than you have lost :).

Time to check all existing *Containers*:

```bash
$ docker ps -a
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS                       PORTS                                           NAMES
37334f0ab883        seqvence/static-site   "/bin/sh -c 'cd /u..."   18 minutes ago      Exited (137) 8 minutes ago                                                   web-servers
58deaf580f75        seqvence/static-site   "/bin/sh -c 'cd /u..."   43 minutes ago      Created                                                                      dazzling_raman
dd4a2c43cd6c        seqvence/static-site   "/bin/sh -c 'cd /u..."   47 minutes ago      Up 47 minutes                443/tcp, 0.0.0.0:8000->80/tcp                   specific-port
aac6bb42ceb8        seqvence/static-site   "/bin/sh -c 'cd /u..."   About an hour ago   Up About an hour             0.0.0.0:32774->80/tcp, 0.0.0.0:32773->443/tcp   set-author
c3df3fb3b30d        seqvence/static-site   "/bin/sh -c 'cd /u..."   About an hour ago   Up About an hour             0.0.0.0:32772->80/tcp, 0.0.0.0:32771->443/tcp   static-site
560fd95b594d        seqvence/static-site   "/bin/sh -c 'cd /u..."   7 hours ago         Up 7 hours                   0.0.0.0:32770->80/tcp, 0.0.0.0:32769->443/tcp   upbeat_wozniak
```

The `web-servers` *Container* is back to the list, but if take a look to the column `STATUS` we can see that is stopped, once status is `Exited ...`.

Let's remove it permanently:

```bash
$ docker rm web-servers
web-servers
```

Checking again all existing *Containers*:

```bash
$ docker ps -a
CONTAINER ID        IMAGE                  COMMAND                  CREATED             STATUS              PORTS                                           NAMES
58deaf580f75        seqvence/static-site   "/bin/sh -c 'cd /u..."   51 minutes ago      Created                                                             dazzling_raman
dd4a2c43cd6c        seqvence/static-site   "/bin/sh -c 'cd /u..."   56 minutes ago      Up 56 minutes       443/tcp, 0.0.0.0:8000->80/tcp                   specific-port
aac6bb42ceb8        seqvence/static-site   "/bin/sh -c 'cd /u..."   About an hour ago   Up About an hour    0.0.0.0:32774->80/tcp, 0.0.0.0:32773->443/tcp   set-author
c3df3fb3b30d        seqvence/static-site   "/bin/sh -c 'cd /u..."   About an hour ago   Up About an hour    0.0.0.0:32772->80/tcp, 0.0.0.0:32771->443/tcp   static-site
560fd95b594d        seqvence/static-site   "/bin/sh -c 'cd /u..."   7 hours ago         Up 7 hours          0.0.0.0:32770->80/tcp, 0.0.0.0:32769->443/tcp   upbeat_wozniak
```

As we can see the *Container* we removed, `web-servers`, is not any more in the list.

So each time I want to remove a *Container* I need to first stop it!!!

I am too lazy, so I believe *Docker* has something in the sleeve to help me...

```bash
$ docker rm --help

Usage:  docker rm [OPTIONS] CONTAINER [CONTAINER...]

Remove one or more containers

Options:
  -f, --force     Force the removal of a running container (uses SIGKILL)
      --help      Print usage
  -l, --link      Remove the specified link
  -v, --volumes   Remove the volumes associated with the container
```

Jackpot, there it is the option `-f`...

From the last list of existing *Containers* let's pick up a running one to force removal...

```bash
$ docker rm -f dazzling_raman
dazzling_raman
```

Hey, once more *Docker* is making our life easier ;)
