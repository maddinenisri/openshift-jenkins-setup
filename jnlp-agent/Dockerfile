FROM jenkins/slave:3.19-1

MAINTAINER Oleg Nenashev <o.v.nenashev@gmail.com>
LABEL Description="This is a base image, which allows connecting Jenkins agents via JNLP protocols" Vendor="Jenkins project" Version="3.19"

# install jenkins slave
ARG HOME=/home/jenkins
ARG VERSION=3.10

ENV HOME=${HOME}

COPY ./scripts/* /usr/local/bin/

RUN mkdir -p /home/jenkins/.jenkins && \
    chmod -R +x /usr/local/bin && \
    chmod -R a+w /home/jenkins

ENTRYPOINT ["/usr/local/bin/jenkins-slave.sh"]