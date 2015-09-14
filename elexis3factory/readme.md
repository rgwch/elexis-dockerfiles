### Simple factory for elexis-3-core

Usage: 

    sudo docker run -v /path/to/elexis/sources:/opt/elexisfactory rgwch/elexisfactory

where /path/to/elexis/sources ist a directory on the host, where the elexis-3-core repository is or can be lconed into.

Note: On Mac and Windows-Hosts, this directory must be within the User's home, or docker won't be able to access it.

The factory will clone elexis-3-core, if the rpository doesn't exist. If it exists, it will `git pull` to update. Then it creates the elexis core, which will be located in /path/to/elexis/sources/elexis-3-core/ch.elexis.core.p2site/target/products after successful build.
