# Docker_Volumes_and_Bind_Mounts
### Bind Mount, persistent data, allows you to attach an existing directory on your host to a directory inside of a container. This is what is used when you are trying to map the files from a directory on the host into a directory in the container.

### When making a new volume for a mysql container, you can look Docker Hub to see where the data path should be located in the container. Looking through the README.md or Dockerfile of the mysql official image, you could find the database path documented or the VOLUME stanza.

![volumes_and_bind_mounts](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/volumes_and_bind_mounts.png)

## You will find VOLUME in official mysql dockerfile
### because this is database, there is probably VOLUME command in dockerfile
```
VOLUME /var/lib/mysql

# this is default location of mysql databases
# This image is programmed telling docker when we start new container from it, it actually creates a new volume location
# and assign it to this directory in the container
# if we put any files in the container, will outlive the container until we manually delete the volume
# volume needs manyal deletion
```
## let's pull mysql and do docker image inspect on mysql
## in config area, there is volume
## it always tell the config that came from dockerfile when it was built, assign the volume to that path
```
Koitaro@MacBook-Pro-3 ~ % docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
afb6ec6fdc1c: Pull complete
0bdc5971ba40: Pull complete
97ae94a2c729: Pull complete
f777521d340e: Pull complete
1393ff7fc871: Pull complete
a499b89994d9: Pull complete
7ebe8eefbafe: Pull complete
597069368ef1: Pull complete
ce39a5501878: Pull complete
7d545bca14bf: Pull complete
211e5bb2ae7b: Pull complete
5914e537c077: Pull complete
Digest: sha256:a31a277d8d39450220c722c1302a345c84206e7fd4cdb619e7face046e89031d
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest

Koitaro@MacBook-Pro-3 ~ % docker image ls
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
mysql               latest              30f937e841c8        41 hours ago        541MB

Koitaro@MacBook-Pro-3 ~ % docker image inspect mysql
[
    {
        "Id": "sha256:30f937e841c82981a9a6363f7f6f35ed6b9d5e3f16df50a72207e4a2a389983f",
        "RepoTags": [
            "mysql:latest"
        ],
        "RepoDigests": [
            "mysql@sha256:a31a277d8d39450220c722c1302a345c84206e7fd4cdb619e7face046e89031d"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2020-05-21T05:20:15.819938557Z",
        "Container": "c677657b0e93cf74176a487f768a475d7d55fad848cda5ede0ab2e0ee1b0508f",
        "ContainerConfig": {
            "Hostname": "c677657b0e93",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {},
                "33060/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "MYSQL_MAJOR=8.0",
                "MYSQL_VERSION=8.0.20-1debian10"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"mysqld\"]"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:97bb1a5092d9371e7556da12b9af0fe592bffe0a78fa4021eb24827dd4ea839e",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "18.09.7",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {},
                "33060/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "MYSQL_MAJOR=8.0",
                "MYSQL_VERSION=8.0.20-1debian10"
            ],
            "Cmd": [
                "mysqld"
            ],
            "ArgsEscaped": true,
            "Image": "sha256:97bb1a5092d9371e7556da12b9af0fe592bffe0a78fa4021eb24827dd4ea839e",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 540571102,
        "VirtualSize": 540571102,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/9430667dccfd4e70409f9ec3168b1ebf094891b1ab76440cf5120eed260132a3/diff:/var/lib/docker/overlay2/30256a6f63ac5ffafbfe200d229b7a9dabe7d5f066c21f06c80bc6b3d7829f41/diff:/var/lib/docker/overlay2/8f50c01b098657c0c361bb0e5fe92920727742834fe55103d8d0d6ffafab44d9/diff:/var/lib/docker/overlay2/38d2f94eec35abfce03ff5ac885849b983ff289ae72e2573a46fc8b64eacab21/diff:/var/lib/docker/overlay2/479dce39ff195ef78dee02afc86d2c769937fd1a5e7e34dd42d70640cebf458b/diff:/var/lib/docker/overlay2/4a0142e2bc38d8a59118bebd094d722780b6e1cddb235b947edab2603264ec38/diff:/var/lib/docker/overlay2/6fea2a942777b35828326368a62e77a4a19b21350f87974b2e82d4bf4908e97a/diff:/var/lib/docker/overlay2/7f9ea96fe5ca85d7e2ea05c0e90e39f1d2e3326552c0321eb4ea8c461800636b/diff:/var/lib/docker/overlay2/c764c20506e960aa27f2eb4739de3f3e2b769728bb7079a1080db9d617f4f09d/diff:/var/lib/docker/overlay2/afdbf601dc6acb70e55588b6f041a3b69815b33390112cfee3701c5f75a6680f/diff:/var/lib/docker/overlay2/5a6df8134bd0d038770a8a38598ea187ac92b70549523b5319f4a43e5c5c3a5b/diff",
                "MergedDir": "/var/lib/docker/overlay2/caefe79a655eaee3fbbb8a3b87b97b1994a8584e73b3b8fa32c2edcdfa5e2d34/merged",
                "UpperDir": "/var/lib/docker/overlay2/caefe79a655eaee3fbbb8a3b87b97b1994a8584e73b3b8fa32c2edcdfa5e2d34/diff",
                "WorkDir": "/var/lib/docker/overlay2/caefe79a655eaee3fbbb8a3b87b97b1994a8584e73b3b8fa32c2edcdfa5e2d34/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:ffc9b21953f4cd7956cdf532a5db04ff0a2daa7475ad796f1bad58cfbaf77a07",
                "sha256:6584e03fcdb77a0e2a7b100322f1cb71b3804bd9ff21ed37ea5ddbeda1288483",
                "sha256:f1705d081ae4e38d30d891cdfc47e8859d490902fecd1f3ded8f097ee68f18f1",
                "sha256:0cff2de5c75d40b5cfca8a963e27f4f7beaa1b8cd37eb8bd9d9c333331396052",
                "sha256:1307812365579e17bfc12de65501c5b167737acfe0f90e2f5f12f2aaecbcb216",
                "sha256:c590eb856ed14297e616631157c0ed4f4a95d071c44b06b270c77f89d5aeacea",
                "sha256:e204fb6af7efcde9a407fda27bc60b11a3047480a64f1c2ec31f3dccf35d9645",
                "sha256:fc7148dca28ed6584ed76082cad6f3e138069e0a299e5e7fee6bbb9366dba88d",
                "sha256:a198a024259b21cd17a018964b9746d7b384aefa824c980140c7119e14212b91",
                "sha256:146a9278d64794ac15ab98e5515b0ac2aa08131312115301e1fae67cc0ca6ccb",
                "sha256:da398237c13fdc256c21f2199386835592b61989f312046c3fa74732f33df0b1",
                "sha256:bd65f855e22883c977b87b0f2977dc089f16c285f8e884021c82d634a9d41628"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]

```
## Next, let's run a container from it
## Note that mysql requires environment variable
## the last mysql is the image we are using
## you can see the volume in Config and Mounts when you inspect the container
## Note: running container getting its own unique location on the host to store that data. In the background, mounted to that locaion in the container
## location of the container is "Destination": "/var/lib/mysql". 
## but the data actually located in "Source": "/var/lib/docker/volumes/96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232/_data", in the host
```
Koitaro@MacBook-Pro-3 ~ % docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql
21e3d2e48e6ceac44d29b3c016805237b03e36fddf571a01b9f6c15d95572a49

# check if mysql is running
Koitaro@MacBook-Pro-3 ~ % docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
21e3d2e48e6c        mysql               "docker-entrypoint.s…"   55 seconds ago      Up 54 seconds       3306/tcp, 33060/tcp   mysql

Koitaro@MacBook-Pro-3 ~ % docker container inspect mysql
[
    {
        "Id": "21e3d2e48e6ceac44d29b3c016805237b03e36fddf571a01b9f6c15d95572a49",
        "Created": "2020-05-22T22:04:33.978461858Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "mysqld"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 1833,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-05-22T22:04:34.417218898Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:30f937e841c82981a9a6363f7f6f35ed6b9d5e3f16df50a72207e4a2a389983f",
        "ResolvConfPath": "/var/lib/docker/containers/21e3d2e48e6ceac44d29b3c016805237b03e36fddf571a01b9f6c15d95572a49/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/21e3d2e48e6ceac44d29b3c016805237b03e36fddf571a01b9f6c15d95572a49/hostname",
        "HostsPath": "/var/lib/docker/containers/21e3d2e48e6ceac44d29b3c016805237b03e36fddf571a01b9f6c15d95572a49/hosts",
        "LogPath": "/var/lib/docker/containers/21e3d2e48e6ceac44d29b3c016805237b03e36fddf571a01b9f6c15d95572a49/21e3d2e48e6ceac44d29b3c016805237b03e36fddf571a01b9f6c15d95572a49-json.log",
        "Name": "/mysql",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/45c5b176d52c009cd9be3c8be00471d2fb621900f38dc09943387f675c56bd28-init/diff:/var/lib/docker/overlay2/caefe79a655eaee3fbbb8a3b87b97b1994a8584e73b3b8fa32c2edcdfa5e2d34/diff:/var/lib/docker/overlay2/9430667dccfd4e70409f9ec3168b1ebf094891b1ab76440cf5120eed260132a3/diff:/var/lib/docker/overlay2/30256a6f63ac5ffafbfe200d229b7a9dabe7d5f066c21f06c80bc6b3d7829f41/diff:/var/lib/docker/overlay2/8f50c01b098657c0c361bb0e5fe92920727742834fe55103d8d0d6ffafab44d9/diff:/var/lib/docker/overlay2/38d2f94eec35abfce03ff5ac885849b983ff289ae72e2573a46fc8b64eacab21/diff:/var/lib/docker/overlay2/479dce39ff195ef78dee02afc86d2c769937fd1a5e7e34dd42d70640cebf458b/diff:/var/lib/docker/overlay2/4a0142e2bc38d8a59118bebd094d722780b6e1cddb235b947edab2603264ec38/diff:/var/lib/docker/overlay2/6fea2a942777b35828326368a62e77a4a19b21350f87974b2e82d4bf4908e97a/diff:/var/lib/docker/overlay2/7f9ea96fe5ca85d7e2ea05c0e90e39f1d2e3326552c0321eb4ea8c461800636b/diff:/var/lib/docker/overlay2/c764c20506e960aa27f2eb4739de3f3e2b769728bb7079a1080db9d617f4f09d/diff:/var/lib/docker/overlay2/afdbf601dc6acb70e55588b6f041a3b69815b33390112cfee3701c5f75a6680f/diff:/var/lib/docker/overlay2/5a6df8134bd0d038770a8a38598ea187ac92b70549523b5319f4a43e5c5c3a5b/diff",
                "MergedDir": "/var/lib/docker/overlay2/45c5b176d52c009cd9be3c8be00471d2fb621900f38dc09943387f675c56bd28/merged",
                "UpperDir": "/var/lib/docker/overlay2/45c5b176d52c009cd9be3c8be00471d2fb621900f38dc09943387f675c56bd28/diff",
                "WorkDir": "/var/lib/docker/overlay2/45c5b176d52c009cd9be3c8be00471d2fb621900f38dc09943387f675c56bd28/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [
            {
                "Type": "volume",
                "Name": "96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232",
                "Source": "/var/lib/docker/volumes/96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "21e3d2e48e6c",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {},
                "33060/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "MYSQL_ALLOW_EMPTY_PASSWORD=True",
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "MYSQL_MAJOR=8.0",
                "MYSQL_VERSION=8.0.20-1debian10"
            ],
            "Cmd": [
                "mysqld"
            ],
            "Image": "mysql",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "1940550fb31e8c08d1ea44ea4e3d3ef73ceac4ef9f9696057b92e8bbd911a30a",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "3306/tcp": null,
                "33060/tcp": null
            },
            "SandboxKey": "/var/run/docker/netns/1940550fb31e",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "471ff5eaa9ec4a785ed8c9c310e1a8117ee73c1f4ee542f8aba083c87bcc8508",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "fe85293b21abf564881227697d4f2e411a069e9e9cd9e80b955c1a6791bb525c",
                    "EndpointID": "471ff5eaa9ec4a785ed8c9c310e1a8117ee73c1f4ee542f8aba083c87bcc8508",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

## Let' inspect the volume
```
Koitaro@MacBook-Pro-3 ~ % docker volume ls
DRIVER              VOLUME NAME
local               96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232

