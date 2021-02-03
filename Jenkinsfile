#!/usr/bin/env groovy

pipeline {
    agent any
    tools {
        jdk 'openJDK-15.0.02'
    }
    options {
        ansiColor('xterm')
    }
    stages {
        stage('Setup'){
            steps{
                git url:'http://10.250.8.1:8929/root/hello-grails.git',branch:'master' 
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
	}

        stage('Build') {
            steps {
                echo "Build ...."
            //    withGradle {
              //      sh './gradlew assemble'
               // }
           // }
            //post{
              //  success{
                //    archiveArtifacts 'build/libs/*.jar'
                  //  echo ".Jar Guardados en build/libs"
               // }

            }
        }
        stage('Deploy') {
            steps {
                echo "Deploy...."
            }
        }
    }
}