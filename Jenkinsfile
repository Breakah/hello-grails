#!/usr/bin/env groovy

pipeline {
    agent any

    options {
        ansiColor('xterm')
    }
    stages {
        stage('Setup'){
            steps{
                git url:'http://10.250.8.1:8929/root/hello-grails.git',branch:'master'
                withGradle{
                    sh './gradlew clean'
                    //sh 'gradle'
                }
	        }
        }
        stage('Test-Unit'){
            steps{
                withGradle{
                    sh './gradlew test'
                }
            }
            post{
                always{
                    junit 'build/test-results/test/TEST-*.xml'  
                }
            }          
        }

        stage('Integration-Test'){
            steps{
                withGradle{
                    configFileProvider(
                    [configFile(
                            fileId: 'hello-grails-gradle.properties',
                            targetLocation: 'gradle.properties'
                    )]) {
                        sh './gradlew integrationTest'
                    }
                }
            }
            post{
                always{
                    junit 'build/test-results/integrationTest/TEST-*.xml'
                }
            }
        }
        stage('Codenarc-Test'){
            steps{
                withGradle{
                    sh './gradlew codenarcTest'
                }
            }
            post{
                always{
                    publishHTML([
                            allowMissing: false,
                            alwaysLinkToLastBuild: false,
                            keepAll: false,
                            reportDir: 'build/reports/codenarc/',
                            reportFiles: 'main.html',
                            reportName: 'HTML Report',
                            reportTitles: 'Coverage Report'
                    ])
                }
                                             }
        }

        stage('Build') {
            steps {
                echo "Build ...."
                withGradle {
                  sh './gradlew assemble'
                }
            }
            //post{
              //  success{
                    //archiveArtifacts 'build/libs/*.jar'
               //}
            //}
        }
        stage('Deploy') {
            steps {
                echo "Deploy...."
            }
        }
    }
}