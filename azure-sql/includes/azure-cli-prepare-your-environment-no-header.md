---
author: dbradish-microsoft
ms.author: dbradish
ms.date: 09/10/2020
ms.topic: include
ms.custom: devx-track-azurecli
---

- Use the Bash environment in [Azure Cloud Shell](/azure/cloud-shell/quickstart). For more information, see [Azure Cloud Shell Quickstart - Bash](/azure/cloud-shell/quickstart).

   [![Launch Cloud Shell in a new window](media/cloud-shell-try-it/hdi-launch-cloud-shell.png)](https://shell.azure.com)
- If you prefer to run CLI reference commands locally, [install](/cli/azure/install-azure-cli) the Azure CLI. If you are running on Windows or macOS, consider running Azure CLI in a Docker container. For more information, see [How to run the Azure CLI in a Docker container](/cli/azure/run-azure-cli-docker).

  - If you're using a local installation, sign in to the Azure CLI by using the [az login](/cli/azure/reference-index#az-login) command. To finish the authentication process, follow the steps displayed in your terminal. For additional sign-in options, see [Sign in with the Azure CLI](/cli/azure/authenticate-azure-cli).

  - When you're prompted, install Azure CLI extensions on first use.  For more information about extensions, see [Use extensions with the Azure CLI](/cli/azure/azure-cli-extensions-overview).
  - Run [az version](/cli/azure/reference-index?#az-version) to find the version and dependent libraries that are installed. To upgrade to the latest version, run [az upgrade](/cli/azure/reference-index?#az-upgrade).