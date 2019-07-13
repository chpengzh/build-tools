FROM openjdk:8

#
# Apache Maven build chains
#
ARG MAVEN_URL=http://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.1/binaries/apache-maven-3.6.1-bin.tar.gz
RUN wget ${MAVEN_URL} -O apache-maven.tar.gz && \
    tar zxf apache-maven.tar.gz && \
    rm -rf apache-maven.tar.gz && \
    mv apache-maven* /opt/apache-maven
ENV MAVEN_HOME=/opt/apache-maven
RUN echo 'export PATH=$PATH:$MAVEN_HOME/bin' >> /root/.bashrc

#
# Gradle build chains
#
ARG GRADLE_URL=https://downloads.gradle.org/distributions/gradle-5.3.1-bin.zip
RUN wget ${GRADLE_URL} -O gradle.zip && \
    unzip gradle.zip && \
    rm -rf gradle.zip && \
    mv gradle* /opt/gradle
ENV GRADLE_HOME=/opt/gradle
RUN echo 'export PATH=$PATH:$GRADLE_HOME/bin' >> /root/.bashrc

#
# Patch apt sources
#
ADD sources.list /etc/apt/sources.list
RUN apt-get update

#
# Vim Editor
#
RUN apt-get install -y vim

#
# Linux GCC & build tools
#
RUN apt-get install -y \
    autoconf \
    automake \
    libtool \
    make \
    tar \
    gcc-multilib \
    libaio-dev

RUN mkdir -p /root/share
WORKDIR /root/share

ENTRYPOINT bash