pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
        }
        stage('Unit Tests - Junit and JaCoCo') {
            steps {
              sh "mvn test"            
            }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
        }
        stage('Docker Build and PUsh') {
            steps {
              withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
                sh "printenv"
                sh 'docker build -t ramavilluri/numericapp:""$GIT_COMMIT"" .' 
                sh 'docker push ramavilluri/numericapp:""$GIT_COMMIT"" .' 
              }         
            }
        }        
    }
}