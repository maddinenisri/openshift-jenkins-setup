FROM jenkinsci/blueocean

#Install Plugins
RUN /usr/local/bin/install-plugins.sh kubernetes
RUN /usr/local/bin/install-plugins.sh github
RUN /usr/local/bin/install-plugins.sh github-organization-folder

# install Maven
#USER root
#RUN yum update -y && yum install -y maven
USER jenkins

