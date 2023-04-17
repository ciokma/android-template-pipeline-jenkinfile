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
                        bat """sonar-scanner \
                        -D sonar.projectVersion=1.0-SNAPSHOT \
                        -D sonar.login=devops \
                        -D sonar.password=devops \
                        -D sonar.projectBaseDir=C:/ProgramData/Jenkins/.jenkins/workspace/android-mobile-pipeline/ \
                            -D sonar.projectKey=android-template-jenkin \
                            -D sonar.sourceEncoding=UTF-8 \
                            -D sonar.language=java \
                            -D sonar.sources=app/src/main \
                            -D sonar.tests=app/src/test \
                            -D sonar.host.url=http://localhost:9000/"""
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
    