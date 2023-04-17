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
           stage('SonarQube Analysis') {
                def scannerHome = tool 'SonarQube'
                withSonarQubeEnv('SonarQube') {
                bat """/var/lib/jenkins/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQube/bin/sonar-scanner \
                -D sonar.projectVersion=1.0-SNAPSHOT \
                -D sonar.login=admin \
                -D sonar.password=admin \
                -D sonar.projectBaseDir=/var/lib/jenkins/workspace/jenkins-sonar/ \
                    -D sonar.projectKey=android-template-jenkin \
                    -D sonar.sourceEncoding=UTF-8 \
                    -D sonar.language=java \
                    -D sonar.sources=app/src/main \
                    -D sonar.tests=app/src/test \
                    -D sonar.host.url=http://localhost:9000/"""
                    }
            }
        }
        post {
            always {
                archiveArtifacts 'app/build/outputs/**/*.apk'
            }
           
	    }
}
    