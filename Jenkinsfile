pipeline {
    agent any
	environment {
        //variable for image name
        PROJECT = "test"
        APP = "jHipster"
	}
    tools{
        maven 'mvn339'
        jdk 'jdk8'
    }  
    stages {
        stage ('Initialize') {
            steps {
                deleteDir()
                googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAA0ee8BzY/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=xBeysykpKmXGcR7l84Wv9nVphU2-OcT3uRRRqUPBiv0%3D', message: "Initializing build process for *${env.JOB_NAME}* ...")
            }   
        }
        stage ('Clean WorkSpace') {
            steps {
                checkout scm
            }  
        }
        stage ('Build') {
            steps {
                script {
                            env.OLD_VERSION = sh(returnStdout: true, script: 'mvn help:evaluate -Dexpression=project.version -q -DforceStdout')
                            env.VERSION = env.OLD_VERSION + '-' + env.BUILD_NUMBER
                        }
                        sh 'mvn versions:set -DnewVersion=$VERSION versions:commit'
                echo 'Running build automation'
                sh 'mvn clean package -U -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
				googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAA0ee8BzY/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=xBeysykpKmXGcR7l84Wv9nVphU2-OcT3uRRRqUPBiv0%3D', message: "build successful for *${env.JOB_NAME}* ...")               
                  }
               }
            }
        }
