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
        
        switch(day) {
           case "Monday":
              build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'Development'), string(name: 'resourceGroup', value: 'elk-eastus-dev-rg')]
              break
           case "Tuesday":
              build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'QA'), string(name: 'resourceGroup', value: 'elk-eastus-qa-rg')]
              break
           case "Wednesday":
              build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'Live'), string(name: 'resourceGroup', value: 'elk-centralus-prod-rg')]
              break
           case "Thursday":
              build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureGermanCloud'), string(name: 'azSubscription', value: 'Germany-Live'), string(name: 'resourceGroup', value: 'elk-germanycentral-prod-rg')]
              break
           default:
              println "no job has been triggered"
              break
        }
        
    }
}
