# Dockerized PathOS

PathOS has a number of components, and elements of configuration
that all have to work together to build a functioning system. Using
Docker makes this easier.

Dockerised PathOS is implemented as a set of cooperating containers that work together to create a working
system. This is shown schematically [here](https://github.com/PapenfussLab/PathOS/tree/master/Dockish-PathOS/Containers.png).

## To bring up an instance

Follow these [commands](https://github.com/PapenfussLab/PathOS/tree/master/Dockish-PathOS/docker_commands_readme.txt) for building on Linux or MacOS without a proxy in the way.
In summary:
- clone/download this repository
- build a WAR file for PathOS
- invoke: docker-compose build
  This will download the necessary docker images and build the
  required derived images.
- populate a demonstration database
- invoke: docker-compose up
  This will create the necessary containers.
  It may take a minute or two to start up.
- browse to http://localhost/PathOS

Dealing with proxies can be tricky and is covered [here](https://github.com/PapenfussLab/PathOS/tree/master/Dockish-PathOS/docker/pathos-build) 
and below. It is recomended to perform the build process outside an organisation's proxy to reduce any networking problems. We are in the process
of making pre-built images available to simplify deployments.

## Using Docker Machine

Create the virtual machine:

    docker-machine create --driver=virtualbox --virtualbox-memory=2048 pathos-machine

If you're behind a proxy, you'll need to set the proxy. If it's
feasible to put the proxy settings directly in the VM, you can
follow the instructions at

    http://mflo.io/2015/08/13/docker-machine-behind-proxy/

However, if you have to authenticate to the proxy, then it is
undesirable to put your credentials in the clear text profile. To
avoid this we assume you're running the cntlm local proxy which
takes care of the authentication for you. To that end, you need to
use port forwarding. For the VM, you need to set proxy environment
variables that point to a port on the VM will be forwared to a port
on the host. (The indented commands are to be executed in the ssh.)

    docker-machine ssh pathos-machine
        sudo -s
        echo export HTTP_PROXY=http://localhost:3128 >> /var/lib/boot2docker/profile
        echo export HTTPS_PROXY=http://localhost:3128 >> /var/lib/boot2docker/profile
        exit
        exit

    docker-machine restart pathos-machine

Now we need to start port-forwarding:
    
    docker-machine ssh pathos-machine -R localhost:3128:localhost:3128 -f -N

Now to get the HTTP port 80, or HTTPS port 443 visible to the outside world, we 
need to forward the relevant port from the host to the VM:

    docker-machine ssh pathos-machine -L localhost:8088:localhost:80 -f -N

With these in place, we can now bootsrap the docker-machine for our application:

    eval $(docker-machine env pathos-machine)
    docker-compose build
    docker-compose create
    docker-compose up -d

