Why not boot2docker?
==================

Running Docker with boot2docker has many compatibility issues on Mac OSX, especially, mounting volumes. People have tried many complicated ways to get it working with additional container volumes ([svendowideit/samba](https://registry.hub.docker.com/u/svendowideit/samba/), rsync, etc...).

Read this boot2docker issue for details: [#4023](https://github.com/dotcloud/docker/issues/4023)

This vagrantfile provides a simplier solution by using docker client with vagrant host. File sharing feature is provided by vargant. You have more control over your docker instance.

### With boot2docker

    [Docker Mac Client] --> DOCKER_HOST=tcp://$(boot2docker ip 2>/dev/null):2375 --> boot2docker.iso


### With vagrant

    [Docker Mac Client] --> DOCKER_HOST=tcp://localhost:4243 --> Vagrant docker instance


## Installation
  
  * Install [Vagrant](https://www.vagrantup.com/downloads.html)
  * Install [boot2docker](https://github.com/boot2docker/osx-installer/releases), we only use the docker client provided by boot2docker

#### Step 1: Provisioning

Create an instance with docker daemon. The vagrant instance exposes docker service at port 4243

    vagrant up
  
  
#### Step 2: Modify DOCKER_HOST

Change the default DOCKER_HOST in enviroment variable to the vagrant instance, e.g.

    # Change from:
    # export DOCKER_HOST=tcp://$(boot2docker ip 2>/dev/null):2375
    
    # Change to:    
    export DOCKER_HOST=tcp://localhost:4243

Finally reload/source your environment variables. Now you can use `docker` command transparently.

Done!


#### Volumes

Vagrant by default mounts current `Vagrantfile` working directory to '/vagrant'. You can change source by adding the following line to Vagrantfile (read comments in the file):

    config.vm.synced_folder "/Projects", "/vagrant"


Now docker can mount your directory:

    docker run -v /vagrant:/container-dir <container-id>



