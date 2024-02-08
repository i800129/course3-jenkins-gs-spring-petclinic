// TYPE: Declarative Pipeline

pipeline {
    agent any
    
    stages {
        
        stage("build") {
            steps {
                sh "./mvnw package"
            }
        }
        
        stage("Parallel 1") {
            parallel {
                stage("tests A") {
                    steps {
                        sh "echo test set A"
                        sleep 2
                        sleep 3
                    }
                }
                stage("tests B") {
                    steps {
                        sh "echo test set B"
                        sleep 4
                    }
                }
                stage("tests C") {
                    steps {
                        sleep 1
                       // sh "false"
                    }
                }
            }
        }
        
        stage("capture") {
            steps {
                // **/target/*.jar
                archiveArtifacts artifacts: '**/target/*.jar'
                jacoco()
                junit stdioRetention: '', testResults: '**/target/surefire-reports/TEST*.xml'
            }
        }
    }
    
    post {
        always {
             emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}",
                to: 'always@foo.bar',
                recipientProviders: [previous()],
                subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
        }
    }
}    