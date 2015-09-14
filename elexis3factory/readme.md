### Simple factory for elexis-3-core

Usage: 

    sudo docker run -v /path/to/elexis/sources:/opt/elexisfactory rgwch/elexisfactory

where `/path/to/elexis/sources` ist a directory on the host, where the elexis-3-core repository is or can be cloned into.

_Note_: On Mac and Windows-Hosts, this directory must be within the User's home, or docker won't be able to access it.

The factory will clone elexis-3-core, if the repository doesn't exist. If it exists, it will `git pull` to update. 
Then it creates the elexis core for all supported architectures
and copies them to `/path/to/elexis/sources/dist` after successful build.

### Speeding up consecutive builds

To re-use a container, find the container using `sudo docker ps -a` and relaunch it with

    sudo docker start -ia name_of_container
    
This will re-use the downloaded maven repository and rebuild the elexis product


To persist a container with preloaded repositories into a new docker image, try something like this:

    docker commit -a "weirich@elexis.ch" -m "loaded maven repository" thirsty_brown rgwch/elexisfactory:1.1.0
    
    
where 'thirsty_brown' is the name of the container as found with `docker ps -a`.
