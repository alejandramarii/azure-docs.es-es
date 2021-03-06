---
title: Permitir a aplicaciones recuperar secretos del almacén de claves de Azure Stack | Microsoft Docs
description: Usar una aplicación de ejemplo para trabajar con el almacén de claves de Azure Stack
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/27/2018
ms.author: sethm
ms.lastreviewed: 12/27/2018
ms.openlocfilehash: e395b3f24ac525ce226d5520ff483b07d23636b5
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55241974"
---
# <a name="a-sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Aplicación de ejemplo que usa claves y secretos almacenados en un almacén de claves

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Siga los pasos de este artículo para ejecutar una aplicación de ejemplo llamada **HelloKeyVault** que recupera las claves y loa secretos de un almacén de claves en Azure Stack.

## <a name="prerequisites"></a>Requisitos previos

Puede instalar los siguientes requisitos previos con el [Kit de desarrollo de Azure Stack](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop), o con un cliente externo basado en Windows si está [conectado a través de VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn):

* Instale los [módulos de Azure PowerShell compatibles con Azure Stack](azure-stack-powershell-install.md).
* Descargue las [herramientas necesarias para trabajar con Azure Stack](azure-stack-powershell-download.md).

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Creación y obtención del almacén de claves y la configuración de la aplicación

Para preparar la aplicación de ejemplo:

* Cree un almacén de claves en Azure Stack.
* Registre una aplicación en Azure Active Directory (Azure AD).

Puede usar Azure Portal o PowerShell para prepararse para la aplicación de ejemplo. En este artículo se muestra cómo crear un almacén de claves y registrar una aplicación mediante PowerShell.

>[!NOTE]
>De forma predeterminada, este script de PowerShell crea una aplicación nueva en Active Directory. Sin embargo, también puede registrar una de las aplicaciones existentes.

Antes de ejecutar el siguiente script, asegúrese de proporcionar valores para las variables `aadTenantName` y `applicationPassword`. Si no especifica ningún valor para `applicationPassword`, este script genera una contraseña aleatoria.

```powershell
$vaultName           = 'myVault'
$resourceGroupName   = 'myResourceGroup'
$applicationName     = 'myApp'
$location            = 'local'

# Password for the application. If not specified, this script will generate a random password during app creation.
$applicationPassword = ''

# Function to generate a random password for the application.
Function GenerateSymmetricKey()
{
    $key = New-Object byte[](32)
    $rng = [System.Security.Cryptography.RNGCryptoServiceProvider]::Create()
    $rng.GetBytes($key)
    return [System.Convert]::ToBase64String($key)
}

Write-Host 'Please log into your Azure Stack user environment' -foregroundcolor Green

$tenantARM = "https://management.local.azurestack.external"
$aadTenantName = "FILL THIS IN WITH YOUR AAD TENANT NAME. FOR EXAMPLE: myazurestack.onmicrosoft.com"

# Configure the Azure Stack operator’s PowerShell environment.
Add-AzureRMEnvironment `
  -Name "AzureStackUser" `
  -ArmEndpoint $tenantARM

Set-AzureRmEnvironment `
  -Name "AzureStackAdmin" `
  -GraphAudience "https://graph.windows.net/"

$TenantID = Get-AzsDirectoryTenantId `
  -AADTenantName $aadTenantName `
  -EnvironmentName AzureStackUser

# Sign in to the user portal.
Add-AzureRmAccount `
  -EnvironmentName "AzureStackUser" `
  -TenantId $TenantID `

$now = [System.DateTime]::Now
$oneYearFromNow = $now.AddYears(1)

$applicationPassword = GenerateSymmetricKey

# Create a new Azure AD application.
$identifierUri = [string]::Format("http://localhost:8080/{0}",[Guid]::NewGuid().ToString("N"))
$homePage = "http://contoso.com"

