# aMule in Ubuntu

This image will setup aMule daemon 2.3.2 in an Ubuntu trusty (14.04) container. The Dockerfile will build aMule from its sources and apply a custom patch.

The image also patches the official webserver with a more modern UI: https://github.com/MatteoRagni/AmuleWebUI-Reloaded

## Start from scratch

Building the image:
<pre>
docker build --platform=linux/amd64 -t amule-amd64:2.3.2 .
</pre>

## Image configuration

The image offers some optional configuration and the possibility to bind some external volumes. Additionaly some ports shall be forwarded out of the container to access (i.e. the web

### Volumes
Ensure that the aMule UID/GID has read/write/execute rights in all volumes
- **/config** for storing the amule configuration. Might be useful to tune some parameters not available via web
- **/temp** to store aMule part files. You could keep this volume within the docker container
- **/incoming** to store downloaded files. You definitely want to map this volume to an external folder

### UID, GID
First time you run the image, the startup script will create the user and group "amule" for you, with the default uid and gid 5000. You can select a different UID and GID setting the environment variables _PUID_ and _PGID_ respectively.

It is important that the UID and GID map to a user in your Linux machine which has rights to write into the previous volumes

### Ports

- **4711/tcp** for the web server
- 4712/tcp
- 47660/udp
- 52718/udp
- 47657/tcp

### Web Server Password
You can setup the password via an environment variable (_WEBUI_PWD_), if you don't do that, the startup script will create a new password for you and will write it to the console if you started in interactive mode (-i)

### Remote GUI Password
You can setup the password via an environment variable (_RGUI_PWD_), if you don't do that, the startup script will create a new password for you and will write it to the console if you started in interactive mode (-i)


## Testing the image:
<pre>
docker run --rm [-ePUID=UID] [-ePGID=GID] -eRGUI_PWD=[PASSWORD] -eWEBUI_PWD=[PASSWORD] -p4711:4711 -p4712:4712 -p47660:47660/udp -p52718:52718/udp -p47657:47657 -v/mnt/host/aMule/Temp:/temp -v/mnt/host/aMule/Incoming:/incoming -v/mnt/host/aMule/config:/config amule-amd64:2.3.2
</pre>

## Creating a container:
<pre>
docker create --name amule [-ePUID=UID] [-ePGID=GID] -eRGUI_PWD=[PASSWORD] -eWEBUI_PWD=[PASSWORD] -p4711:4711 -p4712:4712 -p47660:47660/udp -p52718:52718/udp -p47657:47657 -v/mnt/host/aMule/Temp:/temp -v/mnt/host/aMule/Incoming:/incoming -v/mnt/host/aMule/config:/config amule-amd64:2.3.2
</pre>

## Do you like this Docker image ?
