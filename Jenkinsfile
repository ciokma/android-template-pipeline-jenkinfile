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
                        bat "sonar-scanner -D sonar.login=sqp_d2511c42af59582d4073339f57de140a2efb224c -D sonar.projectKey=android-template-jenkin -D sonar.host.url=http://localhost:9000/"
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
            stage('Upload to App Center') {
               environment {
                    APPCENTER_API_TOKEN = credentials('app-center-token')
               }
               steps {
                        script {
                            echo "currentBuild.number  ${currentBuild.number}"
                            echo "Subiendo Aplicacion a AppCenter"
                            appCenter apiToken: APPCENTER_API_TOKEN,
                                appName: 'mobile-android-app',
                                branchName: '',
                                buildVersion: "${currentBuild.number}",
                                distributionGroups: 'mobile-android-group',
                                ownerName: 'ciokma',
                                pathToApp: '**/app-debug.apk'                                
                        }
                    
                }
            }
            stage('Upload to Nexus') {
                steps {
                    script {
                        echo "subiendo version numero ${currentBuild.number} a nexus"

                        nexusArtifactUploader
                            credentialsId: 'nexus-credentials',
                            groupId: 'android-mobile',
                            nexusUrl: 'localhost:8081',
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            repository: 'android-mobile-app',
                            version: '01-INITIAL',
                            artifacts: [
                                [artifactId: "android-mobile",
                                classifier: '',
                                file: 'my-service-' +  ${currentBuild.number}  + '.apk',
                                type: 'apk']
                            ]
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
    