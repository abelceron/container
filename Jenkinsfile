pipeline {
  agent any
  stages {
    stage("Stop Running Container") {
    when {
        expression {
            DOCKER_EXIST = sh(returnStdout: true, script: 'echo "$(docker ps -q --filter name=dummy-go)"').trim()
            return  DOCKER_EXIST != ''
                    }
         }
      steps {
        sh "docker stop dummy-go"
      }
    }
    stage("Delete Container") {
    when {
            expression {
                DOCKER_EXIST = sh(returnStdout: true, script: 'echo "$(docker ps -q --filter name=dummy-go)"').trim()
                return  DOCKER_EXIST != ''
                        }
             }
      steps {
        sh "docker rm dummy-go"
      }
    }
    stage('build') {
            steps {
                script{
                    sh '''
                    docker build . -t dummy-go:$dummy-go
                    '''
                    }
                }
            }
    stage("Run Container") {
      steps {
        sh """
          docker run --name dummy-go -d -p 80:8182 dummy-go
        """
      }
    }    
  }
}
