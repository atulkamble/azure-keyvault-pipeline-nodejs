Here’s a full working example that demonstrates:

✅ **Creating and using Azure Key Vault**  
✅ **Referencing secrets from Key Vault in Azure Pipelines**  
✅ **Deploying a sample Node.js app securely using secrets**

---

## 🔐 **Azure Key Vault Setup (CLI)**

### Step 1: Create Key Vault & Store Secrets

```bash
az group create --name myResourceGroup --location eastus

az keyvault create --name MyKeyVault123 --resource-group myResourceGroup --location eastus

az keyvault secret set --vault-name MyKeyVault123 --name "DbPassword" --value "MyS3cretP@ssw0rd"
```

---

## 🗂️ **Sample Project Structure**
```
azure-keyvault-pipeline-demo/
├── app/
│   └── index.js
├── azure-pipelines.yml
├── keyvault-setup.md
└── README.md
```

---

## 📦 **Sample Node.js App (app/index.js)**

```js
const http = require('http');

const server = http.createServer((req, res) => {
  const dbPassword = process.env.DB_PASSWORD || 'default';
  res.end(`Secret used in pipeline is: ${dbPassword}`);
});

server.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

---

## ⚙️ **Azure Pipeline with Key Vault Integration (azure-pipelines.yml)**

```yaml
trigger:
- main

variables:
  - group: my-kv-variable-group  # Variable group connected to Key Vault

pool:
  vmImage: 'ubuntu-latest'

steps:
- task: NodeTool@0
  inputs:
    versionSpec: '18.x'
  displayName: 'Install Node.js'

- script: |
    npm install
    node app/index.js &
    sleep 5
    curl http://localhost:3000
  env:
    DB_PASSWORD: $(DbPassword)
  displayName: 'Run app using Key Vault secret'
```

---

## 🔑 **Link Azure Pipeline to Key Vault Secret (via Variable Group)**

### In Azure DevOps:
1. Go to **Pipelines > Library**  
2. Create a **Variable Group** (e.g., `my-kv-variable-group`)  
3. Enable **"Link secrets from an Azure Key Vault as variables"**
4. Authorize Key Vault access.
5. Select secrets like `DbPassword`.

---

## 📘 **`keyvault-setup.md` (Documentation)**

```markdown
# Azure Key Vault + DevOps Pipeline Integration

This project demonstrates how to:
- Securely store secrets in Azure Key Vault
- Reference those secrets in Azure DevOps Pipelines
- Use those secrets inside a Node.js app

## Steps:
1. Set up Key Vault with secrets
2. Create a Variable Group in Azure DevOps linked to Key Vault
3. Use secrets securely in the pipeline YAML

> Always avoid hardcoding secrets in your code or pipelines.
```

---

## 🏁 Repo Name Suggestion:
`azure-keyvault-pipeline-nodejs-demo`

Would you like me to:
- Zip this project as a downloadable file?  
- Create a GitHub-ready `README.md`?  
- Add Terraform/Bicep code to automate Key Vault setup?
