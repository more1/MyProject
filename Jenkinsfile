pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/kendevops4/MyProject.git'
            }
        }
        
        stage("SonarQube analysis") {
            steps {
              withSonarQubeEnv('sonar') {
                sh 'mvn -f SampleWebApp/pom.xml clean package sonar:sonar'
              }
            }
          }
        
        stage('Build with Maven') {
            steps {
                sh 'cd SampleWebApp && mvn package'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'mytomcat', 
                path: '', 
                url: 'http://3.129.248.226:8080')],
                contextPath: 'mypath', 
                war: '**/*.war'
            }
        }
    }
}
