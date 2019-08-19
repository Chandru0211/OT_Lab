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

        println day

        if (day == "Monday") {
           build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'Development'), string(name: 'resourceGroup', value: '')]      
        } else if (day == "Tuesday") {
           build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'QA'), string(name: 'resourceGroup', value: '')]
        } else if (day == "Wednesday") {
           build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'Live'), string(name: 'resourceGroup', value: '')]
        } else if (day == "Thursday") {
           build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureGermanCloud'), string(name: 'azSubscription', value: 'Germany-Live'), string(name: 'resourceGroup', value: '')]
        } else {
           println "no job has been scheduled for today"
        }
    }
}