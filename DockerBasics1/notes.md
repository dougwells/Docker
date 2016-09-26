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