Koitaro@MacBook-Pro-3 ~ % docker volume inspect 96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232
[
    {
        "CreatedAt": "2020-05-22T22:04:51Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232/_data",
        "Name": "96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232",
        "Options": null,
        "Scope": "local"
    }
]

```

## Let's create another mysql server running in the background
## nwo two of them running
## The problem is that there is no easy way to tell one from the other
```
Koitaro@MacBook-Pro-3 ~ % docker container run -d --name mysql2 -e MYSQL_ALLOW_EMPTY_PASSWORD=True mysql
789bb72034f4cfaa9eb9e6343334ca21cb9ecb703cca4e95ff02d849f8abc003

Koitaro@MacBook-Pro-3 ~ % docker volume ls
DRIVER              VOLUME NAME
local               80a9a7958e00c38cbef0d371c96bf480cda1434016952fcb02d9e29dc597ec9c
local               96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232
```
## Now stop both containers
## no container running now
```
Koitaro@MacBook-Pro-3 ~ % docker container stop mysql
mysql

Koitaro@MacBook-Pro-3 ~ % docker container stop mysql2
mysql2

Koitaro@MacBook-Pro-3 ~ % docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES

Koitaro@MacBook-Pro-3 ~ % docker container ls -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                      PORTS               NAMES
789bb72034f4        mysql               "docker-entrypoint.s…"   2 minutes ago       Exited (0) 17 seconds ago                       mysql2
21e3d2e48e6c        mysql               "docker-entrypoint.s…"   17 minutes ago      Exited (0) 26 seconds ago                       mysql
```
## Let's prove that databases outlive the executable
## After deleting containers by rm, volumes are still there!
## my data is still safe

```
Koitaro@MacBook-Pro-3 ~ % docker volume ls
DRIVER              VOLUME NAME
local               80a9a7958e00c38cbef0d371c96bf480cda1434016952fcb02d9e29dc597ec9c
local               96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232

