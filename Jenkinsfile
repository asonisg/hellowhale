pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/asonisg/hellowhale.git', branch:'master'
      }
    }
      
      stage("data.json") {
          steps {
              sh '''#!/bin/bash
                  echo "[ {"Committer": “AbhishekSoni”, "LatestUpdatedVersion": "150", "Kubernetes": “latest , "Jenkins": “latest”]" > test.json
                  cp -r test.json html/data.json || true
                  '''
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
                      myapp.push(${env.BUILD_NUMBER}")
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
