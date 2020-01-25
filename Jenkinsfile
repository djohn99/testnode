def folderName = "dist/br-form-capture-portal"
#def stagbucketName = "test.djohn.com"
def prodbucketName = "staging.djohn.com"
pipeline {
    agent any
    stages {
        stage ('Initialize') {
            steps {
                googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: "Initializing build process for *${env.JOB_NAME}* ...")
            }   
        }
        stage ("Installing Dependencies and Building"){
            steps {
                script{
                    try {
                        if(env.BRANCH_NAME == 'master') {
                           googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: "Build Initialized, Installing & Building Prod *${env.JOB_NAME}* ...")
                            sh '''
                                npm set registry http://41.223.47.82:4873
                                npm install
                                rm -rf dist && rm -rf dist-green
                                node --max_old_space_size=8048 /opt/node_modules/npm/node_modules/@angular/cli/bin/ng build -c=production --output-path dist && node --max_old_space_size=8048 /opt/node_modules/npm/node_modules/@angular/cli/bin/ng build -c=production-green --output-path dist-green
                                ''' 
                            googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: "Build Successful *${env.JOB_NAME}*") 
                        } else {
                            googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: "Build Initialized, Installing & Building Staging *${env.JOB_NAME}* ...")
                            sh '''
                                npm set registry http://41.223.47.82:4873
                                npm install
                                rm -rf dist && rm -rf dist-green
                                node --max_old_space_size=8048 /opt/node_modules/npm/node_modules/@angular/cli/bin/ng build -c=staging
                                ''' 
                            googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: "Build Successful *${env.JOB_NAME}*, ${env.BUILD_URL}")
                        }
                    }
                    catch (Exception e) {
                        googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: ":-( Build Failed *${env.JOB_NAME}*, ${env.BUILD_URL}")
                        currentBuild.result = 'ABORTED'
                        return
                    } 
                }
            }    
        }

        stage("Deploy to S3"){
            steps {
                script{
                    try {
                        if(env.BRANCH_NAME == 'master') {
                            googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: "Deploying Artefact to S3 *${env.JOB_NAME}* ...")
                            sh "aws s3 cp ${env.WORKSPACE}/${folderName}/ s3://${prodbucketName}/ --recursive --include '*'"
                            googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: ":-) Successfully Deployed *${env.JOB_NAME}* to *${prodbucketName}*")
                        } else {
                            googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: "Deploying Artefact to S3 *${env.JOB_NAME}* ...")
                            sh "aws s3 cp ${env.WORKSPACE}/${folderName}/ s3://${stagbucketName}/ --recursive --include '*'"
                            googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: ":-) Successfully Deployed *${env.JOB_NAME}* to *${stagbucketName}*")
                        }
                        
                    }
                    catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        googlechatnotification (url: 'https://chat.googleapis.com/v1/spaces/AAAAuvIDdoQ/messages?key=AIzaSyDdI0hCZtE6vySjMm-WEfRq3CPzqKqqsHI&token=I7VzE4LUOmrrhTt_jmDoiAJwbAI0z70DqfiJ6eOBvUY%3D', message: ":-( Deployment Failed *${env.JOB_NAME}*, {env.BUILD_URL}")
                        return
                    } 
                }
            }
        }
    }
}
