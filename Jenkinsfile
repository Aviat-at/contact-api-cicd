pipeline {
  agent any

  environment {
    AZURE_CLIENT_ID       = credentials('azure-client-id')
    AZURE_CLIENT_SECRET   = credentials('azure-client-secret')
    AZURE_TENANT_ID       = credentials('azure-tenant-id')
    AZURE_SUBSCRIPTION_ID = credentials('azure-subscription-id')

    TF_VAR_resource_group_name   = credentials('tf-resource-group')
    TF_VAR_location              = credentials('tf-location')
    TF_VAR_function_app_name     = credentials('tf-function-name')
    TF_VAR_storage_account_name  = credentials('tf-storage-name')
    TF_VAR_subscription_id       = credentials('azure-subscription-id')
  }

  stages {
    stage('Checkout') {
      steps {
        git credentialsId: 'github-pat', branch: 'main', url: 'https://github.com/Aviat-at/8935920-ci-cd-Assignment3'
      }
    }

    stage('Azure Login') {
      steps {
        sh '''
        az login --service-principal \
          -u "$AZURE_CLIENT_ID" \
          -p "$AZURE_CLIENT_SECRET" \
          --tenant "$AZURE_TENANT_ID"

        az account set --subscription "$AZURE_SUBSCRIPTION_ID"
        '''
      }
    }


   stage('Deploy Function') {
  steps {
    sh '''
    # Create zip from existing directory structure
    zip -r app.zip contact-api/ host.json requirements.txt

    # Deploy to Azure Function
    az functionapp deployment source config-zip \
      --resource-group "$TF_VAR_resource_group_name" \
      --name "$TF_VAR_function_app_name" \
      --src app.zip
    '''
  }
}
  }
}
