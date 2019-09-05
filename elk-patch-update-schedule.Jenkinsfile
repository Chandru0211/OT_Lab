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

        if (day == "Monday") {  
           parallel 'Dev': { 
                build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'Development'), string(name: 'resourceGroup', value: 'elk-eastus-dev-rg')]
            }, 'QA': { 
                build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'QA'), string(name: 'resourceGroup', value: 'elk-eastus-qa-rg')]
            }
        } else if (day == "Thursday") {
           parallel 'Live': { 
                build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureCloud'), string(name: 'azSubscription', value: 'Live'), string(name: 'resourceGroup', value: 'elk-centralus-prod-rg')]
            }, 'Germany-Live': { 
                build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureGermanCloud'), string(name: 'azSubscription', value: 'Germany-Live'), string(name: 'resourceGroup', value: 'elk-germanycentral-prod-rg')]
            }, 'Security': { 
                build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureGermanCloud'), string(name: 'azSubscription', value: 'Security'), string(name: 'resourceGroup', value: 'wazuh-eastus-security-rg')]
            }, 'Wazuh-Germany-Live': { 
                build job: 'elk-cluster-rolling-patch-update', parameters: [string(name: 'azCloud', value: 'AzureGermanCloud'), string(name: 'azSubscription', value: 'Germany-Live'), string(name: 'resourceGroup', value: 'wazuh-germanycentral-prod-rg')]
            }
        } else {
           println "no job has been scheduled for today"
        }
    }
}
