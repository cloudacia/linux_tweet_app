pipeline {
  agent any
    environment {
        registry = "eduarte/linux-tweet-app"
        registryCredential = "dockerhub_id"
        targetRepo = "https://github.com/cloudacia/linux_tweet_app.git"
      }

    stages {
      stage('Cloning GIT repositry') {
        steps {
          git "$targetRepo"
        }
      }

      stage('Building docker image') {
        steps {
          sh "docker build -t $registry:$BUILD_NUMBER ."
        }
      }

      stage('Login dockerhub') {
        steps {
          script {
            withCredentials([
              usernamePassword(
                credentialsId: "$registryCredential",
                usernameVariable: 'username',
                passwordVariable: 'password')
                ]) {
                  sh "docker login -u $username -p $password"
                  }
                }
              }
            }

      stage('Pushing docker image'){
        steps {
          sh "docker push $registry:$BUILD_NUMBER"
          }
        }

      stage('Cleaning up') {
        steps {
          sh "docker rmi $registry:$BUILD_NUMBER"
          }
        }
      }
    }
