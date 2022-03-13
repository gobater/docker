# aMule in Ubuntu



## Start from scratch

Building the image:
<pre>
docker build --platform=linux/amd64 -t amule-amd64:2.3.2 .
</pre>

## Running the image

The image offers some optional configuration.

### Volumes
- **/config** for storing the amule config. Ensure that the aMule UID/GID has read/write/execute rights in the complete folder
- **/temp** to store part files
- **/incoming** to store downloaded files

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
You can setup the password via an environment variable, if you don't do that, the startup script will do it for you

Creating a container:
<pre>
docker create --name amule -ePUID=[UID] -ePGID=[GID] -eWEBUI_PWD=[PASSWORD] -p4711:4711 -p4712:4712 -p47660:47660/udp -p52718:52718/udp -p47657:47657 -v/mnt/host/aMule/Temp:/temp -v/mnt/host/aMule/Incoming:/incoming -v/mnt/host/aMule/config:/config amule-amd64:2.3.2
</pre>



## Do you like this Docker image ?