Koitaro@MacBook-Pro-3 ~ % docker container rm mysql mysql2
mysql
mysql2

Koitaro@MacBook-Pro-3 ~ % docker volume ls
DRIVER              VOLUME NAME
local               80a9a7958e00c38cbef0d371c96bf480cda1434016952fcb02d9e29dc597ec9c
local               96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232

```
## To make it more user-friendly, ther is named-volumes
## ability for us to specify things on the docker run command by -v command
## -v command allows us to specify either new volume that we wanna create for the container that about to run
## Or, -v command allows us to create a named-volume
## docker container run -d --name mysql2 -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v /var/lib/mysql mysql (this is what VOLUME command in the dockerfile did, but we name it with ':')
## Now, my new container is using new volume and it's using a friendly name
```
Koitaro@MacBook-Pro-3 ~ % docker container run -d --name mysql2 -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql
177efaf90eabcc046c7f9408919caf786b8d3899ab656ef19895a18689881cd9

Koitaro@MacBook-Pro-3 ~ % docker volume ls
DRIVER              VOLUME NAME
local               80a9a7958e00c38cbef0d371c96bf480cda1434016952fcb02d9e29dc597ec9c
local               96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232
local               mysql-db

Koitaro@MacBook-Pro-3 ~ % docker volume inspect mysql-db
[
    {
        "CreatedAt": "2020-05-22T22:36:56Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/mysql-db/_data",
        "Name": "mysql-db",
        "Options": null,
        "Scope": "local"
    }
]
```
## Let's delete the container, and run another container with already created named-volume
```
Koitaro@MacBook-Pro-3 ~ % docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                 NAMES
177efaf90eab        mysql               "docker-entrypoint.s…"   16 minutes ago      Up 16 minutes       3306/tcp, 33060/tcp   mysql2

