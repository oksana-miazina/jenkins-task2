pipeline {
    agent any

    environment {
        APP_PORT = '9090'
    }

    def JOB_NAME = env.JOB_NAME

    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Integration Test') {
            parallel {
                stage('Running Application') {
                    agent any
                    options {
                      timeout(time: 60, unit: "SECONDS") 
                    }
                  
                    steps {
                        script {
                            try {
                              dir("target") {
                                sh "contact.war"
                              }
                            } catch(Throwable e) {
                                currentBuild.result = "SUCCESS"
                            }
                        }
                    }
                }
                stage('Running Test') {
                    steps {
                      timeout(time: 30, unit: "SECONDS") 
                      mvn -Dtest=RestIT test
                    }
                }
            }
        }        
    }
}

