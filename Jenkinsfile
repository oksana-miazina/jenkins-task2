def jobName = env.JOB_NAME

pipeline {
    agent any

    environment {
        APP_PORT = '9090'
    }

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
                                sh "java -jar contact.war"
                              }
                            } catch(Throwable e) {
                                currentBuild.result = "SUCCESS"
                            }
                        }
                    }
                }
                stage('Running Test') {
                    steps {
                      sleep(time: 30, unit: "SECONDS") 
                      sh "mvn -Dtest=RestIT test"
                    }
                }
            }
        }        
    }
}

