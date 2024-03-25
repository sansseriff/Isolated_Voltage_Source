## Isolated Voltage Source Webserver and GUI

A vource source controll user interface  web-app developed with svelte on the frontend, and FastAPI on the backend

<!-- <img style="display: block; margin-left: auto; margin-right: auto; width: 30%" src="GUI.PNG"> -->

<!-- <p align="center">
  <img width="300" src="GUI.PNG">
</p> -->

![UI](./vsource_cropped_dark.png#gh-dark-mode-only)
![UI](./vsource_cropped_light.png#gh-light-mode-only)


The webserver is packaged using docker. Docker commands to deploy or build the container:


### Stop and remove existing container:
```console
docker rm -f vsource_control_container
```

### Build command:
```console
docker build -t vsource_control .
```

run command:
### to make a new container from updated image:
```console
docker run -d --restart unless-stopped --name vsource_control_container -p 80:80 vsource_control 
```


### to run existing container
```console
docker run -d -p 80:80 vsource_control -d --restart unless-stopped
```

Note: if you're changing something like CSS, you might need to rebuild the container with no cache. The docker rebuid process is iterative and might not 'notice' that a particular file needs to be updated:
```console
docker build -t vsource_control . --no-cache
```
### to see the console outputs of the container (including outputs from python's `print()`)
1. get the container id:
```console
docker ps
```
2. view the logs in real time
```console
docker logs -f <container_id>
```