---
title: Uso de Azure Data Box para enviar datos al nivel de blob en bloques de acceso frecuente, poco frecuente o de archivo | Microsoft Docs en datos
description: Describe cómo usar Azure Data Box para enviar datos a un nivel de almacenamiento de blobs en bloques apropiado, como de acceso frecuente, poco frecuente o de archivo.
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: article
ms.date: 01/10/2019
ms.author: alkohli
ms.openlocfilehash: 91cabd6601ca8d0c0eabfdb5726d4bd6b37492a6
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "54216087"
---
# <a name="use-azure-data-box-to-send-data-to-appropriate-azure-storage-blob-tier"></a>Uso de Azure Data Box para enviar datos al nivel de blob de Azure Storage adecuado

Azure Data Box mueve grandes cantidades de datos a Azure mediante el envío de un dispositivo de almacenamiento propio. Debe llenar el dispositivo de datos y devolverlo. Los datos de Data Box se cargan en un nivel predeterminado asociado con la cuenta de almacenamiento. A continuación, puede mover los datos a otro nivel de almacenamiento.

En este artículo se describe cómo se pueden mover los datos que haya cargado Data Box a un nivel de blob de acceso frecuente, poco frecuente o de archivo.  

## <a name="choose-the-correct-storage-tier-for-your-data"></a>Selección del nivel de almacenamiento correcto para sus datos

Azure Storage ofrece tres niveles diferentes para almacenar datos de la manera más rentable: frecuente, poco frecuente o archivo. El nivel de almacenamiento de acceso frecuente está optimizada para almacenar datos que se consultan con frecuencia. El almacenamiento frecuente tiene mayores costos que el esporádico o el de archivo, pero menores costos de acceso.

El nivel de almacenamiento de acceso esporádico es para datos que se consultan con poca frecuencia y que deben almacenarse durante un mínimo de 30 días. El costo de almacenamiento del nivel poco frecuente es inferior al del nivel de acceso frecuente, pero los gastos de acceso a los datos son superiores en comparación con el nivel frecuente.

