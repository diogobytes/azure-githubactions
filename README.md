# Deploy to Azure

</header>

<!--
  <<< Author notes: Step 2 >>>
  Start this step by acknowledging the previous step.
  Define terms and link to docs.github.com.
-->

### Store your credentials in GitHub secrets and finish setting up your workflow

1. In a new tab, [create an Azure account](https://azure.microsoft.com/en-us/free/) if you don't already have one. If your Azure account is created through work, you may encounter issues accessing the necessary resources -- we recommend creating a new account for personal use and for this course.
1. Create a [new subscription](https://docs.microsoft.com/en-us/azure/cost-management-billing/manage/create-subscription) in the Azure Portal.
    > **Note**: your subscription must be configured "Pay as you go" which will require you to enter billing information. This course will only use a few minutes from your free plan, but Azure requires the billing information.
1. Install [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) on your machine.
1. In your terminal, run:

    ```shell
    az login
    ```

1. Select the subscription you just selected from the interactive authentication prompt. Copy the value of the subscription ID to a safe place. We'll call this `AZURE_SUBSCRIPTION_ID`. Here's an example of what it looks like:

    ```shell
    No     Subscription name    Subscription ID                       Tenant
    -----  -------------------  ------------------------------------  -----------------
    [1] *  some-subscription    f****a09-****-4d1c-98**-f**********c  Default Directory
    ```

1. In your terminal, run the command below.

    ```shell
    az ad sp create-for-rbac --name "GitHub-Actions" --role contributor \
     --scopes /subscriptions/{subscription-id} \
     --sdk-auth

        # Replace {subscription-id} with the same id stored in AZURE_SUBSCRIPTION_ID.
    ```

    > **Note**: The `\` character works as a line break on Unix based systems. If you are on a Windows based system the `\` character will cause this command to fail. Place this command on a single line if you are using Windows.

1. Copy the entire contents of the command's response, we'll call this `AZURE_CREDENTIALS`. Here's an example of what it looks like:

    ```shell
    {
      "clientId": "<GUID>",
      "clientSecret": "<GUID>",
      "subscriptionId": "<GUID>",
      "tenantId": "<GUID>",
      (...)
    }
    ```

1. Back on GitHub, click on this repository's **Secrets and variables > Actions** in the Settings tab.
1. Click **New repository secret**
1. Name your new secret **AZURE_SUBSCRIPTION_ID** and paste the value from the `id:` field in the first command.
1. Click **Add secret**.
1. Click **New repository secret** again.
1. Name the second secret **AZURE_CREDENTIALS** and paste the entire contents from the second terminal command you entered.
1. Click **Add secret**