Koitaro@MacBook-Pro-3 ~ % docker container rm -f mysql2
mysql2

Koitaro@MacBook-Pro-3 ~ % docker volume ls
DRIVER              VOLUME NAME
local               80a9a7958e00c38cbef0d371c96bf480cda1434016952fcb02d9e29dc597ec9c
local               96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232
local               mysql-db

Koitaro@MacBook-Pro-3 ~ % docker container run -d --name mysql3 -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql
8717f7966a324d6f52104a4cc27d2e4640cca61a4787be33ced5d7e38e71463f

# we haven't created a new volume, same as above, still using the same volume
Koitaro@MacBook-Pro-3 ~ % docker volume ls
DRIVER              VOLUME NAME
local               80a9a7958e00c38cbef0d371c96bf480cda1434016952fcb02d9e29dc597ec9c
local               96a30233b5d53116d77a7f65cdb112a7ed04821b1c581214eb4867e68d790232
local               mysql-db
```
## Let's inspect container mysql3
## we can check what we are running in "Mounts", "Source"
```
Koitaro@MacBook-Pro-3 ~ % docker container inspect mysql3
[
    {
        "Id": "8717f7966a324d6f52104a4cc27d2e4640cca61a4787be33ced5d7e38e71463f",
        "Created": "2020-05-22T22:55:32.659307107Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "mysqld"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 2678,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-05-22T22:55:32.999711444Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:30f937e841c82981a9a6363f7f6f35ed6b9d5e3f16df50a72207e4a2a389983f",
        "ResolvConfPath": "/var/lib/docker/containers/8717f7966a324d6f52104a4cc27d2e4640cca61a4787be33ced5d7e38e71463f/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/8717f7966a324d6f52104a4cc27d2e4640cca61a4787be33ced5d7e38e71463f/hostname",
        "HostsPath": "/var/lib/docker/containers/8717f7966a324d6f52104a4cc27d2e4640cca61a4787be33ced5d7e38e71463f/hosts",
        "LogPath": "/var/lib/docker/containers/8717f7966a324d6f52104a4cc27d2e4640cca61a4787be33ced5d7e38e71463f/8717f7966a324d6f52104a4cc27d2e4640cca61a4787be33ced5d7e38e71463f-json.log",
        "Name": "/mysql3",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": [
                "mysql-db:/var/lib/mysql"
            ],
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Capabilities": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/e9fde664cac3f69ac24d7f2487e98ccb1090c912fd16e6040481b17aa635616e-init/diff:/var/lib/docker/overlay2/caefe79a655eaee3fbbb8a3b87b97b1994a8584e73b3b8fa32c2edcdfa5e2d34/diff:/var/lib/docker/overlay2/9430667dccfd4e70409f9ec3168b1ebf094891b1ab76440cf5120eed260132a3/diff:/var/lib/docker/overlay2/30256a6f63ac5ffafbfe200d229b7a9dabe7d5f066c21f06c80bc6b3d7829f41/diff:/var/lib/docker/overlay2/8f50c01b098657c0c361bb0e5fe92920727742834fe55103d8d0d6ffafab44d9/diff:/var/lib/docker/overlay2/38d2f94eec35abfce03ff5ac885849b983ff289ae72e2573a46fc8b64eacab21/diff:/var/lib/docker/overlay2/479dce39ff195ef78dee02afc86d2c769937fd1a5e7e34dd42d70640cebf458b/diff:/var/lib/docker/overlay2/4a0142e2bc38d8a59118bebd094d722780b6e1cddb235b947edab2603264ec38/diff:/var/lib/docker/overlay2/6fea2a942777b35828326368a62e77a4a19b21350f87974b2e82d4bf4908e97a/diff:/var/lib/docker/overlay2/7f9ea96fe5ca85d7e2ea05c0e90e39f1d2e3326552c0321eb4ea8c461800636b/diff:/var/lib/docker/overlay2/c764c20506e960aa27f2eb4739de3f3e2b769728bb7079a1080db9d617f4f09d/diff:/var/lib/docker/overlay2/afdbf601dc6acb70e55588b6f041a3b69815b33390112cfee3701c5f75a6680f/diff:/var/lib/docker/overlay2/5a6df8134bd0d038770a8a38598ea187ac92b70549523b5319f4a43e5c5c3a5b/diff",
                "MergedDir": "/var/lib/docker/overlay2/e9fde664cac3f69ac24d7f2487e98ccb1090c912fd16e6040481b17aa635616e/merged",
                "UpperDir": "/var/lib/docker/overlay2/e9fde664cac3f69ac24d7f2487e98ccb1090c912fd16e6040481b17aa635616e/diff",
                "WorkDir": "/var/lib/docker/overlay2/e9fde664cac3f69ac24d7f2487e98ccb1090c912fd16e6040481b17aa635616e/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [
            {
                "Type": "volume",
                "Name": "mysql-db",
                "Source": "/var/lib/docker/volumes/mysql-db/_data",
                "Destination": "/var/lib/mysql",
                "Driver": "local",
                "Mode": "z",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "8717f7966a32",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "3306/tcp": {},
                "33060/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "MYSQL_ALLOW_EMPTY_PASSWORD=True",
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.12",
                "MYSQL_MAJOR=8.0",
                "MYSQL_VERSION=8.0.20-1debian10"
            ],
            "Cmd": [
                "mysqld"
            ],
            "Image": "mysql",
            "Volumes": {
                "/var/lib/mysql": {}
            },
            "WorkingDir": "",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "c2ae7037b3af1f1ce70d1c287c4651fd106199a0313ad122cab72951c161b2c9",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "3306/tcp": null,
                "33060/tcp": null
            },
            "SandboxKey": "/var/run/docker/netns/c2ae7037b3af",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "9e593d6808ee18fdf62200b553f3cfb9ed023e05b72513c99415cfd996543efc",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "fe85293b21abf564881227697d4f2e411a069e9e9cd9e80b955c1a6791bb525c",
                    "EndpointID": "9e593d6808ee18fdf62200b553f3cfb9ed023e05b72513c99415cfd996543efc",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```
# Bind Mounts
![bind_mounts](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/bind_mounts.png)
## Let's try bind mounts with nginx
```
Koitaro@MacBook-Pro-3 udemy-docker-mastery % cd dockerfile-sample-2
Koitaro@MacBook-Pro-3 dockerfile-sample-2 % ls
Dockerfile	index.html

