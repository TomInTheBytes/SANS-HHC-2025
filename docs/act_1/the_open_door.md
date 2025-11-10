# The Open Door

Difficulty: :material-star::material-star-outline::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Help Goose Lucas in the hotel parking lot find the dangerously misconfigured Network Security Group rule that's allowing unrestricted internet access to sensitive ports like RDP or SSH.

??? quote "Lucas"

    (Spanish mode) Hi... welcome to the Dosis Neighborhood! Nice to meet you!

    Please make sure the towns Azure network is secured properly.

    The Neighborhood HOA uses Azure for their IT infrastructure.

    Audit their network security configuration to ensure production systems aren't exposed to internet attacks.

    They claim all systems are properly protected, but you need to {=verify there are no overly permissive NSG rules.=}

## Hints

??? tip "The Open Door"

    This terminal has built-in hints!

## Solution

??? success "Solution"

```
az group list
az group list -o table
az network nsg list -o table
az network nsg show --name nsg-web-eastus --resource-group theneighborhood-rg1 | less
az network nsg rule list --nsg-name nsg-mgmt-eastus --resource-group theneighborhood-rg2 | less
az network nsg rule list --nsg-name nsg-production-eastus --resource-group theneighborhood-rg1 | less
az network nsg rule show --nsg-name nsg-production-eastus --resource-group theneighborhood-rg1 --name Allow-RDP-From-Internet | less
finish
```

## Response

!!! quote "Lucas"

    Ha! 'Properly protected' they said. More like 'properly exposed to the entire internet'! Good catch, amigo.
