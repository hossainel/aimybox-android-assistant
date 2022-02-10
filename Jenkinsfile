#!groovy

def slackSendFunc(){
    env.message = "Job <https://jenkins.just-ai.com/job/${APP}/job/${BRANCH_NAME}/${env.BUILD_NUMBER}/|${APP}> » ${branch} #${env.BUILD_NUMBER}: ${env.status}"
    slackSend channel: 'mobile-dev-builds', color: "${env.color}", message: "${env.message}"
}

pipeline {
    agent {
        label 'slave01-onprem'
    }
    triggers {
        pollSCM '*/5 * * * *'
    }
    environment {
        APP = "aimybox-android-assistant"
        BRANCH = sh(script: "echo ${BRANCH_NAME} | tr '[:upper:]' '[:lower:]'", returnStdout: true).trim()
        ANDROID_SDK_ROOT = "/opt/android-sdk"
    }
    options {
        buildDiscarder(logRotator(artifactDaysToKeepStr: '30', artifactNumToKeepStr: '30', daysToKeepStr: '30', numToKeepStr: '30'))
        disableConcurrentBuilds()
    }
    stages {
        stage('build gradle'){
            steps {
                script {
                    sh "chmod +x ./gradlew && ./gradlew build"
//                     env.apk_file_path = sh(script: "ls ${env.WORKSPACE}/app/build/outputs/apk/release/*.apk", returnStdout: true).trim()
                }
            }
        }
//         stage('deploy artifact'){
//             steps {
//                 script {
//                     def version_name = sh(script: "cat app/build.gradle.kts | grep versionName | tr -d ' ' | sed -n '1p' | cut -f2 -d '=' | tr -d '\n'", returnStdout: true)
//                     def version_code = sh(script: "cat app/build.gradle.kts | grep versionCode | tr -d ' ' | sed -n '1p' | cut -f2 -d '=' | tr -d '\n'", returnStdout: true)
//
//                     withCredentials([usernameColonPassword(credentialsId: 'nx_jenkins', variable: 'nexus_creds')]) {
//                         def REPO_URL = "https://nx01-infra-htz.lab.just-ai.com/repository/raw-hosted/justai/mobile_dev/android/${APP}"
//
//                         echo "uploading artifact..."
//                         sh("""curl -f -sS --user $nexus_creds --upload-file "${env.apk_file_path}" "${REPO_URL}/${APP}-${BRANCH}-version_${version_name}_${version_code}.apk" &&
//                             echo "uploaded successfully" ||
//                             echo "uploading is failed && exit 1"
//                         """)
//                     }
//                 }
//             }
//         }
    }
//     post {
//         success {
//             script {
//                 env.color = 'good'
//                 env.status = 'SUCCESS'
//                 slackSendFunc()
//             }
//         }
//         failure {
//             script {
//                 env.color = 'danger'
//                 env.status = 'FAILED'
//                 slackSendFunc()
//             }
//         }
//         always {
//             cleanWs()
//         }
//     }
}