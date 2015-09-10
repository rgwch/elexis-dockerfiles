# Use Debian Wheezy update 8
FROM debian:7.8
MAINTAINER Niklaus Giger "elexis.giger@member.fsf.org"

#    apt-get install -y libreoffice-writer # to be able to use ODF documents
# iceweasel # to be able to display web pages
# icedove # to be able to send-email
RUN apt-get update && \
    apt-get install -y abiword \
    iceweasel \
    icedove \
    libxext-dev libxrender-dev libxtst-dev wget unzip sudo vim \
    libwebkitgtk-1.0-0

# Install Oracle Java-8 and accept automatically its license
RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" | tee /etc/apt/sources.list.d/webupd8team-java.list && \
    apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys EEA14886 && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections && \
    apt-get update && \
    apt-get install -y oracle-java8-installer

RUN mkdir -p /opt/elexis && \
    echo 'Installing demoDB' && \
    cd       /opt/elexis && \
    wget --quiet http://download.elexis.info/demoDB/demoDB_3.0_with_administrator.zip -O demoDB.zip

RUN wget --quiet -q --no-check-certificate -O /tmp/elexis.zip https://jenkins.elexis.info/jenkins/view/3.0/job/Elexis-3.0-Core/lastSuccessfulBuild/artifact/ch.elexis.core.p2site/target/products/ch.elexis.core.application.ElexisApp-linux.gtk.x86_64.zip && \
    echo 'Installing elexis' && \
    cd       /opt/elexis && \
    unzip -q /tmp/elexis.zip && \
    rm /tmp/elexis.zip

RUN cd /usr/local/bin && \
  wget --quiet http://ftp.fau.de/eclipse/tools/buckminster/products/director_latest.zip && \
  unzip -q director_latest.zip

RUN /usr/local/bin/director/director \
  -destination /opt/elexis \
  -repository http://download.elexis.info/elexis.3.core/snapshot/ \
  -repository http://download.elexis.info/elexis.3.base/snapshot/ \
  -application org.eclipse.equinox.p2.director \
  -installIU ch.elexis.base.ch.feature.feature.group \
  -installIU org.iatrix.feature.feature.group \
  -installIU ch.elexis.omnivore.feature.feature.group \
  -installIU ch.elexis.base.ch.legacy.feature.feature.group \
  -installIU com.hilotec.elexis.opendocument.feature.feature.group \
  -installIU com.hilotec.elexis.kgview.feature.feature.group \
  -installIU com.hilotec.elexis.messwerte.v2.feature.feature.group \
  -installIU ch.elexis.agenda.feature.feature.group \
  -installIU ch.medelexis.text.templator.feature.feature.group

ADD assets/run /usr/local/bin/elexis-3

# install support for all languages
RUN apt-get install -y locales-all

# cleanup (minimizes size of generated image)
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/*

# As Eclipse/Java defines .user.home as /root even when the $HOME environment is /home/elexis
# we need a symbolic link to allow Elexis to work correctly
# RUN ln -s /home/elexis/elexis /root

RUN chmod +x /usr/local/bin/elexis-3
# If you want to use a different user ID for elexis, uncomment and adapt the following lines
RUN chmod +x /usr/local/bin/elexis-3 && \
     mkdir -p /home/elexis && \
     echo "elexis:x:1300:1300:Elexis user,,,:/home/elexis:/bin/bash" >> /etc/passwd && \
     echo "elexis:x:1300:" >> /etc/group && \
     echo "elexis ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/elexis && \
     chmod 0440 /etc/sudoers.d/elexis && \
     chown elexis:elexis -R /home/elexis && \
     chown root:root /usr/bin/sudo && chmod 4755 /usr/bin/sudo
USER elexis


CMD /usr/local/bin/elexis-3
# CMD /bin/bash
ENV HOME /home/elexis
WORKDIR /home/elexis

