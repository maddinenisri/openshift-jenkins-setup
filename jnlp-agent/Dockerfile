FROM webdizz/centos-java8

# install jenkins slave
ARG HOME=/home/jenkins
ARG VERSION=3.10
#ARG JENKINS_USERNAME=admin
#ARG JENKINS_PASSWORD=password

#ENV JENKINS_USERNAME=${JENKINS_USERNAME}
#ENV JENKINS_PASSWORD=${JENKINS_PASSWORD}
ENV JENKINS_SWARM_VERSION 2.0
ENV HOME=${HOME}
#ENV JENKINS_SERVICE_HOST=blueocean-jenkins
#ENV JENKINS_SERVICE_PORT=8080
#ENV JENKINS_SLAVE_SERVICE_PORT=50000
#ENV JENKINS_SLAVE_SERVICE_HOST=blueocean-jenkins-jnlp

COPY ./scripts/* /usr/local/bin/

RUN curl --create-dirs -sSLo /opt/jenkins-slave/bin/swarm-client-$JENKINS_SWARM_VERSION-jar-with-dependencies.jar http://repo.jenkins-ci.org/releases/org/jenkins-ci/plugins/swarm-client/$JENKINS_SWARM_VERSION/swarm-client-$JENKINS_SWARM_VERSION-jar-with-dependencies.jar && \
    curl --create-dirs -sSLo /usr/share/jenkins/slave.jar https://repo.jenkins-ci.org/public/org/jenkins-ci/main/remoting/${VERSION}/remoting-${VERSION}.jar && \
    chmod 755 /opt/jenkins-slave /usr/share/jenkins && \
    chmod 644 /usr/share/jenkins/slave.jar && \
    mkdir -p /home/jenkins/.jenkins && \
    chmod -R +x /usr/local/bin && \
    chmod -R a+w /home/jenkins

ENTRYPOINT ["/usr/local/bin/jenkins-slave.sh"]