Koitaro@MacBook-Pro-3 dockerfile-sample-2 % ls -alF
total 16
drwxr-xr-x@  4 Koitaro  staff   128 May 15 19:34 ./
drwxr-xr-x  36 Koitaro  staff  1152 May 14 15:38 ../
-rw-r--r--@  1 Koitaro  staff   481 May 21 23:03 Dockerfile
-rw-r--r--   1 Koitaro  staff   249 May 14 15:31 index.html
```
## Dockerfile and index.html in this directory
## Dockerfile is very simple and just specifying a working directory and copy the index.html into that directory
```
# Dockerfile
# this shows how we can extend/change an existing official image from Docker Hub

FROM nginx:latest
# highly recommend you always pin versions for anything beyond dev/learn

WORKDIR /usr/share/nginx/html
# change working directory to root of nginx webhost
# using WORKDIR is preferred to using 'RUN cd /some/path'

COPY index.html index.html

# copy my source code from my local machine into your container images
# I don't have to specify EXPOSE or CMD because they're in my FROM

# index.html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">

  <title>Your 2nd Dockerfile worked!</title>

</head>

<body>
  <h1>You just successfully ran a container with a custom file copied into the image at build time!</h1>
</body>
</html>
```
## Let's run a container
## name it as nginx, and give it port 80:80, and a volume
## we need to tell it our current working directory, which is going to be mounted into that working directory in the container
## index.html is here in the folder. I want this file to be in the container so I can edit it here in the folder and it is seen live in the container
## Instead of typing whole path, we can use $(pwd), which is shell shortcut that prints out working directory and replaces this command with that path
## Right side is the working directory that is specified in Dockerfile
## Finally, we specify the nginx image
![custom_nginx_working_in_localhost](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/custom_nginx_working_in_localhost.png)
```
Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
afb6ec6fdc1c: Already exists
b90c53a0b692: Pull complete
11fa52a0fdc0: Pull complete
Digest: sha256:30dfa439718a17baafefadf16c5e7c9d0a1cde97b4fd84f63b69e13513be7097
Status: Downloaded newer image for nginx:latest
2163ea4cf527cb5b7e83edcea786ea90fb2d180c87d86a2d526bdfbafd9ab717
```
## Let's try to run container names as nginx2 using default nginx index.html file on a different port 8080
![default_nginx_index_html_running_on_8080_edit](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/default_nginx_index_html_running_on_8080_edit.png)
```
Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker container run -d --name nginx2 -p 8080:80 nginx
636665d487b941d1a61b6a35e92102a1c6ff64a5fddf9c7ff3cf9a774acd6bea

