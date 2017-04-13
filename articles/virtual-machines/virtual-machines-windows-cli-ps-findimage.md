<properties
   pageTitle="Pronađite i odaberite slika Windows VM | Microsoft Azure"
   description="Saznajte kako odrediti publisher, ponude i SKU-om za slike prilikom stvaranja virtualnog računala za Windows s modelom implementacije Voditelj resursa."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="squillace"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"
   />

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure"
   ms.date="08/23/2016"
   ms.author="rasquill"/>

# <a name="navigate-and-select-windows-virtual-machine-images-in-azure-with-powershell-or-the-cli"></a>Pronađite i odaberite slika virtualnog računala sustava Windows Azure pomoću programa PowerShell ili na EŽA

U ovoj se temi opisuje kako pronaći izdavači VM slike, nudi, SKU-ove i verzija za svaku lokaciju u koju mogu uvesti. Da bi se dobilo primjer su neke najčešće korištenih Windows VM slike:

## <a name="table-of-commonly-used-windows-images"></a>Tablica slika najčešće korištenih sustava Windows


| PublisherName                        | Ponude                                 | SKU                         |
|:---------------------------------|:-------------------------------------------|:---------------------------------|:--------------------|
| MicrosoftDynamicsNAV             | DynamicsNAV                                | 2015.                             |
| MicrosoftSharePoint              | MicrosoftSharePointServer                  | 2013                             |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Enterprise – Optimizirano-za-DW      |
| MicrosoftSQLServer               | SQL2014 WS2012R2                           | Enterprise – Optimizirano-za-OLTP    |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 R2 Standard                  |
| MicrosoftWindowsServer           | WindowsServer                              | 2012 Standard               |
| MicrosoftWindowsServer           | WindowsServer                              | 2008 R2 SP1 |
| MicrosoftWindowsServer           | WindowsServer                              | Pretpregled sustava Windows – Server – tehničkoj |
| MicrosoftWindowsServerEssentials | WindowsServerEssentials                    | WindowsServerEssentials          |
| MicrosoftWindowsServerHPCPack    | WindowsServerHPCPack                       | 2012R2                           |


[AZURE.INCLUDE [virtual-machines-common-cli-ps-findimage](../../includes/virtual-machines-common-cli-ps-findimage.md)]
