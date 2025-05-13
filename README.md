# Login to Azure
az login --use-device-code

# Set the correct subscription
az account set --subscription "Azure subscription 1"

# Create a resource group
az group create --name proj6ResourceGroup --location canadacentral

# Create a unique storage account (change name to be globally unique if needed)
az storage account create --name proj6storage$RANDOM --resource-group proj6ResourceGroup --location canadacentral --sku Standard_LRS --kind StorageV2 --access-tier Hot

# Enable static website hosting
az storage blob service-properties update --account-name <your-storage-account-name> --static-website --index-document index.html

# Create project folder and HTML file
mkdir proj6
cd proj6


# Upload HTML file
az storage blob upload --account-name <your-storage-account-name> --container-name \$web --name index.html --file index.html --auth-mode login

# Get the public URL
az storage account show --name <your-storage-account-name> --query "primaryEndpoints.web" --output tsv

# Initialize Git and push to GitHub
git init
git add .
git commit -m "Deployed static website on Azure via CLI"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo-name>.git
git push -u origin main
