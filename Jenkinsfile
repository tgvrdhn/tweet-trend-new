pipeline {
    agent {
        node {
            label 'maven'
        }
    }
    stages {
        stage ('code-clone'){
            steps {
                git branch: 'main', url: 'https://github.com/tgvrdhn/tweet-trend-new.git'
            }
        }
    }
}