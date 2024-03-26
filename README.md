# Isolated Voltage Source Webserver and GUI

A web-app user interface for controlling an UDP connected isolated voltage source, built with svelte for the frontend, and FastAPI for the backend. 

<!-- <img style="display: block; margin-left: auto; margin-right: auto; width: 30%" src="GUI.PNG"> -->

<!-- <p align="center">
  <img width="300" src="GUI.PNG">
</p> -->

![UI](https://raw.githubusercontent.com/sansseriff/Isolated_Voltage_Source/master/vsource_cropped_dark.png#gh-dark-mode-only)
![UI](https://raw.githubusercontent.com/sansseriff/Isolated_Voltage_Source/master/vsouce_cropped_light.png#gh-light-mode-only)


## Installation

### 1. Install docker

#### Red Hat Enterprise Linux (RHEL) 7.X
RHEL7 needs some additional steps prior to installing Docker CE Engine.

1. Add the docker community edition repo

    ```console
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    ```

2. Edit repo to add centos (centos is the open source free version of RHEL, at least up until RHEL 8)

    Edit the `/etc/yum.repos.d/docker-ce.repo`, for example using nano with the command:

    ```console
    sudo nano /etc/yum.repos.d/docker-ce.repo
    ```

    Add the following to the top or bottom of the file:
    ```
    [centos-extras]
    name=Centos extras - $basearch
    baseurl=http://mirror.centos.org/centos/7/extras/x86_64
    enabled=1
    gpgcheck=0
    ```

    Save the file, and run `sudo yum update`

    Now, you should be able to run `sudo yum install docker`. 

3. Without extra configuration, all docker commands will need to be prefaced with `sudo`. For example, `docker ps` becomes `sudo docker ps`

4. After installation, you may need to start the docker daemon:
    ```
    sudo systemctl start docker
    ```

#### Red Hat Enterprise Linux (RHEL) 8 - untested

The above guide for RHEL may work, with the baseurl in `[centos-extras]` changed to `https://mirror.centos.org/centos/8/extras/x86_64/`. Also or alteratively, [this](https://docs.docker.com/engine/install/centos/) guide may work. 

#### Ubuntu

follow [this](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04) guide for installation, and for configuring docker commands to work without sudo. 

### 2a. Download and run published image
If you don't plan to customize the voltage source user interface or back-end webserver, you may download a version from dockerhub:

Download:

```
docker pull sansseriff/vsource_control
```

Run:
```
docker run -d --restart unless-stopped --name vsource_control_container -p 80:80 sansseriff/vsource_control
```

The user interface should now be visible by typing `0.0.0.0` into the browser of the computer running docker. If docker was installed on a remote host computer on the same network, view the UI by directing the browser to the ip address of the host computer. 

### 2b. Clone github repo and build container
If you need to customize the user interface or webserver, clone this repository, and rebuild the container:

```
git clone https://github.com/sansseriff/Isolated_Voltage_Source.git
```

You may customize backend python code in the `backend/` directory. 

In order to customize and rebuild the frontend javascript and html, you will need to have `node` installed. Inside the `frontend/` folder, run `npm install` to install all the necessary libraries needed to work with svelte. Running `npm run build`, will rebuild the frontend to vanilla javascript & html and place it in `backend/snspd_bias_control/` where the python backend webserver will find it. 

When the code is ready to go, run this command in the root directory to build the container, using the instructions present in the local `Dockerfile`; 

```
docker build -t vsource_control .
```

Then run the built container image:

```
docker run -d --restart unless-stopped --name vsource_control_container -p 80:80 vsource_control 
```

The user interface should now be visible by typing `0.0.0.0` into the browser of the computer running docker. If docker was installed on a remote host computer on the same network, view the UI by directing the browser to the ip address of the host computer. 


## Various Useful Docker Commands

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


### Stop and remove existing container:
```console
docker rm -f vsource_control_container
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