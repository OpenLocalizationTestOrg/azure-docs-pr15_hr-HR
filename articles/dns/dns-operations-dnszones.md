<properties
   pageTitle="Upravljanje DNS zone pomoću komponente PowerShell | Microsoft Azure"
   description="Možete upravljati DNS zone pomoću komponente Powershell Azure. Kako ažuriranje, brisanje i stvaranje DNS zone na Azure DNS-a"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="how-to-manage-dns-zones-using-powershell"></a>Kako upravljati DNS zone pomoću komponente PowerShell

> [AZURE.SELECTOR]
- [Azure EŽA](dns-operations-dnszones-cli.md)
- [PowerShell](dns-operations-dnszones.md)



U ovom se članku vidjet ćete kako upravljati DNS zone pomoću komponente PowerShell. Da biste koristili ove korake, morate instalirati najnoviju verziju programa Azure resursima PowerShell cmdleti (1.0 ili noviji). Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .


## <a name="create-a-new-dns-zone"></a>Stvaranje nove DNS zone

Da biste stvorili DNS zone, potražite u članku [Stvaranje DNS zone pomoću komponente PowerShell](dns-getstarted-create-dnszone.md).

## <a name="get-a-dns-zone"></a>Početak DNS zone

Da biste dohvatili DNS zone, poslužite se `Get-AzureRmDnsZone` cmdlet. Ovaj postupak vraća objekt DNS zone koja odgovara postojeće zone u Azure DNS. Objekt sadrži podatke o zone (kao što je broj skupove zapisa), ali ne sadrži zapis skupove sami.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup

## <a name="list-dns-zones"></a>Popis DNS zone

Po ispuštanje zona iz `Get-AzureRmDnsZone`, možete numerirati svih zona u grupu resursa. Ovaj postupak vraća polje zone objekata.

    $zoneList = Get-AzureRmDnsZone -ResourceGroupName MyAzureResourceGroup

## <a name="update-a-dns-zone"></a>Ažuriranje DNS zone

Promjene DNS zone resursa možete napraviti pomoću `Set-AzureRmDnsZone`. To se neće ažurirati bilo koji od skupove DNS zapisa unutar zone (pogledajte [upute za upravljanje DNS zapisima](dns-operations-recordsets.md)). Koristi se samo za ažuriranje svojstava zone resursa sam. Ovo je trenutno ograničena za Azure upravitelj resursa oznake za resurs zone. Dodatne informacije potražite u [Etags i oznake](dns-getstarted-create-dnszone.md#Etags-and-tags) .

Koristite nešto od sljedećeg dva načina za ažuriranje DNS zone:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group"></a>Određivanje zone pomoću zone grupa naziv i resursa

    Set-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Tag $tags]

### <a name="specify-the-zone-using-a-zone-object"></a>Određivanje zone pomoću $zone objekta

Određivanje zone pomoću $zone objekta iz `Get-AzureRmDnsZone`. Prilikom korištenja `Set-AzureRmDnsZone` $zone objekt, provjere e-oznake koristit će se da biste bili sigurni istovremene promjene se prebrisati. Možete koristiti neobavezna *-prebrisati* promjenu izostavlja te provjere. Dodatne informacije potražite u [Etags i oznake](dns-getstarted-create-dnszone.md#Etags-and-tags) .


    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    <..modify $zone.Tags here...>
    Set-AzureRmDnsZone -Zone $zone [-Overwrite]


## <a name="delete-a-dns-zone"></a>Brisanje DNS Zone

DNS zone moguće je izbrisati pomoću cmdleta Ukloni AzureRmDnsZone.

Prije brisanja DNS zone u Azure DNS, morate izbrisati sve skupove zapisa, osim NS i SOA zapise u korijenu zoni stvoreni automatski kada je stvorena u zonu.

Koristite jedan od sljedeća dva načina da biste uklonili DNS zone:

### <a name="specify-the-zone-using-the-zone-name-and-resource-group-name"></a>Određivanje zone pomoću zone imena i naziva grupa resursa

Ovaj postupak je programa neobavezno *– prisilno* parametar koji ukida se zatraži da potvrdite da želite ukloniti DNS zone.

    Remove-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]

### <a name="specify-the-zone-using-a-zone-object"></a>Određivanje zone pomoću $zone objekta

Određivanje zone pomoću $zone objekta iz `Get-AzureRmDnsZone`. Ovaj postupak je programa neobavezno *– prisilno* parametar koji ukida se zatraži da potvrdite da želite ukloniti DNS zone. Kao i `Set-AzureRmDnsZone`, određivanje zone pomoću $zone objekta omogućuje provjere e-oznake da biste bili sigurni da neće se izbrisati istovremene promjene. <BR>
Neobavezna *-prebrisati* zastavice ukida te provjere. Dodatne informacije potražite u [Etags i oznake](dns-getstarted-create-dnszone.md#Etags-and-tags) .

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsZone -Zone $zone [-Force] [-Overwrite]



Objekt zone može biti piped i umjesto predugački kao parametar:

    Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsZone [-Force] [-Overwrite]

## <a name="next-steps"></a>Daljnji koraci

Nakon stvaranja DNS zone, stvaranje [skupa zapisa i zapisa](dns-getstarted-create-recordset.md) da biste pokrenuli raščlanjuju imena za svoju domenu Internet.