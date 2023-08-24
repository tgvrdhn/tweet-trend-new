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
        stage ('Build'){
            steps {
                echo "------Build Started ---------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "------Build Completed ---------"
            }
        }
        stage ("Test") {
            steps {
                echo "-------- unit test started -------"
                sh 'mvn surefire-report:report'
                echo "--------- unit test completed --------"
            }
        }
        stage("SonarQube analysis") {
            environment {
            scannerHome = tool 'tgvrdhn-sonar-scaner'
            }
            steps {
                withSonarQubeEnv('tgvrdhn-sonarqube-server') { 
                sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage("Quality Gate"){
            steps {
                script {
                    timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
                    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                    if (qg.status != 'OK') {
                    error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                    }
                }    
            }
        }
    }
}