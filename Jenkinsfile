#!groovy

import groovy.json.JsonOutput

stage('Run') {
    node {

        currentBuild.result = "SUCCESS"

        try {

            checkout scm

            sh './gradlew run'

        } catch (err) {
            currentBuild.result = "FAILURE"

            def build = "${env.JOB_NAME} - #${env.BUILD_NUMBER}".toString()
            def notifierEndpoint = "${env.NOTIFIER_ENDPOINT}".toString()
            def emailAddress = "${env.EMAIL}".toString()

            def email = [to: emailAddress, from: emailAddress, subject: "$build failed!", body: "${env.JOB_NAME} failed! See ${env.BUILD_URL} for details."]

            emailext body: email.body, recipientProviders: [[$class: 'DevelopersRecipientProvider']], subject: email.subject, to: "${env.EMAIL}"

            throw err
        }
    }
}