Write-Host "Creating a new AAD Application"
$ADApp = New-AzureRmADApplication `
  -DisplayName $applicationName `
  -HomePage $homePage `
  -IdentifierUris $identifierUri `
  -StartDate $now `
  -EndDate $oneYearFromNow `
  -Password $applicationPassword

Write-Host "Creating a new AAD service principal"
$servicePrincipal = New-AzureRmADServicePrincipal `
  -ApplicationId $ADApp.ApplicationId

# Create a new resource group and a key vault in that resource group.
New-AzureRmResourceGroup `
  -Name $resourceGroupName `
  -Location $location

Write-Host "Creating vault $vaultName"
$vault = New-AzureRmKeyVault -VaultName $vaultName `
  -ResourceGroupName $resourceGroupName `
  -Sku standard `
  -Location $location

# Specify full privileges to the vault for the application.
Write-Host "Setting access policy"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName `
  -ObjectId $servicePrincipal.Id `
  -PermissionsToKeys all `
  -PermissionsToSecrets all

Write-Host "Paste the following settings into the app.config file for the HelloKeyVault project:"
'<add key="VaultUrl" value="' + $vault.VaultUri + '"/>'
'<add key="AuthClientId" value="' + $servicePrincipal.ApplicationId + '"/>'
'<add key="AuthClientSecret" value="' + $applicationPassword + '"/>'
Write-Host
```

En la imagen siguiente se muestra la salida del script usado para crear el almacén de claves:

![Almacén de claves con claves de acceso](media/azure-stack-key-vault-sample-app/settingsoutput.png)

Tome nota de los valores de **VaultUrl**, **AuthClientId** y **AuthClientSecret** que devolvió el script anterior. Para ejecutar la aplicación HelloKeyVault, utilizará estos valores.

## <a name="download-and-configure-the-sample-application"></a>Descarga y configuración de la aplicación de ejemplo

Descargue el ejemplo de almacén de claves de la página de Azure [Key vault client samples](https://www.microsoft.com/download/details.aspx?id=45343) (ejemplos de cliente del almacén de claves). Extraiga el contenido del archivo .zip en la estación de trabajo de desarrollo. Hay dos aplicaciones dentro de la carpeta de ejemplos. En este artículo se usa **HelloKeyVault**.

Para cargar el ejemplo **HelloKeyVault**:

* Vaya a la carpeta **Microsoft.Azure.KeyVault.Samples** > **ejemplos** > **HelloKeyVault**.
* Abra la aplicación **HelloKeyVault** en Visual Studio.

### <a name="configure-the-sample-application"></a>Configurar la aplicación de ejemplo

En Visual Studio:

* Abra el archivo HelloKeyVault\App.config y busque el elemento &lt;**appSettings**&gt;.
* Actualice las claves **VaultUrl**, **AuthClientId** y **AuthClientSecret** con los valores devueltos por aquellas que se usaron para crear el almacén de claves. De forma predeterminada, el archivo App.config tiene un marcador de posición para `AuthCertThumbprint`. Reemplace este marcador de posición por `AuthClientSecret`.

  ![Configuración de la aplicación](media/azure-stack-key-vault-sample-app/appconfig.png)

* Recompile la solución.

## <a name="run-the-application"></a>Ejecución de la aplicación

Al ejecutar HelloKeyVault, la aplicación inicia sesión en Azure AD y, a continuación, usa el token de AuthClientSecret para autenticarse en el almacén de claves de Azure Stack.

Puede usar el ejemplo HelloKeyVault para:

* Realizar operaciones básicas, como crear, cifrar, ajustar y eliminar, en las claves y secretos.
* Pase parámetros como `encrypt` y `decrypt` a HelloKeyVault, y aplique los cambios específicos a un almacén de claves.

## <a name="next-steps"></a>Pasos siguientes

- [Implementación de una máquina virtual con una contraseña de Key Vault](azure-stack-key-vault-deploy-vm-with-secret.md)
- [Implementación de una máquina virtual con un certificado de Key Vault](azure-stack-key-vault-push-secret-into-vm.md)
