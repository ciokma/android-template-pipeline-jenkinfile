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
                        gradle "--version"
                        gradle "clean"
                        gradle "build"
                        gradle "assembleDebug"
                    }
                }
            }
        }
    }