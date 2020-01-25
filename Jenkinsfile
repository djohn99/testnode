pipeline {
    agent any
	
    tools
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
                        sh 'mvn clean package -U -Ddockerfile.skip -DskipTests'
                echo 'Running build automation'
                sh 'mvn clean package -U -DskipTests'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
				googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAA0ee8BzY/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=xBeysykpKmXGcR7l84Wv9nVphU2-OcT3uRRRqUPBiv0%3D', message: "build successful for *${env.JOB_NAME}* ...")               
                  }
               }


//         stage("Deploy to S3"){
//             steps {
//                 script{
//                     try {
//                         if(env.BRANCH_NAME == 'master') {
//                             googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: "Deploying Artefact to S3 *${env.JOB_NAME}* ...")
//                             sh "aws s3 cp ${env.WORKSPACE}/${folderName}/ s3://${prodbucketName}/ --recursive --include '*'"
//                             googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: ":-) Successfully Deployed *${env.JOB_NAME}* to *${prodbucketName}*")
//                         } else {
//                             googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: "Deploying Artefact to S3 *${env.JOB_NAME}* ...")
//                             sh "aws s3 cp ${env.WORKSPACE}/${folderName}/ s3://${stagbucketName}/ --recursive --include '*'"
//                             googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: ":-) Successfully Deployed *${env.JOB_NAME}* to *${stagbucketName}*")
//                         }
                        
//                     }
//                     catch (Exception e) {
//                         currentBuild.result = 'FAILURE'
//                         googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: ":-( Deployment Failed *${env.JOB_NAME}*, {env.BUILD_URL}")
//                         return
//                     } 
//                 }
//             }
//         }
//     }
// }
