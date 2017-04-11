<properties
   pageTitle="Stvaranje skupu zapisa i zapisa za DNS zone pomoću komponente PowerShell | Microsoft Azure"
   description="Upute za stvaranje zapisa za glavno računalo za Azure DNS. Postavljanje zapis postavlja i zapisa pomoću komponente PowerShell"
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



# <a name="create-dns-record-sets-and-records-by-using-powershell"></a>Stvaranje DNS zapisa skupova i zapise pomoću komponente PowerShell


> [AZURE.SELECTOR]
- [Portal za Azure](dns-getstarted-create-recordset-portal.md)
- [PowerShell](dns-getstarted-create-recordset.md)
- [Azure EŽA](dns-getstarted-create-recordset-cli.md)

U ovom se članku vodit će vas kroz postupak stvaranja zapisa i skupova zapisa pomoću komponente Windows PowerShell. Nakon stvaranja DNS zone, dodajte DNS zapise za svoju domenu. Da biste to učinili, najprije morate razumjeti DNS zapise i skupove zapisa.

[AZURE.INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

## <a name="verify-that-you-have-the-latest-version-of-powershell"></a>Provjerite imate li najnoviju verziju programa PowerShell

Provjerite jeste li instalirali najnoviju verziju programa Azure resursima PowerShell cmdleti. Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="create-a-record-set-and-record"></a>Stvaranje skup zapisa i zapisa

U ovom se odjeljku opisuje kako stvoriti skup zapisa i zapis.


### <a name="1-connect-to-your-subscription"></a>1. povezati pretplate

Otvorite konzole za PowerShell i povezati s računom. Pomoću sljedeće ogledne možete povezati:

    Login-AzureRmAccount

Provjerite pretplate za račun.

    Get-AzureRmSubscription

Navedite pretplatu u koju želite koristiti.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

Dodatne informacije o radu sa servisom PowerShell potražite u članku [Pomoću komponente Windows PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md).


### <a name="2-create-a-record-set"></a>2. Stvaranje skupa zapisa

Stvaranje skupove zapisa pomoću na `New-AzureRmDnsRecordSet` cmdlet. Kada stvarate skup zapisa, morate navedite zapis skupa naziva, u zonu, vrijeme trajanja (TTL) i vrstu zapisa.

Da biste stvorili zapis u vrh zone (u tom slučaju "contoso.com"), koristite naziv zapisa "@", uključujući navodnike. To je uobičajena konvencija DNS-a.

Sljedeći primjer stvara zapis postavljanje relativni naziva "www" u "contoso.com" DNS Zone. Potpuno kvalificiran naziv skupa zapisa je "www.contoso.com". Vrsta zapisa je "A", a na TTL 60 sekundi. Nakon dovršetka ovaj korak, imat ćete skup zapisa za prazan "www" koji je dodijeljen varijable *$rs*.

    $rs = New-AzureRmDnsRecordSet -Name "www" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 60

#### <a name="if-a-record-set-already-exists"></a>Postoji li već postavljen zapisa

Ako zapis postavljanje već postoji, naredba neće uspjeti ako na *-prebrisati* koristi parametar. Na *-prebrisati* mogućnost pokreće odzivnik za potvrdu, koji možete prikazati pomoću na *– prisilno* prijelaz.


    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 -ZoneName contoso.com -ResouceGroupName MyAzureResouceGroup [-Tag $tags] [-Overwrite] [-Force]


U ovom se primjeru pomoću zone ime i naziv grupe resursa navedite zone. Osim toga, možete odrediti zone objekt, kao što je vratio `Get-AzureRmDnsZone` ili `New-AzureRmDnsZone`.

    $zone = Get-AzureRmDnsZone -Name contoso.com –ResourceGroupName MyAzureResourceGroup
    $rs = New-AzureRmDnsRecordSet -Name www -RecordType A -Ttl 300 –Zone $zone [-Tag $tags] [-Overwrite] [-Force]

`New-AzureRmDnsRecordSet`Vraća lokalni objekt koji predstavlja skup zapisa koja je stvorena u Azure DNS-a.

### <a name="3-add-a-record"></a>3. dodavanje zapisa

Da biste koristili skup zapisa novostvorenu "www", morate dodati zapise. Možete dodati IPv4 *A* zapisa sa zapisom "www" postavite pomoću u sljedećem primjeru. U ovom se primjeru ovisi o varijable *$rs* koje ste postavili u prethodnom koraku.

Dodavanje zapisa sa zapisom zakazivati pomoću `Add-AzureRmDnsRecordConfig` je izvan mreže operacija. Ažurira se samo na lokalne varijable *$rs* .


    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.185.46
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address 134.170.188.221

### <a name="4-commit-the-changes"></a>4. Zapiši promjene

Pohranite promjene skup zapisa. Korištenje `Set-AzureRmDnsRecordSet` da biste prenijeli promjene na postavljanje Azure DNS zapis.

    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="5-retrieve-the-record-set"></a>5. dohvatiti skup zapisa

Možete dohvatiti zapisa postavite iz Azure DNS pomoću `Get-AzureRmDnsRecordSet` kao što je prikazano u sljedećem primjeru.


    Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup


    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : MyAzureResourceGroup
    Ttl               : 3600
    Etag              : 68e78da2-4d74-413e-8c3d-331ca48246d9
    RecordType        : A
    Records           : {134.170.185.46, 134.170.188.221}
    Tags              : {}


Možete koristiti i alata nslookup ili druge alate za DNS upit novi skup zapisa.

Ako još nije dodijeljeno domene na poslužitelje naziva Azure DNS-a, morate izričito odredite naziv, poslužitelj i adresu zone.


    nslookup www.contoso.com ns1-01.azure-dns.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    Name:    www.contoso.com
    Addresses:  134.170.185.46
                134.170.188.221

## <a name="create-a-record-set-of-each-type-with-a-single-record"></a>Stvaranje skupa zapisa svake vrste s jednim zapisom


Sljedeći primjeri pokazuju kako stvoriti skup zapisa svaku vrstu zapisa. Svaki skup zapisa sadrži jedan zapis.

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]


## <a name="next-steps"></a>Daljnji koraci

[Kako upravljati DNS zone pomoću komponente PowerShell](dns-operations-dnszones.md)

[Upravljanje DNS zapise i skupove zapisa pomoću komponente PowerShell](dns-operations-recordsets.md)

[Automatiziranje Azure operacija s .NET SDK](dns-sdk.md)
