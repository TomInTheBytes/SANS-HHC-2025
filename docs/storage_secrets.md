# Blob Storage Challenge in the Neighborhood

Difficulty: :material-star::material-star-outline::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Help the Goose Grace near the pond find which Azure Storage account has been misconfigured to allow public blob access by analyzing the export file.

??? quote "Grace"

    HONK!!! HONK!!!!

    The Neighborhood HOA uses Azure storage accounts for various IT operations.

    You've been asked to audit their storage security configuration to ensure no sensitive data is publicly accessible.

    Recent security reports suggest some storage accounts {=might have public blob access enabled, creating potential data exposure risks.=}

## Hints

??? tip "Blob Storage Challenge in the Neighborhood"

    This terminal has built-in hints!

## Solution

```
az help | less
az account show | less
az storage account list | less
az storage account show --name neighborhood2 | less
az storage container list --account-name neighborhood2
az storage blob list --account-name neighborhood2 --container-name public
az storage blob download --account-name neighborhood2 --container-name public --name admin_credentials.txt --file /dev/stdout | less
finish
```

??? success "Solution to question 1"

```
  {
    "id": "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/theneighborhood-rg1/providers/Microsoft.Storage/storageAccounts/neighborhood2",
    "kind": "StorageV2",
    "location": "eastus2",
    "name": "neighborhood2",
    "properties": {
      "accessTier": "Cool",
      "allowBlobPublicAccess": true,
      "encryption": {
        "keySource": "Microsoft.Storage",
        "services": {
          "blob": {
            "enabled": false
          }
        }
      },
      "minimumTlsVersion": "TLS1_0"
    },
    "resourceGroup": "theneighborhood-rg1",
    "sku": {
      "name": "Standard_GRS"
    },
    "tags": {
      "owner": "Admin"
    }
  },
```

## Response

!!! quote "Grace"

    HONK HONK HONK! 'No sensitive data publicly accessible' they claimed. Meanwhile, literally everything was public! Good save, security expert!
