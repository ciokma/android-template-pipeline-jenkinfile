pipeline {
        agent any
        tools {
            gradle "gradle8"
        }
        parameters {
            string(name: 'myInput', description: 'Some pipeline parameters')
        }
        stages {
            stage('Stage one') {
                steps {
                    script {
                        echo "Parameter from template creation: " 
                    }
                }
            }
            stage('Stage two') {
                steps {
                    script {
                        echo "Job input parameter: "
                    }
                }
            }
            stage('Gradle') {
                steps {
                    script {
                        bat "gradle clean build assembleDebug"
                    }
                }
            }
        }
        post {
            always {
                archiveArtifacts 'app/build/outputs/**/*.apk'
                publishHTML target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'app/build/reports',
                    reportFiles: 'index.html',
                    reportName: 'Test Report'
                ]
            }
           
	    }
}
    