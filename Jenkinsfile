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
        stage('Test-Unit_sonarqube'){
            steps{
                withSonarQubeEnv(credentialsId: '326817cd-8053-44a1-8b59-15a3b8903c3b', installationName: 'hello_grails') 
                {                     
                    sh './gradlew test'
                }
            }
            post{
                always{
                    junit 'build/test-results/test/TEST-*.xml'  
                }
            }          
        }

        stage('test_sonarqube') {   
            steps {
                withSonarQubeEnv(credentialsId: '326817cd-8053-44a1-8b59-15a3b8903c3b', installationName: 'hello_grails') 
                {                    
                sh './gradlew sonarqube'           
                }                            
            }            
            post {                
            always {                    
                recordIssues enabledForFailure: true, tool: spotBugs(pattern: 'build/reports/spotbugs/*.xml')                   
                recordIssues enabledForFailure: true, tool: pmdParser(pattern: 'build/reports/pmd/*.xml')                
                }            
            }        
        }



/*
        stage('Integration-Test'){
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
        }*/
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
