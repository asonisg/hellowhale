pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/asonisg/hellowhale.git', branch:'master'
      }
    }
    
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
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Trigger Deloy Job') {
      steps {
          build job: 'app-deploy', parameters: [string(name: 'BUILD_NUMBER', value: env.BUILD_NUMBER)]
      }
    }

  }

}
