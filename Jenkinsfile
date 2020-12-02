pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "MAVEN_HOME"
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                // Get some code from a GitHub repository
                git 'https://github.com/spring-guides/gs-spring-boot.git'

                // Run Maven on a Unix agent.
                sh "mvn clean package -f complete/pom.xml"
                //bat "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    //junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'complete/target/*.jar'
                }
            }
        }
        stage('Test'){
            steps{
                echo 'Testing..'
                sh "mvn test -f complete/pom.xml"
            }
        }
        stage('Run'){
            steps{
                echo 'Deploying..'
                //sh "mvn spring-boot:run -f complete/pom.xml"
                script{
                    withEnv(['JENKINS_NODE_COOKIE=dontkill']){
                       sh "nohup mvn spring-boot:run -f complete/pom.xml &" 
                    }
                }
            }
        }
    }
}
