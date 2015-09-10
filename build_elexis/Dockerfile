# Use Debian Jessie (8.1)
FROM debian:jessie
MAINTAINER Niklaus Giger "elexis.giger@member.fsf.org"

RUN apt-get update && \
    apt-get install -y libxext-dev libxrender-dev libxtst-dev wget unzip sudo vim maven acl ruby git

# Install Oracle Java-8 and accept automatically its license
RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee /etc/apt/sources.list.d/webupd8team-java.list && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886 && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections && \
    apt-get update && \
    apt-get install -y oracle-java8-installer

RUN apt-get install -y xvfb xterm
# cleanup (minimizes size of generated image)
# RUN apt-get clean && \
#     rm -rf /var/lib/apt/lists/* && \
#     rm -rf /tmp/*

# The next step is needed or maven would save the all downloaded plugins under /root/.m2 (the home specified via /etc/passd)
# Reason: java defines .user.home as /root even when the $HOME environment is /home/elexis
# we need a symbolic link to allow Elexis to work correctly
RUN ln -s /home/elexis/.m2 /root
COPY assets/build_inside_container /usr/local/bin/build_inside_container
CMD /bin/bash
VOLUME /home/elexis
ENV HOME /home/elexis
WORKDIR /home/elexis

