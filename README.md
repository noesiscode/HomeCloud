# Home cloud cheatsheet: 

## Install docker 

Follow official guide (for example debian):
- [Installation](https://docs.docker.com/engine/install/debian/)
- [Post installation operation](https://docs.docker.com/engine/install/linux-postinstall/)

## Install Portainer CE:

- It Installs as a docker container from docker CLI:

```sh
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.21.4
```
- Connect to: http://localhost:9000
- Choose a user and password.

OR [Official Guide](https://docs.portainer.io/start/install-ce/server/docker/linux), if you follow this guide choose to route the 9000 port instead of 9443 if you don't need HTTPS.

## Workspace Creation
Choose a folder on your system, this is your workspace to host the applications data folders.

Annotate the path of your workspace and take a note like this:

```ssh
home_path=/Users/noesiscode/
```

## Connect on portainer:

- connect to default local environment:

<img width="2532" alt="image" src="https://github.com/user-attachments/assets/45c68d3a-4cdf-44d3-8e8b-6d1a2ebfd8ff" />

## Duckdns

### Register on Duck DNS: 

- go to [Duck DNS](https://www.duckdns.org/)
- login with google account
- copy your token

### Create a new stack on portainer

- call the stack dinamic-dns
- define the stack like this:

```yaml
services:
  duckdns:
    image: lscr.io/linuxserver/duckdns:latest
    container_name: duckdns
    network_mode: host #optional
    environment:
      - PUID=1000 #optional
      - PGID=1000 #optional
      - TZ=Etc/UTC #optional
      - SUBDOMAINS=#YOUR-SUBDOMAIN#
      - TOKEN=#YOUR-TOKEN#
      - UPDATE_IP=ipv4 #optional
      - LOG_FILE=false #optional
    volumes:
      - ${home_path}/config:/config #optional
    restart: unless-stopped
```
- *#YOUR-SUBDOMAIN#*: your preferred subdomain, the url your instance will be like  **your-subdomain.duckdns.org**
- *#YOUR-TOKEN#*: your duckdns token
- Put a new environment variable with your workspace copied from above:

<img width="2197" alt="image" src="https://github.com/user-attachments/assets/d83814b5-d6f8-42e1-a6cd-c4669968df6e" />




