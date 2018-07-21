FROM jenkinsci/blueocean

#Copy plugins file
COPY plugins.txt /usr/share/jenkins/plugins.txt

#Install Plugins
RUN /usr/local/bin/install-plugins.sh $(cat /usr/share/jenkins/plugins.txt | tr '\n' ' ')

# install Maven
#USER root
#RUN yum update -y && yum install -y maven
USER jenkins

