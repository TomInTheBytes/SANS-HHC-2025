# Spare Key

Difficulty: :material-star::material-star-outline::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Help Goose Barry near the pond identify which identity has been granted excessive Owner permissions at the subscription level, violating the principle of least privilege.

??? quote "Barry"

    You want me to say what exactly? Do I really look like someone who says MOOO?

    The Neighborhood HOA hosts a static website on Azure Storage.

    An admin accidentally uploaded an infrastructure config file that {=contains a long-lived SAS token.=}

    Use Azure CLI to find the leak and report exactly where it lives.

## Hints

??? tip "Spare Key"

    This terminal has built-in hints!

## Solution

??? success "Solution"

    We need to execute the following commands:

    ```sh linenums="1"
    az group list -o table
    az storage account list --resource-group rg-the-neighborhood -o table
    az storage blob service-properties show --account-name neighborhoodhoa --auth-mode login
    az storage container list --account-name neighborhoodhoa --auth-mode login
    az storage blob list --account-name neighborhoodhoa --container-name '$web' --auth-mode login
    az storage blob download --account-name neighborhoodhoa --container-name '$web' --name iac/terraform.tfvars --file /dev/stdout | less
    finish
    ```

    Look for this object:
    ``` json
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

!!! quote "Barry"

    There it is. A SAS token with read-write-delete permissions, publicly accessible. At least someone around here knows how to do a proper security audit.

```

```
