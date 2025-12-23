# Owner

Difficulty: :material-star::material-star-outline::material-star-outline::material-star-outline::material-star-outline:

## Objective

!!! question "Task description"

    Help Goose James near the park discover the accidentally leaked SAS token in a public JavaScript file and determine what Azure Storage resource it exposes and what permissions it grants.

??? quote "James"

    CLUCK CLU... I think I might be losing my mind. All the elves are gone and I'm still hearing voices.

    The Neighborhood HOA uses Azure for their IT infrastructure.

    The Neighborhood network admins use RBAC fo access control.

    Your task is to audit their RBAC configuration to ensure they're following security best practices.

    They claim all elevated access uses PIM, but you need to verify there are no permanently assigned Owner roles.

## Hints

??? tip "Owner"

    This terminal has built-in hints!

## Solution

??? success "Solution"

    We need to execute the following commands:

    ``` sh linenums="1"
    az account list --query "[].name"
    az account list --query "[?state=='Enabled'].{Name:name, ID:id}"
    az role assignment list --scope "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64" --query [?roleDefinition=='Owner']
    az role assignment list --scope "/subscriptions/065cc24a-077e-40b9-b666-2f4dd9f3a617" --query [?roleDefinition=='Owner']
    az ad member list --group "6b982f2f-78a0-44a8-b915-79240b2b4796"
    az ad member list --group "631ebd3f-39f9-4492-a780-aef2aec8c94e"
    finish
    ```

## Images

![answer](../media/owner/owner_1)
/// caption
Challenge terminal.
///

## Response

!!! quote "James"

    You found the permanent assignments! CLUCK! See, I'm not crazy - the security really WAS misconfigured. Now maybe I can finally get some peace and quiet...

```

```
