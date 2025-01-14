# shellbro/dropbox

[![](https://img.shields.io/docker/cloud/build/shellbro/dropbox)](https://hub.docker.com/r/shellbro/dropbox/)

Dropbox in a (Docker) container.

Running Dropbox in a container is a solution for running Dropbox on older Linux
systems. At the end of 2018 Dropbox team dropped support for Linux operating
systems that use `glibc < 2.19` and this affects CentOS 7 which ships with
`glibc 2.17`. This container image is based on CentOS 8 which runs with newer
`glibc 2.28`.

# Docker Hub

This image uses automated build service offered by Docker Hub.

https://hub.docker.com/r/shellbro/dropbox/

# Quick start (store files inside a container)

```
docker run --name=dropbox -d --log-driver=journald --restart=always shellbro/dropbox
```

After container is created click on the URL from `docker logs dropbox` to link
your Dropbox account.

# Quick start (store files on the host)

Note: replace `/home/shellbro/Dropbox` example path with the host path you would
like to use.

```
docker run --name=dropbox -d -v /home/shellbro/Dropbox:/home/dropbox-user/Dropbox --log-driver=journald --restart=always shellbro/dropbox
```

# Check Dropbox status

```
docker exec -ti -e "LANG=en_US.UTF-8" dropbox /home/dropbox-user/bin/dropbox status
```

You might want to put the following shell alias in your `~/.bashrc` file:

```
alias dropbox='docker exec -ti -e "LANG=en_US.UTF-8" dropbox /home/dropbox-user/bin/dropbox'
```

and simply use it like:

```
dropbox status
```

# Manual update

Shortly after each new version of Dropbox is released I update this image
utilizing the latest Dropbox version available. To update your setup just run:

```
docker stop dropbox
docker rm dropbox
docker pull shellbro/dropbox
```

and start your container from the new image like in the `Quick start` sections.

# Auto-update issue

Unfortunately, it happens that Dropbox tries to update itself inside
a container after it detects that update is available but that process fails
when running inside a container. You can check that this is the case by running
`dropbox status` or `docker logs dropbox` command. The only workaround I know
about at the moment is to follow the procedure of manual Dropbox update from the
section above after each Dropbox release.
