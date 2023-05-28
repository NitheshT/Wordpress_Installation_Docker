# Description

This project is a demontration explaining step by step procedure on how we can intall WordPress on docker containers.

## Architeture Model

![Model](https://github.com/NitheshT/Wordpress_Installation_Docket/assets/122042254/9c92a8cf-2534-421c-8514-feb1577872d1)

## Step by step proceedures:

1. Configuriing an EC2 instance with user as mentioned below which has docker installed:

```
#!/bin/bash

echo "ClientAliveInterval 60" >> /etc/ssh/sshd_config
systemctl restart sshd.service

yum install docker -y
usermod -a -G docker ec2-user
systemctl restart docker.service
systemctl enable docker.service
```

2. Creating a custom bridge network:

```
[ec2-user@ip-172-31-7-241 ~]$ docker network create wp-net
3fe95d44e83b415527f3ce8296eaeca551aa63964c03de4016aac9e1306f0751
```

Explanation of user-defined bridge is explained in the official [docker documentation](https://docs.docker.com/network/bridge/#manage-a-user-defined-bridge)

Listing the networks:

```
[ec2-user@ip-172-31-7-241 ~]$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
64f86635eaf7   bridge    bridge    local
70a622a9dafd   host      host      local
d80e6c7f54c8   none      null      local
3fe95d44e83b   wp-net    bridge    local
```

3. Pulling mysql image:

```
[ec2-user@ip-172-31-7-241 ~]$ docker image pull mysql:debian
debian: Pulling from library/mysql
f03b40093957: Pull complete
460bb444d52d: Pull complete
58ceec72175a: Pull complete
8ca6f73d0c4e: Pull complete
37af5e268315: Pull complete
63c12e7f4538: Pull complete
6fe8fac55990: Pull complete
459c00b032f6: Pull complete
da46d64fa018: Pull complete
4c38d2a0b5b8: Pull complete
9d594c6498db: Pull complete
b4d10222ee51: Pull complete
Digest: sha256:8997d21bc7a8406c9ea3de932a25306091749e702a37d51aebafcf93beb3db2e
Status: Downloaded newer image for mysql:debian
docker.io/library/mysql:debian
```

Listing the image:

```
[ec2-user@ip-172-31-7-241 ~]$ docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
mysql        debian    e11a2cc22198   5 days ago   602MB
```

4. Creating MySQL container:

```
[ec2-user@ip-172-31-7-241 ~]$ docker container run \
-d \
--name mysql-container \
--network wp-net \
-e MYSQL_ROOT_PASSWORD="mysqlroot123" \
-e MYSQL_USER="wpuser" \
-e MYSQL_PASSWORD="wpuser123" \
-e MYSQL_DATABASE="wordpress" \
mysql:debian
c6bdd4a55d36b8f8ed01c1b6d1fe977aaf406740bfdb0fa8bc1a26e3b128ee18
```

Details regarding MySQL image and variables defined in the command for container creation can be found by visiting [docker hub](https://hub.docker.com/_/mysql)

5. Pulling WordPress image:

```
[ec2-user@ip-172-31-7-241 ~]$ docker image pull wordpress:latest
latest: Pulling from library/wordpress
f03b40093957: Already exists
662d8f2fcdb9: Pull complete
78fe0ef5ed77: Pull complete
4e258eed73a3: Pull complete
1ddd4f300547: Pull complete
0edd0b2ea53b: Pull complete
ee9d310c9717: Pull complete
3be73cbb78ed: Pull complete
62edfdaa6c06: Pull complete
a7d809597732: Pull complete
8dcda23a1ec0: Pull complete
8e82d36bf349: Pull complete
be2504a04769: Pull complete
f60bbfef8afb: Pull complete
25b9cf8fa6c5: Pull complete
dd6bd4b973f0: Pull complete
bf578ddde323: Pull complete
d0d94cd02b75: Pull complete
5d2692c9049a: Pull complete
6b77814d05fe: Pull complete
231067982bc3: Pull complete
Digest: sha256:a308900c96bb1979a3540f00bc726647dcf6d664e8d23e33a7e84f00a47dc387
Status: Downloaded newer image for wordpress:latest
docker.io/library/wordpress:latest
```

Listing all the images that are currently pulled:

```
[ec2-user@ip-172-31-7-241 ~]$ docker image ls
REPOSITORY   TAG       IMAGE ID       CREATED      SIZE
wordpress    latest    5174bdcbb532   4 days ago   616MB
mysql        debian    e11a2cc22198   5 days ago   602MB
```

6. WordPress container creation:

```
```