Koitaro@MacBook-Pro-3 dockerfile-sample-2 % docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                  NAMES
636665d487b9        nginx               "nginx -g 'daemon of…"   15 seconds ago      Up 14 seconds       0.0.0.0:8080->80/tcp   nginx2
2163ea4cf527        nginx               "nginx -g 'daemon of…"   10 minutes ago      Up 10 minutes       0.0.0.0:80->80/tcp     nginx
```

## Let's now open another command line and get a command line in the container that we are running (custom one)
## Then, go to that directory, specified in Dockerfile
## We can see the Dockerfile because we mapped the whole directory into the working directory in the container
```
Koitaro@MacBook-Pro-3 ~ % docker container exec -it nginx bash
root@2163ea4cf527:/# cd /usr/share/nginx/html
root@2163ea4cf527:/usr/share/nginx/html# ls -al
total 12
drwxr-xr-x 4 root root  128 May 15 23:34 .
drwxr-xr-x 3 root root 4096 May 15 20:15 ..
-rw-r--r-- 1 root root  481 May 22 03:03 Dockerfile
-rw-r--r-- 1 root root  249 May 14 19:31 index.html
```
## Let's create a new file in the current folder
```
Koitaro@MacBook-Pro-3 dockerfile-sample-2 % touch testme.txt

Koitaro@MacBook-Pro-3 dockerfile-sample-2 % ls -alF
total 16
drwxr-xr-x@  5 Koitaro  staff   160 May 22 22:04 ./
drwxr-xr-x  36 Koitaro  staff  1152 May 14 15:38 ../
-rw-r--r--@  1 Koitaro  staff   481 May 21 23:03 Dockerfile
-rw-r--r--@  1 Koitaro  staff   249 May 14 15:31 index.html
-rw-r--r--   1 Koitaro  staff     0 May 22 22:04 testme.txt
```
## Let's check what's in the container with another command line
## Now, we see the created testme.txt
```
root@2163ea4cf527:/usr/share/nginx/html# ls -al
total 12
drwxr-xr-x 5 root root  160 May 23 02:04 .
drwxr-xr-x 3 root root 4096 May 15 20:15 ..
-rw-r--r-- 1 root root  481 May 22 03:03 Dockerfile
-rw-r--r-- 1 root root  249 May 14 19:31 index.html
-rw-r--r-- 1 root root    0 May 23 02:04 testme.txt
```
## Let's write something in testme.txt in the current directory by echo
```
Koitaro@MacBook-Pro-3 dockerfile-sample-2 % echo "is it me you are looking for" > testme.txt
```
## Let's check if the change is reflected in the file in the container
![edited_file_and_check_it_in_the_container](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/edited_file_and_check_it_in_the_container.png)

# Database Upgrades with Named Volumes
![database_upgrades_with_named_volumes](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/database_upgrades_with_named_volumes.png)
## Find the path for volume in Docker Hub
![docker_hub_postgres_9_6_1](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/docker_hub_postgres_9_6_1.png)
![postgres_9_6_1_volume](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/postgres_9_6_1_volume.png)

## Let's run a container from image, postgres 9.6.1
## name it as psql, name the volume as psql, no ports are necessary this time
```
Koitaro@MacBook-Pro-3 ~ % docker container run -d --name psql -v psql:/var/lib/posgtresql/data postgres:9.6.1
Unable to find image 'postgres:9.6.1' locally
9.6.1: Pulling from library/postgres
5040bd298390: Pull complete
f08454c3c700: Pull complete
4db038cdfe03: Pull complete
e1d9ba315f03: Pull complete
25e0ee93170e: Pull complete
3f28084c3f51: Pull complete
78c91f0aedcd: Pull complete
93ab52dbcbb8: Pull complete
27ec75825613: Pull complete
28ef691a9920: Pull complete
0f0dd20755c9: Pull complete
2a4a824861f7: Pull complete
Digest: sha256:0842a7ef786aa2658623085160cb38451eb3d40856e7d222ae0069b6e6296877
Status: Downloaded newer image for postgres:9.6.1
f0b87bfe0a3e561612ac9c1dd33e5d87c0b962c9dd71f54d4e4ccdc5a9de3f11

```
## Check the log of the container, psql
## we can keep watching it by -f command as it runs, database system is ready to accept connections
## then stop the container
```
Koitaro@MacBook-Pro-3 ~ % docker container logs -f psql
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.utf8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/data ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.
syncing data to disk ... ok

Success. You can now start the database server using:

    pg_ctl -D /var/lib/postgresql/data -l logfile start

