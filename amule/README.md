# Notice

This Dockerfile is for x86_64 architecture only. 

This is a fork of [tchabaud repository](https://github.com/tchabaud/dockerfiles), so all credits for him. Changes are:
* Reduced image size (~100MB)
* aMule compiled using Boost lib to use ASIO sockets instead of wxWidgets

# Usage

## Docker compose

```
---
version: "2.1"
services:
  amule:
    image: jjmcampoy/amule
    container_name: amule
    environment:
      - PUID=1001
      - PGID=1000
      - WEBUI_PWD=password
      - WEBUI_TEMPLATE=AmuleWebUI-Reloaded
    volumes:
      - /path/to/amule/config:/home/amule/.aMule
      - /path/to/amule/tmp:/temp
      - /path/to/amule/incoming:/incoming
    ports:
      - 4711:4711
      - 4662:4662
      - 4672:4672/udp
    restart: unless-stopped
```

## docker cli

Just run the container with the following command lines (replace brackets with the values you want to use) :

### Start from scratch

If you don't have any existing amule configuration, you can specify a custom password for GUI and / or WebUI using adequate environment variables.

- To use amule GUI as interface :

```sh
docker run -p 4712:4712 -p 4662:4662 -p 4672:4672/udp \
    -e PUID=[wanted_uid] -e PGID=[wanted_gid] \
    -e GUI_PWD=[wanted_password_for_gui] \
    -v ./amule/conf:/home/amule/.aMule -v ./amule/incoming:/incoming -v ./amule/tmp:/temp tchabaud/amule
```

- To use web ui from a browser :

```sh
docker run -p 4711:4711 -p 4662:4662 -p 4672:4672/udp \
    -e PUID=[wanted_uid] -e PGID=[wanted_gid] \
    -e WEBUI_PWD=[wanted_password_for_web_interface] \
    -v ./amule/conf:/home/amule/.aMule -v ./amule/incoming:/incoming -v ./amule/tmp:/temp tchabaud/amule
```
### Using an existing amule configuration

Just mount existing directory as a volume :

```sh
docker run -p 4711:4711 -p 4662:4662 -p 4672:4672/udp \
    -e PUID=[wanted_uid] -e PGID=[wanted_gid] \
    -v ~/.aMule:/home/amule/.aMule -v ~/.aMule/Incoming:/incoming -v ~/.aMule/Temp:/temp tchabaud/amule
```

### Web UI theming

If you don't like default amule web ui, you can switch [to this nice bootstrap based web theme](https://github.com/MatteoRagni/AmuleWebUI-Reloaded) by setting the environment variable _WEBUI_TEMPLATE_ to _AmuleWebUI-Reloaded_

*Example* :

```sh
docker run -p 4711:4711 -p 4662:4662 -p 4672:4672/udp \
    -e PUID=[wanted_uid] \
    -e PGID=[wanted_gid] \
    -e WEBUI_TEMPLATE=AmuleWebUI-Reloaded \
    -v ~/.aMule:/home/amule/.aMule \
    -v ~/.aMule/Incoming:/incoming \
    -v ~/.aMule/Temp:/temp tchabaud/amule
```
