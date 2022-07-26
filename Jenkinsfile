pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/more1/MyProject.git'
            }
        }
        
       stage("SonarQube analysis") {
            steps {
              withSonarQubeEnv('sonar') {
                sh 'mvn -f SampleWebApp/pom.xml clean package sonar:sonar'
              }
            }
          }
      stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true, credentialsId: 'sonar-cred'
            }
        }  
        stage('Build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean package -Dbuild.number=${BUILD_NUMBER}'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'mytomcat', 
                path: '', 
                url: 'http://35.182.65.45:8080/')],
                contextPath: 'mypath', 
                war: '**/*.war'
            }
        }
    }
}
