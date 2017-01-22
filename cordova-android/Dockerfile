FROM node:boron

MAINTAINER Paulo Haacke <paulohaacke@gmail.com>

RUN npm install json-server cordova ionic -g 

# Java 8
RUN echo "deb http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" > /etc/apt/sources.list.d/java-8-debian.list
RUN echo "deb-src http://ppa.launchpad.net/webupd8team/java/ubuntu trusty main" >> /etc/apt/sources.list.d/java-8-debian.list
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886
RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections
RUN apt-get update
RUN apt-get install -y oracle-java8-installer
RUN apt-get install -y oracle-java8-set-default
ENV JAVA_HOME "/usr/lib/jvm/java-8-oracle"

# Android SDK
ENV ANDROID_HOME /opt/android-sdk
RUN cd /tmp && wget https://dl.google.com/android/repository/tools_r25.2.3-linux.zip
RUN apt-get install -y unzip
RUN mkdir ${ANDROID_HOME}
RUN unzip /tmp/tools_r25.2.3-linux.zip -d ${ANDROID_HOME}
ENV PATH $PATH:${ANDROID_HOME}/tools:${ANDROID_HOME}/platform-tools

# Android SDK Packages
RUN (echo y | android update sdk -s --no-ui --all --filter 2,3,33,34,36,37,166)

# Npm dependencies for the project
RUN mkdir -p /opt/app
ADD package.json /opt/app/package.json
RUN cd /opt/app && npm install

# Bower dependencies for the project
RUN npm install bower -g
ADD bower.json /opt/app/bower.json
RUN cd /opt/app && bower install --allow-root

# Add android platform
ADD . /opt/app/
ENV GRADLE_USER_HOME /opt/app/gradle
RUN (cd /opt/app && ionic platform add android)
VOLUME /opt/app

#ADD . /opt/app/

WORKDIR /opt/app

EXPOSE 8080

