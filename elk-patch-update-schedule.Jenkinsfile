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

        if (day == "Thursday") {  
           parallel 'one': { 
                build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'Development'), string(name: 'resourceGroup', value: 'elk-eastus-dev-rg')],
                    propagate: false,
                    wait: true
            }, 'two': { 
                build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'QA'), string(name: 'resourceGroup', value: 'elk-eastus-qa-rg')],
                    propagate: false,
                    wait: true
            }
        } else if (day == "Wednesday") {
           parallel 'one': { 
                build job: 'elk-cluster-rolling-patch-update',
                    parameters: [
                        [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'Live'), string(name: 'resourceGroup', value: 'elk-centralus-prod-rg')]
                    ],
                    propagate: false,
                    wait: true
            }, 'two': { 
                build job: 'elk-cluster-rolling-patch-update', 
                    parameters: [
                        [string(name: 'azCloud', value: 'AzureGermanCloud'), string(name: 'azSubscription', value: 'Germany-Live'), string(name: 'resourceGroup', value: 'elk-germanycentral-prod-rg')]
                    ],
                    propagate: false,
                    wait: true
           }
        } else {
           println "no job has been scheduled for today"
        }
    }
}
