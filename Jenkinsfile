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
                configFileProvider(
                        [configFile(
                                fileId: 'hello-grails-gradle.properties'
                        )]) {
                }
                withGradle{
                    sh 'gradle'
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

        stage('Interation-Test'){
            steps{
                withGradle{
                    sh './gradlew integrationTest'
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
                            reportFiles: 'index.html',
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