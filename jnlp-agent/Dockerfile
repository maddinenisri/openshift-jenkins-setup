FROM jenkins/slave:3.19-1

MAINTAINER Oleg Nenashev <o.v.nenashev@gmail.com>
LABEL Description="This is a base image, which allows connecting Jenkins agents via JNLP protocols" Vendor="Jenkins project" Version="3.19"

COPY ./scripts/* /usr/local/bin/

ENTRYPOINT ["/usr/local/bin/jenkins-slave.sh"]