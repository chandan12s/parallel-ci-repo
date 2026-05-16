pipeline {
    agent any
    tools {
        maven 'mymaven'
    }

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
                        sh 'mvn test jacoco:report'
                    }
                }

                stage('Integration Tests') {
                    steps {
                        sh 'mvn verify jacoco:report'
                    }
                }

                stage('Code Analysis') {
                    steps {
                        sh 'mvn pmd:pmd'
                    }
                }
            }
        }
        stage('Publish Reports & Archive') {
            steps {
                sh 'mvn jacoco:report || true'

                junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                junit allowEmptyResults: true, testResults: 'target/failsafe-reports/*.xml'

                archiveArtifacts artifacts: 'target/*.jar, target/*.war, target/site/jacoco/**, target/surefire-reports/*.xml, target/failsafe-reports/*.xml', fingerprint: true
            }
        }
    }
}
