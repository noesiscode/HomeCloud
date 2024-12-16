# Home cloud cheatsheet: 

## Install docker 

Follow official guide (for example debian):
- [Installation](https://docs.docker.com/engine/install/debian/)
- [Post installation operation](https://docs.docker.com/engine/install/linux-postinstall/)

## Install Portainer CE:

(Official Guide)[https://docs.portainer.io/start/install-ce/server/docker/linux]
N.B. Choose to route the 9000 port instead of 9443 if you don't need HTTPS.
```sh
docker volume create portainer_data
docker run -d -p 8000:8000 -p 9000:9000 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:2.21.4
```
Choose a user and password.

## Workspace Creation
Choose a folder on your system, this is your workspace to host the applications data folders.

Annotate the path of your workspace and take a note like this:

```ssh
home_path=/Users/noesiscode/
```

