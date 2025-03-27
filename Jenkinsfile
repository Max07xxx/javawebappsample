import groovy.json.JsonSlurper

node {
  withEnv([
    'AZURE_SUBSCRIPTION_ID=abf3bd46-d2f6-4985-81c4-458f7b99c79a',
    'AZURE_TENANT_ID=0332699e-0bd8-4393-a324-9915956d4efd'
  ]) {

    stage('init') {
      checkout scm
    }

    stage('build') {
      sh 'mvn clean package'
    }

    stage('deploy') {
      def resourceGroup = 'jenkins-get-started-rg'
      def webAppName = 'siqizhao-webapp'

      withCredentials([usernamePassword(
        credentialsId: 'AzureServicePrincipal',
        passwordVariable: 'AZURE_CLIENT_SECRET',
        usernameVariable: 'AZURE_CLIENT_ID'
      )]) {

        sh '''
          az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
          az account set -s $AZURE_SUBSCRIPTION_ID
          az webapp deploy --resource-group jenkins-get-started-rg --name siqizhao-webapp --src-path target/calculator-1.0.war --type war
          az logout
        '''
      }
    }
  }
}
