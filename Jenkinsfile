pipeline {

  agent any
  
    environment {
    PROJECT_ID = 'handy-hexagon-318203'
    CLUSTER_NAME = 'cluster'
    LOCATION = 'us-central1-c'
    CREDENTIALS_ID = 'jenkins'
  }


  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/hema1795/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("17hema/sample:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', '17hema') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
