---
ms.topic: include
ms.service: azure-devops-artifacts
ms.author: rabououn
author: ramiMSFT
ms.date: 09/11/2023
---

::: moniker range="<=azure-devops"

1. Sign in to your Azure DevOps organization, and then navigate to your project.

1. Select **Artifacts**, and then select **Connect to feed**.

    :::image type="content" source="../../npm/media/npm-scopes-connect-to-feed.png" alt-text="A screenshot showing how to connect to a feed.":::

1. Select **npm**, and then select **Other**.

1. Add a `.npmrc` file in the same directory as your package.json, and paste the following snippet into your file.

    - **Organization-scoped feed**:
    
        ```Command
        registry=https://pkgs.dev.azure.com/<ORGANIZATION_NAME/_packaging/<FEED_NAME>/npm/registry/
        
        always-auth=true
        ```

    - **Project-scoped feed**:

        ```Command
        registry=https://pkgs.dev.azure.com/<ORGANIZATION_NAME>/<PROJECT_NAME>/_packaging/<FEED_NAME>/npm/registry/
        
        always-auth=true
        ```

### Set up credentials

1. Copy the following snippet into your user-level `.npmrc` file, (Example: C:\Users\FabrikamUser\.npmrc). Make sure you don't paste it into the .npmrc file within your source repository.

    - **Organization-scoped feed**:

        ```Command
        ; begin auth token
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/<FEED_NAME>/npm/registry/:username=[ENTER_ANY_VALUE_BUT_NOT_AN_EMPTY_STRING]
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/<FEED_NAME>/npm/registry/:_password=[BASE64_ENCODED_PERSONAL_ACCESS_TOKEN]
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/<FEED_NAME>/npm/registry/:email=npm requires email to be set but doesn't use the value
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/<FEED_NAME>/npm/:username=[ANY_VALUE_BUT_NOT_AN_EMPTY_STRING]
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/<FEED_NAME>/npm/:_password=[BASE64_ENCODED_PERSONAL_ACCESS_TOKEN]
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/_packaging/<FEED_NAME>/npm/:email=npm requires email to be set but doesn't use the value
        ; end auth token
        ```

    - **Project-scoped feed**:

        ```Command
        ; begin auth token
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/<PROJECT_NAME>/_packaging/<FEED_NAME>/npm/registry/:username=[ENTER_ANY_VALUE_BUT_NOT_AN_EMPTY_STRING]
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/<PROJECT_NAME>/_packaging/<FEED_NAME>/npm/registry/:_password=[BASE64_ENCODED_PERSONAL_ACCESS_TOKEN]
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/<PROJECT_NAME>/_packaging/<FEED_NAME>/npm/registry/:email=npm requires email to be set but doesn't use the value
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/<PROJECT_NAME>/_packaging/<FEED_NAME>/npm/:username=[ENTER_ANY_VALUE_BUT_NOT_AN_EMPTY_STRING]
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/<PROJECT_NAME>/_packaging/<FEED_NAME>/npm/:_password=[BASE64_ENCODED_PERSONAL_ACCESS_TOKEN]
        //pkgs.dev.azure.com/<ORGANIZATION_NAME>/<PROJECT_NAME>/_packaging/<FEED_NAME>/npm/:email=npm requires email to be set but doesn't use the value
        ; end auth token
        ```

1. Generate a [personal access token](../../../organizations/accounts/use-personal-access-tokens-to-authenticate.md) with **Packaging** > **Read & write** scopes.

1. Run the following command to encode your newly generated personal access token. When prompted, paste your personal access token and then copy the resulting Base64 encoded value.

    ```Command
    node -e "require('readline') .createInterface({input:process.stdin,output:process.stdout,historySize:0}) .question('PAT> ',p => { b64=Buffer.from(p.trim()).toString('base64');console.log(b64);process.exit(); })"
    ```

    > [!NOTE]
    > As of July 2024, Azure DevOps Personal Access Tokens (PATs) are [82 characters long](../../organizations/accounts/use-personal-access-tokens-to-authenticate.md#changes-to-format). Some tools may insert automatic line breaks when encoding tokens to Base64. To avoid this, use the `-w0` flag with the *base64* command to ensure the output stays on a single line. 
    > In this tutorial, we use Node’s Buffer method, which produces a single-line *Base64* string by default.

1. Open your `.npmrc` file and replace the placeholder `[BASE64_ENCODED_PERSONAL_ACCESS_TOKEN]` with your encoded personal access token you just created.

::: moniker-end

