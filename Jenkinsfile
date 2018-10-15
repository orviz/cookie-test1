#!/usr/bin/groovy

Library(['github.com/indigo-dc/jenkins-pipeline-library']) _

pipeline {
    parameters {
        string(name: 'appName',
               defaultValue: "cookie-test1",
               description: 'Application name from DEEP-OC repository')
    }
    stages {
        stage('Fetch the repository') {
            steps {
                checkout scm
            }
        }
	
        stage('Style analysis: PEP8') {
            steps {
                ToxEnvRun('pep8')
            }
            post {
                always {
                    WarningsReport('Pep8')
                }
            }
        }

        stage('Style analysis: Pylint') {
            steps {
                ToxEnvRun('pylint')
            }
            post {
                always {
                    WarningsReport('Pylint')
                }
            }
        }

        stage("Re-build DEEP-OC-${params.appName} Docker image") {
            steps {
                job_location = "Pipeline-as-code/DEEP-OC-org/DEEP-OC-${params.appName}"
                JenkinsBuildJob($job_location)
            }
        }
    }
}