****************************************************
WARNING: No password has been set for the database.
         This will allow anyone with access to the
         Postgres port to access your database. In
         Docker's default configuration, this is
         effectively any other container on the same
         system.

         Use "-e POSTGRES_PASSWORD=password" to set
         it in "docker run".
****************************************************
waiting for server to start....LOG:  could not bind IPv6 socket: Cannot assign requested address
HINT:  Is another postmaster already running on port 5432? If not, wait a few seconds and retry.
LOG:  database system was shut down at 2020-05-23 13:06:20 UTC
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started
 done
server started
ALTER ROLE


/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*

LOG:  received fast shutdown request
LOG:  aborting any active transactions
LOG:  autovacuum launcher shutting down
waiting for server to shut down....LOG:  shutting down
LOG:  database system is shut down
 done
server stopped

PostgreSQL init process complete; ready for start up.

LOG:  database system was shut down at 2020-05-23 13:06:21 UTC
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started


Koitaro@MacBook-Pro-3 ~ % docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS               NAMES
f0b87bfe0a3e        postgres:9.6.1      "/docker-entrypoint.…"   7 minutes ago       Up 7 minutes        5432/tcp            psql

Koitaro@MacBook-Pro-3 ~ % docker container stop f0b
f0b
Koitaro@MacBook-Pro-3 ~ % docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
## Let's run a container with new upgraded posgtres image 9.6.2, with a different container name
```
Koitaro@MacBook-Pro-3 ~ % docker container run -d --name psql2 -v psql:/var/lib/posgtresql/data postgres:9.6.2
Unable to find image 'postgres:9.6.2' locally
9.6.2: Pulling from library/postgres
10a267c67f42: Pull complete
e9a920522e33: Pull complete
6888e696bd71: Pull complete
798096eed143: Pull complete
fb58419959b5: Pull complete
97f9ec09cb68: Pull complete
d58678d9d3ab: Pull complete
ece2bc4a78f4: Pull complete
eadac36b8440: Pull complete
4da13987a6ca: Pull complete
bd2eab93fc5a: Pull complete
2efd8a94a8d7: Pull complete
cd1f07c4ebbe: Pull complete
Digest: sha256:5284ba74a1065e34cf1bfccd64caf8c497c8dc623d6207b060b5ebd369427d34
Status: Downloaded newer image for postgres:9.6.2
17147db53c0079ba6d19cceb2ecdfd2381ea4d25dcb6ddb772233bb76d753da0

Koitaro@MacBook-Pro-3 ~ % docker container ls -a
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS                     PORTS               NAMES
17147db53c00        postgres:9.6.2      "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes               5432/tcp            psql2
f0b87bfe0a3e        postgres:9.6.1      "/docker-entrypoint.…"   13 minutes ago      Exited (0) 5 minutes ago                       psql
```
## check if we have only one volume for posgres containers because we only used one volume
```
Koitaro@MacBook-Pro-3 ~ % docker volume ls
DRIVER              VOLUME NAME
local               psql
```
## check the logs of the upgraded database container, psql2
## database system is ready to accept connections
```
Koitaro@MacBook-Pro-3 ~ % docker container logs psql2
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.utf8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/data ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
selecting dynamic shared memory implementation ... posix
creating configuration files ... ok
running bootstrap script ... ok
performing post-bootstrap initialization ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.
syncing data to disk ... ok

Success. You can now start the database server using:

    pg_ctl -D /var/lib/postgresql/data -l logfile start

****************************************************
WARNING: No password has been set for the database.
         This will allow anyone with access to the
         Postgres port to access your database. In
         Docker's default configuration, this is
         effectively any other container on the same
         system.

         Use "-e POSTGRES_PASSWORD=password" to set
         it in "docker run".
****************************************************
waiting for server to start....LOG:  could not bind IPv6 socket: Cannot assign requested address
HINT:  Is another postmaster already running on port 5432? If not, wait a few seconds and retry.
LOG:  database system was shut down at 2020-05-23 13:17:38 UTC
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started
 done
server started
ALTER ROLE


/usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*

waiting for server to shut down...LOG:  received fast shutdown request
LOG:  aborting any active transactions
LOG:  autovacuum launcher shutting down
LOG:  shutting down
.LOG:  database system is shut down
 done
server stopped

PostgreSQL init process complete; ready for start up.

LOG:  database system was shut down at 2020-05-23 13:17:39 UTC
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started
```
# Edit Code Running in Containers with Bind Mounts
![bind_mounts_assignment](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/bind_mounts_assignment.png)
### Taking host data that you have on your host machine, and then mounting that into a container, then changing it on the host and watching it be reflected inside the container
```
# standard template files from Jekyll website
Koitaro@MacBook-Pro-3 udemy-docker-mastery % cd bindmount-sample-1
Koitaro@MacBook-Pro-3 bindmount-sample-1 % ls -alF
total 56
drwxr-xr-x@  9 Koitaro  staff   288 May 14 15:31 ./
drwxr-xr-x  36 Koitaro  staff  1152 May 14 15:38 ../
-rw-r--r--   1 Koitaro  staff    58 May 14 15:31 .gitignore
-rw-r--r--   1 Koitaro  staff  4583 May 14 15:31 404.html
-rw-r--r--   1 Koitaro  staff  1126 May 14 15:31 Gemfile
-rw-r--r--   1 Koitaro  staff  1652 May 14 15:31 _config.yml
drwxr-xr-x   3 Koitaro  staff    96 May 14 15:31 _posts/
-rw-r--r--   1 Koitaro  staff   539 May 14 15:31 about.md
-rw-r--r--   1 Koitaro  staff   175 May 14 15:31 index.md
```
## Let's run a container from an image that Bret created for me
## All information is in his docker hub such as WORKDIR, ports that I shoudl expose, etc
## Here is the image from Bret's docker hub
[jekyll-serve image](https://hub.docker.com/r/bretfisher/jekyll-serve)
## When jekyll was started, it built the website from the source file, and server is running
![the_site_mounted_into_directory_in_container](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/the_site_mounted_into_directory_in_container.png)
```
Koitaro@MacBook-Pro-3 bindmount-sample-1 % docker container run -p 80:4000 -v $(pwd):/site bretfisher/jekyll-serve
Unable to find image 'bretfisher/jekyll-serve:latest' locally
latest: Pulling from bretfisher/jekyll-serve
aad63a933944: Pull complete
cca28d9d22b9: Pull complete
e46ee7bc75f6: Pull complete
0f1bb95caf3b: Pull complete
c9e95017f9d8: Pull complete
3ab9072e63ee: Pull complete
ba1c1568b588: Pull complete
79a9273d3131: Pull complete
501b69c4b92e: Pull complete
Digest: sha256:661d4e73afdb91ca8bfe38f40ea361c32d2c540b2b11bed56785149a40935fa2
Status: Downloaded newer image for bretfisher/jekyll-serve:latest
Fetching gem metadata from https://rubygems.org/...........
Fetching gem metadata from https://rubygems.org/.
Resolving dependencies...
Using bundler 2.1.4
Using colorator 1.1.0
Using concurrent-ruby 1.1.6
Using eventmachine 1.2.7
Using http_parser.rb 0.6.0
Using ffi 1.12.2
Using forwardable-extended 2.6.0
Using liquid 4.0.3
Using mercenary 0.3.6
Using safe_yaml 1.0.5
Using unicode-display_width 1.7.0
Fetching rb-fsevent 0.10.4
Fetching public_suffix 4.0.5
Fetching rouge 3.19.0
Using i18n 1.8.2
Fetching rexml 3.2.4
Using rb-inotify 0.10.1
Using pathutil 0.16.2
Using terminal-table 1.8.0
Using em-websocket 0.5.1
Fetching sassc 2.3.0
Installing rexml 3.2.4
Installing rb-fsevent 0.10.4
Installing public_suffix 4.0.5
Using listen 3.2.1
Fetching kramdown 2.2.1
Using jekyll-watch 2.2.1
Using addressable 2.7.0
Installing sassc 2.3.0 with native extensions
Installing rouge 3.19.0
Installing kramdown 2.2.1
Using kramdown-parser-gfm 1.1.0
Using jekyll-sass-converter 2.1.0
Fetching jekyll 4.0.1
Installing jekyll 4.0.1
Fetching jekyll-feed 0.13.0
Fetching jekyll-seo-tag 2.6.1
Installing jekyll-feed 0.13.0
Installing jekyll-seo-tag 2.6.1
Fetching minima 2.5.1
Installing minima 2.5.1
Bundle complete! 6 Gemfile dependencies, 31 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
Post-install message from jekyll:
-------------------------------------------------------------------------------------
Jekyll 4.0 comes with some major changes, notably:

  * Our `link` tag now comes with the `relative_url` filter incorporated into it.
    You should no longer prepend `{{ site.baseurl }}` to `{% link foo.md %}`
    For further details: https://github.com/jekyll/jekyll/pull/6727

  * Our `post_url` tag now comes with the `relative_url` filter incorporated into it.
    You shouldn't prepend `{{ site.baseurl }}` to `{% post_url 2019-03-27-hello %}`
    For further details: https://github.com/jekyll/jekyll/pull/7589

  * Support for deprecated configuration options has been removed. We will no longer
    output a warning and gracefully assign their values to the newer counterparts
    internally.
-------------------------------------------------------------------------------------
Configuration file: /site/_config.yml
            Source: /site
       Destination: /site/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.598 seconds.
 Auto-regeneration: enabled for '/site'
    Server address: http://0.0.0.0:4000/
  Server running... press ctrl-c to stop.
```
## Let's edit the title of the first post in host directory to see if the edit is reflected in the container and website
![the_site_mounted_into_directory_in_container_title_edited](https://github.com/NoriKaneshige/Docker_Volumes_and_Bind_Mounts/blob/master/the_site_mounted_into_directory_in_container_title_edited.png)
