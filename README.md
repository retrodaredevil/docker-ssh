# docker-ssh

This repository contains `autossh` and `sshd` docker containers.
These docker images aim to be extremely minimal and are meant to be just the commands themselves:
No custom configuration with environment variables you've never heard of!

This is work in progress.

## Setting up a server

```shell
cd /opt/containers/
mkdir ssh-forward-server
cd ssh-forward-server/
mkdir ssh
edit ssh/sshd_config
ssh-keygen -A -f .
mv etc/ssh/* ssh
rm -r etc
docker run -p 2222:22 -v "./authorized_keys:/home/ssh/.ssh/authorized_keys:ro" -v "./ssh:/etc/ssh:ro" ghcr.io/retrodaredevil/sshd
```

Or instead of `docker run`:

```yaml

version: '3.7'

services:
  ssh-forward-server:
    image: 'ghcr.io/retrodaredevil/sshd:latest'
    restart: unless-stopped
    ports:
      - "2222:22"
    volumes:
      - './authorized_keys:/home/ssh/.ssh/authorized_keys:ro'
      - './ssh:/etc/ssh:ro'
```

## Setting up a client using `autossh`

This example will show you



## Testing `sshd`

```shell

mkdir {autossh,sshd}/test
cd sshd/test
mkdir ssh
cat /etc/ssh/sshd_config | grep -v '^#' | grep -v '^$' > ssh/sshd_config
mkdir -p etc/ssh
ssh-keygen -A -f .
mv etc/ssh/* ssh
rm -r etc
sudo docker run --rm -p 8022:22 -v "./authorized_keys:/home/ssh/.ssh/authorized_keys:ro" -v "./ssh:/etc/ssh:ro" -it $(sudo docker build -q ..)
```

## Testing `autossh`

```shell
```

## To-do
* Upload to different central registries
  * https://quay.io/
  * https://hub.docker.com/