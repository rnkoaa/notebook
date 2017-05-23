# On Linux servers, edit `/etc/default/docker` and add the following lines
```sh
DOCKER_OPTS="--dns 8.8.8.8 --dns 192.168.1.1 --insecure-registry rfstaging.asuscomm.com:8082 --insecure-registry rfstaging.asuscomm.com:8083"
```
docker login -u rnkoaa -p Admin2193! http://rfstaging.asuscomm.com:8082
docker login -u rnkoaa -p Admin2193! http://rfstaging.asuscomm.com:8083


docker pull rfstaging.asuscomm.com:8082/rnkoaa/mongo-express

## raspberry pi fix for docker insecure registries
[Source](http://developmentalmadness.com/2016/03/09/docker-configure-insecure-registry-for-systemd/)

Create systemd Override Files
If the path `/etc/systemd/system/docker.service.d/docker.conf` already exists just open the docker.conf file and skip to the next section. If it doesn’t exist, create the directory `etc/systemd/system/docker.service.d` directory and `docker.conf` file:

```sh
$ sudo mkdir /etc/systemd/system/docker.service.d
$ sudo touch /etc/systemd/system/docker.service.d/docker.conf
$ sudo vi /etc/systemd/system/docker.service.d/docker.conf
```

Then add the following contents to docker.conf:

```sh
$ sudo vi /etc/systemd/system/docker.service.d/docker.conf
[Service]
ExecStart=
ExecStart=/usr/bin/docker daemon -H fd://
```

Configure insecure-registry
Append the --insecure-registry option to the end of the ExecStart options so it looks something like this: (multiple entries follow array convention. See docs)

```sh
ExecStart=/usr/bin/docker daemon -H fd:// --insecure-registry myregistry.mydomain.com
```

Just make sure you replace myregistry.mydoamin.com with the url (and optionally the port if you’re not using port 80) for your registry.

Save the file, then flush changes and restart:

```sh
$ sudo systemctl daemon-reload
$ sudo systemctl restart docker
```

Verify docker daemon is running

```sh
$ ps aux | grep docker | grep -v grep
```