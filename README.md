# mslearn-toy-website-workflow


```bash

githubOrganizationName='bpelikan'
githubRepositoryName='mslearn-toy-website-workflow'

applicationRegistrationDetails=$(az ad app create --display-name 'mslearn-toy-website-workflow')
applicationRegistrationObjectId=$(echo $applicationRegistrationDetails | jq -r '.id')
applicationRegistrationAppId=$(echo $applicationRegistrationDetails | jq -r '.appId')

az ad app federated-credential create \
   --id $applicationRegistrationObjectId \
   --parameters "{\"name\":\"mslearn-toy-website-workflow\",\"issuer\":\"https://token.actions.githubusercontent.com\",\"subject\":\"repo:${githubOrganizationName}/${githubRepositoryName}:ref:refs/heads/main\",\"audiences\":[\"api://AzureADTokenExchange\"]}"


resourceGroupResourceId=$(az group create --name ToyWebsite --location westus --query id --output tsv)

az ad sp create --id $applicationRegistrationObjectId
az role assignment create \
   --assignee $applicationRegistrationAppId \
   --role Contributor \
   --scope $resourceGroupResourceId


echo "AZURE_CLIENT_ID: $($applicationRegistration.AppId)"
echo "AZURE_TENANT_ID: $(az account show --query tenantId --output tsv)"
echo "AZURE_SUBSCRIPTION_ID: $(az account show --query id --output tsv)"


``
