# Version 1.0.0

FROM dockeredcom/debian-stretch-gpg

MAINTAINER Yves Vindevogel (yves.vindevogel@asynchrone.com / yves.vindevogel@dockered.com)

ARG DEBIAN_FRONTEND=noninteractive

ARG JDK_BUILD=8u181-b13
ARG DOWNLOAD_LOCATION=http://download.oracle.com/otn-pub/java/jdk/8u181-b13/96a7b8442fe848ef90c96a2fad6ed6d1/jdk-8u181-linux-x64.tar.gz

# Yves Vindevogel
# 2018-08-15
# Using SDKMan anyway to install the local JDK
# SDKMan has other advantages for installing other SDKs
# So for re-usage, I already have a good base

ARG SDKMAN_DIR=/opt/sdkman

ENV SHELL=/bin/bash

WORKDIR /tmp

# This copies file that sets JAVA_HOME and adds java to the path

COPY ./java.sh /etc/profile.d/java.sh
RUN chmod +x /etc/profile.d/java.sh
RUN echo "[[ -s /etc/profile.d/java.sh ]] && source /etc/profile.d/java.sh" >> /root/.bashrc

# Installation of packages needed (for SDKMAN, except curl)

RUN apt-get update --yes
RUN apt-get upgrade --yes

RUN apt-get install unzip zip curl ca-certificates --yes --no-install-recommends

# Download of the JDK from Oracle

RUN curl --header "Cookie:oraclelicense=accept-securebackup-cookie" --location $DOWNLOAD_LOCATION --output /tmp/jdk.tar.gz

# Untar the JDK in /opt

RUN mkdir -p /opt/oracle-jdk
RUN tar -zxf /tmp/jdk.tar.gz -C /opt/oracle-jdk --strip 1
RUN rm -f /tmp/jdk.tar.gz

# Installing SDKMAN

RUN curl -s "https://get.sdkman.io" | bash

# Installing the JDK using SDKMAN

RUN bash -l -c "sdk install java $JDK_BUILD /opt/oracle-jdk"

# Removing unneeded files. Oracle will forgive me I hope

RUN rm -f /opt/oracle-jdk/javafx-src.zip
RUN rm -f /opt/oracle-jdk/src.zip
RUN rm -f /opt/oracle-jdk/THIRDPARTYLICENSEREADME-JAVAFX.txt
RUN rm -f /opt/oracle-jdk/THIRDPARTYLICENSEREADME.txt

# Standard cleanup

RUN apt-get autoremove --yes
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD [ "/bin/bash" ]
