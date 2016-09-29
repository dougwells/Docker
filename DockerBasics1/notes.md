# Docker Machine
<!-- Commands (DM = docker-machine)
$ docker-machine ls				→ list docker mach
$ docker-machine start [machine name]
$ docker-machine env [machine name] 	→ change env vars
$ docker-machine ip				→ ip address of mach
$ docker-machine				→ list all commands
Run above commands from Docker QuickStart Terminal
Different than standard terminal
Test with any docker client command (ie, $ docker ps )
If doesn’t work, see link below (Docker issue w/iTerm2)
Problem with Docker & iTerm2.  Easy fix in link below (modify Docker Quickstart Terminal.app/Contents/Resources/Scripts/iterm.scpt)
See https://gitlab.com/gnachman/iterm2/issues/4287
Even w/above, may want to hook “normal” terminal to docker machine.  Follow steps below to do this.
Test w/ $ docker ps (as above). If err, steps below
$ docker-machine env [machine name]   
must include name, even if for default machine
Returns the following.  NOTE blue font
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://192.168.99.100:2376"
export DOCKER_CERT_PATH="/Users/dougwells/.docker/machine/machines/default"
export DOCKER_MACHINE_NAME="default"
Run this command to configure your shell:
eval $(docker-machine env default)

Run blue font command above to configure shell
$ eval $(docker-machine env default) 1
Now test $ docker ps
Should work -->


# Docker Client
Docker Client (DC) - download & manages images and containers from w/in linux virt box
Docker Client commands
(note if commands not working, terminal might not be “hooked up” to DM)
$ docker 			- lists all available commands
$ docker pull [image name]	- downloads from docker.hub
$ docker run [image name]	- runs the image (& pulls if not local)
$ docker start [cont ID]		- starts an existing container
$ docker stop [cont ID]		- stops a container (does NOT delete)
$ docker rm [cont ID]		- deletes.  Add -f tag if need to force
$ docker images		- lists all images
$ docker ps			- lists all containers running
$ docker ps -a			- list all containers (running & stopped)
$ docker logs [cont ID]		- show logs of running container
$ docker network ls		- list docker networks
$ docker network inspect [ID]	- network info (including containers in it)
$ docker exec [cont ID]  xyz	- execute command in running container
$ docker exec d64 node seed.js	- as if in console (d64 $ node seed.js)


Using Docker Client commands
docker pull 	-  dockerHub.com, find image & copy pull command
docker rm 198..	-  removes/deletes container (only if it is stopped)
docker rm -f 198 - removes force deletes container (even if running)
docker rmi 198	  - removes image (only need first few parts of identifier)
docker run -p 80:80 imageName - on port (machine port : container port)
Container export IP = http://192.168.99.100/
Node example → $ docker run -p 8080:3000 node
docker stop 198  - stops container

# Communicating between multiple containers
Communicating between containers
High Level, 2 methods
Legacy Linking - via naming convention
Bridge Network - container w/in bridge network can talk w/each other


Legacy Linking
3 steps
Each container to be linked must have a name
$ docker run -d --name <contName> <imageName>
	Example: 	$ docker run -d --name my-mongodb  mongo


Link to running container by name
$ docker run -p 3000:3000 --link my-mongodb:mongodb danwal/node
This command runs danwal/postgres
Links danwal/node to my-mongodb
<contName>:<contAlias> - gives alias (‘mongodb’)
can use alias internally by linked containers (node)
Optional to give a named container an alias

Repeat for additional containers


Example: Linking Node.js container to MongoDB container
Run mongodb from image AND NAME THE CONTAINER
$ docker run -d --name my-mongodb  mongo
Run node from image & link to mongodb container (“my-mongodb”)
$ docker run -p 3000:3000 --link my-mongodb:mongodb dougwells/node
That’s it …


Network Linking (Creating an isolated network).
AKA “Container Networking with a Bridge Driver”
NOTE: if connecting more than 2 or 3 containers, likely better to use Docker Compose (see below)
Note: Docker docs refers to networking as “Communicate between containers”
Steps
Create a bridge network
Run containers w/in that network
All containers within bridge network can “talk” with one another
Note: containers can belong to more than one network
Docker commands for creating bridge network & running containers in it
Create network
$ docker network create --driver bridge isolated_network
create network	    	 bridge network	   network name

Run containers from within the network
$ docker run -d --net=isolated_network --name mongodb mongo
$ docker run -d --net=isolated_network --name nodeapp -p 3000:3000 dougwells/node

Docker commands that help with networks
$ docker network ls		- list docker networks
$ docker network inspect [ID]	- network info (including containers in it)
$ docker disconnect  [cont ID}	- disconnect a container from a network
$ docker network rm [ntwk ID]	- remove entire network

# Docker Compose	
