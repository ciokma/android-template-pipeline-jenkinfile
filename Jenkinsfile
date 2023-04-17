pipeline {
        agent any
        tools {
            gradle "gradle8"
        }
        parameters {
            string(name: 'myInput', description: 'Some pipeline parameters')
        }
        stages {
     
            stage('Gradle Build') {
                steps {
                    script {
                        bat "gradle clean build assembleDebug"
                    }
                }
            }
            stage('Gradle Test') {
            // Ejecuta las pruebas unitarias de Android
                steps {
                    script {
                        bat "gradle test"
                    }
                }
            }
            stage('SonarQube Scan') {
                // Escanea el proyecto con SonarQube
                environment {
                    scannerHome = tool 'SonarScanner'
                }
                steps {
                    script {
                        bat "gradle test"
                    }
                }
            }
        }
        post {
            always {
                archiveArtifacts 'app/build/outputs/**/*.apk'
            }
           
	    }
}
    