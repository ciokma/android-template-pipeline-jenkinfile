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
                // Sube el archivo APK a App Center si la calidad del código cumple con el Quality Gate
                when {
                    expression { currentBuild.result == 'SUCCESS' }
                }
                steps {
                    withCredentials([string(credentialsId: 'app-center-token', variable: 'APPCENTER_TOKEN')]) {
                        script {
                            echo "Subiendo Aplicacion a AppCenter"
                            appCenter apiToken: '$APPCENTER_TOKEN', appName: 'mobile-android-app', branchName: '', buildVersion: '', commitHash: '', distributionGroups: 'mobile-android-group', mandatoryUpdate: false, notifyTesters: true, ownerName: 'ciokma', pathToApp: '**/*', pathToDebugSymbols: '', pathToReleaseNotes: '', releaseNotes: ''
                        }
                    }
                }
            }
            stage('Upload to App Center 2') {
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
        }
        post {
            always {
                archiveArtifacts 'app/build/outputs/**/*.apk'
            }
           
	    }
}
    