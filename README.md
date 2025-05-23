# MyMvcApp Contact Database Application

## Overview
This repository contains the `MyMvcApp`, an ASP.NET Core MVC application designed to manage a contact database. The application is deployed to Azure using GitHub Actions and can be easily set up with an ARM template for infrastructure provisioning.

## Features
- ASP.NET Core MVC architecture.
- CRUD operations for managing contacts.
- Deployment to Azure App Service using GitHub Actions.
- Infrastructure provisioning using an ARM template.

## Deployment Guide

### Prerequisites
1. **Azure Account**: Ensure you have an active Azure subscription.
2. **Azure Web App**: Create an Azure Web App (e.g., `my-mvcapp-demo-001`).
3. **GitHub Secrets**:
   - `AZURE_WEBAPP_NAME`: Name of the Azure Web App.
   - `AZURE_WEBAPP_PUBLISH_PROFILE`: Publish profile of the Azure Web App.

### GitHub Actions Workflow
The repository includes a GitHub Actions workflow file located at `.github/workflows/azure-webapp.yml`. This workflow automates the build, publish, and deployment process.

#### Workflow Steps
1. **Build**: Restores dependencies, builds the project, and publishes the output.
2. **Upload Artifact**: Uploads the build artifacts for deployment.
3. **Deploy**: Downloads the artifacts and deploys them to the Azure Web App using the publish profile.

#### Key Configuration
```yaml
with:
  app-name: ${{ secrets.AZURE_WEBAPP_NAME }}
  slot-name: 'Production'
  package: ./
  publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
```

### ARM Template for Infrastructure
You can use an Azure Resource Manager (ARM) template to provision the necessary infrastructure for the application. Below is an example template:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "type": "Microsoft.Web/sites",
      "apiVersion": "2021-02-01",
      "name": "my-mvcapp-demo-001",
      "location": "[resourceGroup().location]",
      "properties": {
        "serverFarmId": "[resourceId('Microsoft.Web/serverfarms', 'my-app-service-plan')]"
      }
    },
    {
      "type": "Microsoft.Web/serverfarms",
      "apiVersion": "2021-02-01",
      "name": "my-app-service-plan",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "F1",
        "tier": "Free"
      },
      "properties": {
        "reserved": false
      }
    }
  ]
}
```

### Steps to Deploy with ARM Template
1. Save the ARM template as `deploy.json`.
2. Use the Azure CLI or Azure Portal to deploy the template:
   ```bash
   az deployment group create --resource-group <resource-group-name> --template-file deploy.json
   ```

### Running the Application
Once deployed, the application will be accessible at `http://<your-app-name>.azurewebsites.net`.

## Contributing
Feel free to fork this repository and submit pull requests for improvements or bug fixes.

## License
This project is licensed under the MIT License.
