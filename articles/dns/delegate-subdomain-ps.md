---
title: Delegación de un subdominio de Azure DNS con Azure PowerShell
description: Aprenda a delegar un subdominio de Azure DNS con Azure PowerShell.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 2/7/2019
ms.author: victorh
ms.openlocfilehash: 40b2a4d98e6269d9740856ba44c1043af75ce1b8
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55896645"
---
# <a name="delegate-an-azure-dns-subdomain-using-azure-powershell"></a>Delegación de un subdominio de Azure DNS con Azure PowerShell

Puede usar Azure PowerShell para delegar un subdominio DNS. Por ejemplo, si es propietario del dominio contoso.com, puede delegar un subdominio llamado *ingeniería* a otra zona independiente que se puede administrar por separado respecto de la zona contoso.com.

Si lo prefiere, puede delegar un subdominio usando [Azure Portal](delegate-subdomain.md).

> [!NOTE]
> Contoso.com se usa como ejemplo en todo este artículo. Sustituya su propio nombre de dominio por contoso.com.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

## <a name="prerequisites"></a>Requisitos previos

Para delegar un subdominio de Azure DNS, primero debe delegar el dominio público en Azure DNS. Consulte [Delegación de un dominio en Azure DNS](./dns-delegate-domain-azure-dns.md) para obtener instrucciones sobre cómo configurar los servidores de nombres para su delegación. Una vez que el dominio se delega a la zona de Azure DNS, puede delegar el subdominio.

## <a name="create-a-zone-for-your-subdomain"></a>Creación de una zona para el subdominio

En primer lugar, cree la zona para el subdominio **ingeniería**.

`New-AzDnsZone -ResourceGroupName <resource group name> -Name engineering.contoso.com`

## <a name="note-the-name-servers"></a>Observación de los servidores de nombres

A continuación, anote los cuatro servidores de nombres para el subdominio de ingeniería.

`Get-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -RecordType NS`

## <a name="create-a-test-record"></a>Creación de un registro de prueba

Crear un registro **A** en la zona de ingeniería que se usará para las pruebas.

   `New-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -Name www -RecordType A -ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address 10.10.10.10)`.

## <a name="create-an-ns-record"></a>Creación de un registro NS

A continuación, cree un registro de servidor de nombres para la zona **ingeniería** en la zona contoso.com.

```azurepowershell
$Records = @()
$Records += New-AzDnsRecordConfig -Nsdname <name server 1 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 2 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 3 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 4 noted previously>
$RecordSet = New-AzDnsRecordSet -Name engineering -RecordType NS -ResourceGroupName <resource group name> -TTL 3600 -ZoneName contoso.com -DnsRecords $Records
```

## <a name="test-the-delegation"></a>Prueba de la delegación

Utilice nslookup para probar la delegación.

1. Abra una ventana de PowerShell.
2. En el símbolo del sistema, escriba `nslookup www.engineering.contoso.com.`.
3. Debería recibir una respuesta no autoritativa que muestra la dirección **10.10.10.10**.

## <a name="next-steps"></a>Pasos siguientes

Aprenda a [configurar búsquedas inversas de DNS para servicios hospedados en Azure](dns-reverse-dns-for-azure-services.md).