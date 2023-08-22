pipeline {
    agent {
        node {
            label 'maven'
        }
    }

environment {
    PATH = "/opt/apache-maven-3.9.4/bin:$PATH"
}
    stages {
        stage ('build'){
            steps {
                sh 'mvn clean deploy'
            }
        }
        stage('SonarQube analysis') {
            environment {
            scannerHome = tool 'tgvrdhn-sonar-scaner'
            }
            steps {
                withSonarQubeEnv('tgvrdhn-sonarqube-server') { 
                sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}