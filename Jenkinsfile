#!/usr/bin/env groovy

pipeline {
    agent any

    options {
        ansiColor('xterm')
    }
    stages {
        stage('Setup'){
            steps{
                withGradle{
                    sh './gradlew clean'
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

        stage('Integration-Test_sonarqube'){
            steps{
                withSonarQubeEnv(credentialsId: '326817cd-8053-44a1-8b59-15a3b8903c3b', installationName: 'hello-grails') 
                {                    
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
                            reportFiles: 'main.html',
                            reportName: 'HTML Report',
                            reportTitles: 'Coverage Report'
                    ])
                }                                             
            }
        }       
    }
}
