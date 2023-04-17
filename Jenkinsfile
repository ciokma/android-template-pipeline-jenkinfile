pipeline {
        agent any
        tools {
            gradle "gradle8"
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
            
            stage('SonarQube Analysis') {
                steps {
                     script {
                        bat "sonar-scanner -D sonar.login=sqp_846d18024c9a0c129203055c7ac76218181e4309 -D sonar.projectKey=android-template-jenkin -D sonar.host.url=http://localhost:9000/"
                     }
                }
               
            }
            stage('Firmar APK') {
                // Firma el archivo APK para su uso en producción
                steps {
                    script {
                        echo "Firmando APK"
                        bat "gradle assembleRelease --stacktrace --no-daemon"
                     }
                }
            }
            stage('Quality Gate') {
                // Evalúa el resultado del análisis de SonarQube para el Quality Gate
                steps {
                    script {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
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
    