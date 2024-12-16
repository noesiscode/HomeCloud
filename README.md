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

- create a new stack *dynamic-dns*, define the stack like this:

```yaml
services:
  duckdns:
    image: lscr.io/linuxserver/duckdns:latest
    container_name: duckdns
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
- edit *#YOUR-SUBDOMAIN#*: your preferred subdomain, the url your instance will be like  **your-subdomain.duckdns.org**
- edit *#YOUR-TOKEN#*: your duckdns token
- create a new environment variable with your workspace copied from above:

<img width="2197" alt="image" src="https://github.com/user-attachments/assets/d83814b5-d6f8-42e1-a6cd-c4669968df6e" />

## Nginx reverse proxy

### Nginx Proxy Manager

- (official site)[https://nginxproxymanager.com/]
- create the required folders on your workspace:
  ```sh
  mkdir nginx_manager
  cd nginx_manager
  mkdir data
  mkdir letsencrypt
  ```
  
- create a new stack called **apps** and define the stack like this:

```sh
version: '3.8'
services:
  nginx_manager:
    image: 'jc21/nginx-proxy-manager:latest'
    restart: unless-stopped
    ports:
      - '80:80' # Public HTTP Port
      - '443:443' # Public HTTPS Port
      - '81:81' # Admin Web Port

    volumes:
      - ${home_path}/nginx_manager/data:/data
      - ${home_path}/nginx_manager/letsencrypt:/etc/letsencrypt
```

- again create a new environment env variable **home_path** 

- now you have: http page welcome page http://localhost and a manager gui: http://localhost:81

- access to manager gui with default credentials:

```sh
Email:    admin@example.com
Password: changeme
```

- change your mail and your password


## Navidrome


### Navidrome setup

- create 3 folder on your workspace:

```sh
mkdir navidrome
cd navidrome
mkdir data
mkdir music
```

- ...put some music into music folde

- modify the apps stack on portainer and add this:

```yaml
services:
  navidrome:
    image: deluan/navidrome:latest
    user: 1000:1000 # should be owner of volumes
    ports:
      - "4533:4533"
    restart: unless-stopped
    environment:
      # Optional: put your config options customization here. Examples:
      ND_SCANSCHEDULE: 1h
      ND_LOGLEVEL: info  
      ND_SESSIONTIMEOUT: 24h
      ND_BASEURL: ""
    volumes:
      - "${home_path}/navidrome/data:/data"
      - "${home_path}/navidrome/music:/music:ro"
```

- again create a new environment env variable **home_path**
- visit http://localhost:4533 and setup an admin account


### Nginx setup

- login into nginx manager and add a proxy host:

<img width="497" alt="image" src="https://github.com/user-attachments/assets/e9f5faca-e4b5-41c5-868e-66360d10eca2" />




















