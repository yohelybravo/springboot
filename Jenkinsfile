pipeline {
  agent any
  stages {
    stage('Build') {
      post {
        success {
          archiveArtifacts 'complete/target/*.jar'
        }

      }
      steps {
        echo 'Building..'
        git 'https://github.com/spring-guides/gs-spring-boot.git'
        sh 'mvn clean package -f complete/pom.xml'
      }
    }

    stage('Test') {
      steps {
        echo 'Testing..'
        sh 'mvn test -f complete/pom.xml'
      }
    }

    stage('Run') {
      steps {
        echo 'Deploying..'
        script {
          withEnv(['JENKINS_NODE_COOKIE=dontkill']){
            sh "nohup mvn spring-boot:run -f complete/pom.xml &"
          }
        }

      }
    }

    stage('Testt2') {
      steps {
        echo 'Aqui en Test2'
      }
    }

  }
  tools {
    maven 'MAVEN_HOME'
  }
}