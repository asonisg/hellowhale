pipeline {

  agent any

  stages {
  
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("asonisg/hellowhale:${env.BUILD_NUMBER}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                      myapp.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }

    
    stage('Trigger Deloy Job') {
      steps {
          build job: 'app-deploy', parameters: [string(name: 'APP_VERSION', value: env.BUILD_NUMBER)]
      }
    }

  }

}
