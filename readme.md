# 1 - INSTALL DE DOCKER

Selon mes connaissances ancestrales (grâce à la doc surtout), voici comment installer docker sur une machine

### a - apt install (oue cé importan)

--> sah si j'ai besoin de tout écrire ... bref :

```
toto@zizicopter:~$ sudo apt-get install ca-certificates curl
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
ca-certificates is already the newest version (20240203).
ca-certificates set to manually installed.
curl is already the newest version (8.5.0-2ubuntu10.6).
curl set to manually installed.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
toto@zizicopter:~$ sudo install -m 0755 -d /etc/apt/keyrings
toto@zizicopter:~$ sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
toto@zizicopter:~$ sudo chmod a+r /etc/apt/keyrings/docker.asc
toto@zizicopter:~$ echo \
>   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
>   $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
>   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
toto@zizicopter:~$ sudo apt-get update
Hit:1 http://azure.archive.ubuntu.com/ubuntu noble InRelease
Hit:2 http://azure.archive.ubuntu.com/ubuntu noble-updates InRelease
Hit:3 http://azure.archive.ubuntu.com/ubuntu noble-backports InRelease
Hit:4 http://azure.archive.ubuntu.com/ubuntu noble-security InRelease
Get:5 https://download.docker.com/linux/ubuntu noble InRelease [48.8 kB]
Get:6 https://download.docker.com/linux/ubuntu noble/stable amd64 Packages [20.3 kB]
Fetched 69.2 kB in 1s (99.1 kB/s)
Reading package lists... Done
toto@zizicopter:~$
```
puis 

```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
On vérifie qu'on est pas débile au point de fail une install a partir d'une doc : 

```
toto@zizicopter:~$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
e6590344b1a5: Pull complete
Digest: sha256:7e1a4e2d11e2ac7a8c3f768d4166c2defeb09d2a750b010412b6ea13de1efb19
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
```

Masterclass ! On est pas attardé hihi


### b - ajout de notre user dans le grp docker

On va add notre petit toto au grp docker pour éviter de sudo toute les commandes

```
sudo usermod -aG docker $(whoami)
```

On vérifie : 

```
toto@zizicopter:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
toto@zizicopter:~$
```
On le voit ci-dessus : plus besoin de sudo







