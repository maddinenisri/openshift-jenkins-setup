podTemplate(label: 'maven-selenium-docker', containers: [
  containerTemplate(name: 'docker', image: 'docker-registry.default.svc:5000/devops/oc-docker:latest', ttyEnabled: true, command: 'cat'),
  containerTemplate(name: 'maven-firefox', image: 'maven:3.3.9-jdk-8-alpine', ttyEnabled: true, command: 'cat'),
  containerTemplate(name: 'maven-chrome', image: 'maven:3.3.9-jdk-8-alpine', ttyEnabled: true, command: 'cat'),
  containerTemplate(name: 'selenium-hub', image: 'selenium/hub:3.4.0'),
  containerTemplate(name: 'selenium-chrome', image: 'selenium/node-chrome:3.4.0', envVars: [
    containerEnvVar(key: 'HUB_PORT_4444_TCP_ADDR', value: 'localhost'),
    containerEnvVar(key: 'HUB_PORT_4444_TCP_PORT', value: '4444'),
    containerEnvVar(key: 'DISPLAY', value: ':99.0'),
    containerEnvVar(key: 'SE_OPTS', value: '-port 5556'),
  ]),
  containerTemplate(name: 'selenium-firefox', image: 'selenium/node-firefox:3.4.0', envVars: [
    containerEnvVar(key: 'HUB_PORT_4444_TCP_ADDR', value: 'localhost'),
    containerEnvVar(key: 'HUB_PORT_4444_TCP_PORT', value: '4444'),
    containerEnvVar(key: 'DISPLAY', value: ':98.0'),
    containerEnvVar(key: 'SE_OPTS', value: '-port 5557'),
  ])],
  volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')]) {

  node('maven-selenium-docker') {
    stage('Checkout') {
      git 'https://github.com/carlossg/selenium-example.git'
      parallel (
        firefox: {
          container('maven-firefox') {
            stage('Test firefox') {
              sh 'mvn -B clean test -Dselenium.browser=firefox -Dsurefire.rerunFailingTestsCount=5 -Dsleep=0'
            }
          }
        },
        chrome: {
          container('maven-chrome') {
            stage('Test chrome') {
              sh 'mvn -B clean test -Dselenium.browser=chrome -Dsurefire.rerunFailingTestsCount=5 -Dsleep=0'
            }
          }
        }
      )
    }

    stage('Logs') {
      containerLog('selenium-chrome')
      containerLog('selenium-firefox')
    }


    def image = "jnlp-slave:1.0.${env.BUILD_NUMBER}"
    stage('Build Docker image') {
      git 'https://github.com/jenkinsci/docker-jnlp-slave.git'
      container('docker') {
        sh "docker build -t ${image} ."
        sh "docker tag ${image} docker-registry.default.svc:5000/devops/${image}"

      }
    }

    stage('Approve') {
        container('docker') {
            try {
                withCredentials([string(credentialsId: 'oc_admin_login_time_based_token', variable: 'token')]) {
                    sh "docker login -p $token -e unused -u unused && docker push docker-registry.default.svc:5000/devops/${image}"
                }
            }
            catch(failed) {
                timeout(time: 30, unit: 'MINUTES') { // Upload failed make sure manual upload from slave
                    userInput = input(
                        id: 'Proceed1', message: 'Is Dokcer image upload successful?', parameters: [
                        [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please confirm you agree with this']
                    ])
                }
            }
        }
    }
  }
}