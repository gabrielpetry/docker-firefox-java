# Why do I need Firefox with Java plugin in Docker?

iDRAC, all because iDRAC. I couldn't smoothly remote connect to my DELL machine console on mac. So I dockerize it.

# Usage

Allow xorg in another hostname, you can specify whatever hostname you want, but the container must use it

    xhost +local:batman
    
Start the container

    docker run -it --rm \
    -e "DISPLAY=${DISPLAY}" \
    -v "/tmp/.X11-unix:/tmp/.X11-unix" \
    -v "$HOME/DockerDownloads:/home/firefox/Downloads" \
    -h 'batman' \
    gabrielpetry/firefox-java

It works!!!

![iDrac](iDRAC.png "iDrac")

# Issue

use oracle-java8 will occur "JVMLauncher.afterStart(): starting JVM process watcher" InterruptedException

    RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" > /etc/apt/sources.list.d/OracleJava.list && \
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886 && \
    apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y oracle-java8-installer oracle-java8-set-default

    RUN DEBIAN_FRONTEND=noninteractive apt-get install -y dbus dbus-x11 && \
    dbus-uuidgen > /var/lib/dbus/machine-id

# Reference

* [Running GUI apps with Docker](http://fabiorehm.com/blog/2014/09/11/running-gui-apps-with-docker/)
* [khozzy/docker](https://github.com/khozzy/docker/blob/master/selenium_java_firefox/Dockerfile)
* [how to use -e DISPLAY flag on osx?](https://github.com/docker/docker/issues/8710)
