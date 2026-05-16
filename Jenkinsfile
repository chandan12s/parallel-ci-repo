pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/chandan12s/parallel-ci-repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Parallel Tests') {
            parallel {

                stage('Unit Tests') {
                    steps {
                        sh 'mvn test'
                    }
                }

                stage('Integration Tests') {
                    steps {
                        sh 'mvn verify'
                    }
                }

                stage('Code Analysis') {
                    steps {
                        sh 'mvn pmd:pmd'
                    }
                }
            }
        }
    }
}