El nivel de almacenamiento de archivo de Azure se realiza sin conexión y ofrece los menores costos de almacenamiento, pero los mayores costos de acceso. Este nivel está pensado para datos que permanecen en el almacenamiento de archivo durante un período mínimo de 180 días. Para obtener detalles de cada uno de estos niveles y el modelo de precios, consulte [Comparación de los niveles de almacenamiento](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers#comparison-of-the-storage-tiers).

Los datos de Data Box se cargan en un nivel de almacenamiento asociado con la cuenta de almacenamiento. Al crear una cuenta de almacenamiento, puede establecer el nivel de acceso como frecuente o poco frecuente. Según el patrón de acceso de la carga de trabajo y el costo, puede cambiar estos datos del nivel predeterminado a otro nivel de almacenamiento.

Solo puede disponer los datos de almacenamiento de objetos en niveles de acceso en cuentas de Blob Storage o de uso general v2 (GPv2). Las cuentas de General Purpose v1 (GPv1) no admiten niveles. Para elegir el nivel de almacenamiento correcto para sus datos, revise las consideraciones que se detallan en [Azure Blob Storage: Niveles de almacenamiento de acceso frecuente, acceso esporádico y acceso de archivo](https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers).

## <a name="set-a-default-blob-tier"></a>Establecimiento de un nivel de blob predeterminado

El nivel de blob predeterminado se especifica cuando se crea la cuenta de almacenamiento en Azure Portal. Una vez que se selecciona un tipo de almacenamiento como Blob Storage o GPv2, se puede especificar el atributo de nivel de acceso. De forma predeterminada, se selecciona el nivel de acceso frecuente.

Los niveles no se pueden especificar si intenta crear una nueva cuenta al realizar un pedido de Data Box. Una vez creada la cuenta, puede modificar la cuenta en el portal para establecer el nivel de acceso predeterminado.

También puede crear primero una cuenta de almacenamiento con el atributo de nivel de acceso especificado. Al crear el pedido de Data Box, seleccione la cuenta de almacenamiento existente. Para más información sobre cómo establecer el nivel de blob predeterminado durante la creación de la cuenta de almacenamiento, consulte [Creación de una cuenta de almacenamiento en Azure Portal](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=portal).

## <a name="move-data-to-a-non-default-tier"></a>Transferencia de datos a un nivel distinto al predeterminado

Una vez que se carguen los datos de Data Box en el nivel predeterminado, podrá mover los datos a un nivel no predeterminado. Existen dos maneras de mover los datos a un nivel distinto al predeterminado.

- **Administración del ciclo de vida de Azure Blob Storage**: puede usar un enfoque basado en directivas para asignar automáticamente los datos a un nivel o para que expiren al final de su ciclo de vida. Para más información, consulte [Administración del ciclo de vida de Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-lifecycle-managment-concepts).
- **Scripts**: puede usar un enfoque con scripts mediante Azure PowerShell para habilitar niveles de blob. Puede llamar a la operación `SetBlobTier` para establecer el nivel en el blob.

## <a name="use-azure-powershell-to-set-the-blob-tier"></a>Uso de Azure PowerShell para establecer el nivel de blob

Los pasos siguientes describen cómo establecer el nivel de blob de archivo mediante un script de Azure PowerShell.

1. Abra una sesión de Windows PowerShell con privilegios elevados. Asegúrese de utilizar PowerShell 5.0 o superior. Escriba: 

   `$PSVersionTable.PSVersion`     

2. Inicie sesión en Azure PowerShell. 

   `Login-AzureRmAccount`  

3. Defina las variables para la cuenta de almacenamiento, la clave de acceso, el contenedor y el contexto de almacenamiento.

    ```powershell
    $StorageAccountName = "<enter account name>"
    $StorageAccountKey = "<enter account key>"
    $ContainerName = "<enter container name>"
    $ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    ```

4. Obtenga todos los blobs del contenedor.

    `$blobs = Get-AzureStorageBlob -Container "<enter container name>" -Context $ctx`
 
5. Establezca el nivel de todos los blobs del contenedor en el nivel de archivo.

    ```powershell
    Foreach ($blob in $blobs) {
    $blob.ICloudBlob.SetStandardBlobTier("Archive")
    }
    ```

    A continuación se muestra una salida de ejemplo:

    ```
    Windows PowerShell
    Copyright (C) Microsoft Corporation. All rights reserved.
    PS C:\WINDOWS\system32> $PSVersionTable.PSVersion

    Major  Minor  Build  Revision
    -----  -----  -----  --------
    5      1      17763  134
    PS C:\WINDOWS\system32> Login-AzureRmAccount

    Account          : gus@contoso.com
    SubscriptionName : MySubscription
    SubscriptionId   : subscription-id
    TenantId         : tenant-id
    Environment      : AzureCloud

    PS C:\WINDOWS\system32> $StorageAccountName = "mygpv2storacct"
    PS C:\WINDOWS\system32> $StorageAccountKey = "mystorageacctkey"
    PS C:\WINDOWS\system32> $ContainerName = "test"
    PS C:\WINDOWS\system32> $ctx = New-AzureStorageContext -StorageAccountName $StorageAccountName -StorageAccountKey $StorageAccountKey
    PS C:\WINDOWS\system32> $blobs = Get-AzureStorageBlob -Container "test" -Context $ctx
    PS C:\WINDOWS\system32> Foreach ($blob in $blobs) {
    >> $blob.ICloudBlob.SetStandardBlobTier("Archive")
    >> }
    PS C:\WINDOWS\system32>
    ```
> [!TIP]
> Si desea que los datos se archiven al ingerirse, establezca el nivel de cuenta predeterminado en frecuente. Si el nivel predeterminado es esporádico, habrá una penalización de eliminación temprana de 30 días si los datos se cambian al nivel de archivo inmediatamente.

## <a name="next-steps"></a>Pasos siguientes

-  Obtenga información acerca de cómo tratar [escenarios de niveles de datos comunes con reglas de directivas de ciclo de vida](https://docs.microsoft.com/azure/storage/blobs/storage-lifecycle-management-concepts#examples).

