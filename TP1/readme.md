# PART 1
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

# 2 - USE IT 
### a - commencer lancer un docker
On va maintenant mettre les mains dans le cambouis en lançant notre premier conteneur !

```docker run --name web -d -v /home/toto/nginx:/usr/share/nginx/html -p 9999:80 nginx```
Ici --> on lance un conteneur a partir de l'image nginx, ou son nom sera web (--name) et ou le port machine 8888 sera connecté au port 80 du conteneur (-p)
        Le -d signifie que le conteneur tourne en arrière plan. Le -v /home/toto/nginx:/usr/share/nginx/html monte le répertoire /home/toto/nginx de ta machine locale à l'intérieur du conteneur, dans le répertoire /usr/share/nginx/html. Cela permet à Nginx, à l'intérieur du conteneur, d'avoir accès aux fichiers HTML présents sur la machine

```
toto@zizicopter:~/nginx$ docker run --name web -d -v /home/toto/nginx:/usr/share/nginx/html -p 9999:80 nginx
ecf3aa351af879d14fe36efdd957c8e8181a79d7e87c0b4631507d258e773564
toto@zizicopter:~/nginx$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                     NAMES
ecf3aa351af8   nginx     "/docker-entrypoint.…"   4 seconds ago   Up 3 seconds   0.0.0.0:9999->80/tcp, [::]:9999->80/tcp   web
toto@zizicopter:~/nginx$
```

Je sais pas comment je vais expliquer ça, j'ai pas de fw sur la machine, ouvert le port sur azure...

```
PS C:\Users\Jessy> curl http://172.187.219.45:9999


StatusCode        : 200
StatusDescription : OK
Content           : Welcome to nginx

RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Accept-Ranges: bytes
                    Content-Length: 17
                    Content-Type: text/html
                    Date: Mon, 17 Mar 2025 21:57:35 GMT
                    ETag: "67d89abf-11"
                    Last-Modified: Mon, 17 Mar 2025 21...
Forms             : {}
Headers           : {[Connection, keep-alive], [Accept-Ranges, bytes], [Content-Length, 17], [Content-Type,
                    text/html]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 17



PS C:\Users\Jessy>
```
### b - comment custom le lancement d'un docker 
On custom le launch de ce conteneur : 

```
toto@zizicopter:~/nginx$ docker run --name meow \
>   -d \
>   -v /home/toto/nginx.conf:/etc/nginx/conf.d/custom.conf \
>   -v /home/toto/tp_docker:/var/www/tp_docker \
>   -p 7777:7777 \
>   --memory=512m \
>   nginx
8deadcd8af56e49a4880ff87b05807a5548a5a891823cd76ce0651fe5505a112
toto@zizicopter:~/nginx$
```
Expliquons en bref --> --name pour l'appeler meow (chef, c'est quoi ce nom ???) 
                        -v lie mon fichier de conf et .html de la machine a ceux du docker
                        -p set les ports d'écoute sur 7777 et 7777 de la machine et du docker

