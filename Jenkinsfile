#!groovy

node {

    stage('Clean Workspace') {
        deleteDir()
    }

    stage('Checkout Source') {
        checkout scm
    }

    stage('Trigger elk-cluster-rolling-patch-update job') {

        def day = sh (script: "date +%A", returnStdout:true).toString().trim()

        def time = sh (script: "date +%R", returnStdout:true).toString().trim()

        println day
        println time

        if (day == "Monday" && time == "13:00") {
           build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'Development'), string(name: 'resourceGroup', value: '')]      
        } else if (day == "Monday" && time == "13:05") {
           build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'QA'), string(name: 'resourceGroup', value: '')]
        } else if (day == "Monday" && time == "13:10") {
           build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'Live'), string(name: 'resourceGroup', value: '')]
        } else if (day == "Monday" && time == "13:15") {
           build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureGermanCloud'), string(name: 'azSubscription', value: 'Germany-Live'), string(name: 'resourceGroup', value: '')]
        } else {
           println "no job has been scheduled for today"
        }
    }
}