<properties
   pageTitle="Upravljanje DNS zapisa skupova i zapise pomoću portala za Azure | Microsoft Azure"
   description="Upravljanje DNS zapisa skupova i zapisa u Azure DNS hostiranje vaše domene na Azure DNS-a. Sve naredbe ljuske PowerShell za operacije na skupove zapisa i zapisa."
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

# <a name="manage-dns-records-and-record-sets-by-using-powershell"></a>Upravljanje DNS zapise i skupove zapisa pomoću komponente PowerShell



> [AZURE.SELECTOR]
- [Portal za Azure](dns-operations-recordsets-portal.md)
- [Azure EŽA](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)



U ovom se članku objašnjava upravljanje skupove zapisa i zapisa zone DNS-a pomoću komponente Windows PowerShell.

Važno je da razlika između skupove zapisa u DNS-a i pojedinačne DNS zapisa. Skup zapisa je skup zapisa u zoni koji imaju isti naziv i imaju iste vrste. Dodatne informacije potražite u članku [Stvaranje DNS zapisa skupova i zapise pomoću portala za Azure](dns-getstarted-create-recordset-portal.md).

Da biste upravljali skupove zapisa i zapisa, morate na najnoviju verziju programa Azure resursima PowerShell cmdleti. Dodatne informacije potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md). Dodatne informacije o radu sa servisom PowerShell potražite u članku [Korištenje Azure PowerShell s Azure Voditelj resursa](../powershell-azure-resource-manager.md).



## <a name="create-a-new-record-set-and-a-record"></a>Stvaranje novog skupa zapisa i zapisa

Da biste stvorili zapis zakazivati pomoću komponente PowerShell potražite u članku [Stvaranje DNS zapisa skupova i zapise pomoću komponente PowerShell](dns-getstarted-create-recordset.md).

## <a name="get-a-record-set"></a>Početak skupu zapisa

Da biste dohvatili postojeći skup zapisa, koristite `Get-AzureRmDnsRecordSet`. Navedite zapis skupa relativni naziv, vrstu zapisa i zone.

    $rs = Get-AzureRmDnsRecordSet –Name www –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

Kao i `New-AzureRmDnsRecordSet`, naziv zapisa mora biti relativni naziv, što znači da ga morate isključiti naziv zone.

U zonu možete odrediti pomoću zona i naziv grupe resursa ili pomoću zone objekta:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $rs = Get-AzureRmDnsRecordSet -Name www –RecordType A -Zone $zone

`Get-AzureRmDnsRecordSet`Vraća lokalni objekt koji predstavlja skup zapisa koja je stvorena u Azure DNS-a.

## <a name="list-record-sets"></a>Popis zapisa skupovima

Možete koristiti`Get-AzureRmDnsRecordSet` skupovima popis zapisa Ako izostavite na *– naziv* i/ili *– RecordType* parametara.

### <a name="to-list-all-record-sets"></a>Da biste dobili popis sve skupove zapisa

U ovom se primjeru vraća sve skupove zapisa, bez obzira na naziv i vrsta zapisa:

    $list = Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-list-record-sets-of-a-given-record-type"></a>Na popisu zapisa skupove navedene vrste zapisa

U ovom se primjeru prikazuje sve skupove zapisa koje odgovaraju vrsti dani zapis. U ovom slučaju postavljanje odnosno vraćena je zapis "U" zapisa:

    $list = Get-AzureRmDnsRecordSet –RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

U zonu možete navesti pomoću ili *RecordAdd –* i *– ResourceGroupName* parametre (kao što je prikazano) ili navođenjem zone objekta:

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResouceGroupName MyAzureResourceGroup
    $list = Get-AzureRmDnsRecordSet -Zone $zone

## <a name="add-a-record-to-a-record-set"></a>Dodavanje zapisa u skupu zapisa

Dodavanje zapisa skupove zapisa pomoću na `Add-AzureRmDnsRecordConfig` cmdlet. Ovo je operaciju izvanmrežno. Promijenit će se samo lokalni objekt koji predstavlja skup zapisa.

Parametri za dodavanje zapisa u skupu zapisa razlikuju se ovisno o vrsti skup zapisa. Na primjer, koristite li skup zapisa vrste "A", možete navesti samo zapise s parametrom *-IPv4Address*.

Dodatni zapisi se može dodati u svaki zapis postavio dodatne pozive na `Add-AzureRmDnsRecordConfig`. Možete dodati do 20 zapisa bilo koji skup zapisa. Međutim, skupove zapisa vrste "CNAME" može sadržavati najviše jedan zapis i skup zapisa ne može sadržavati dva zapisa jednake. Prazan skupove zapisa (s nulom zapisa) mogu se kreirati, ali ne prikazuju se na poslužitelje naziva Azure DNS-a.

Nakon skup zapisa sadrži željeni skup zapisa, morate izvršiti pomoću na `Set-AzureRmDnsRecordSet` cmdlet. Nakon izvršenja je skup zapisa, ona će zamijeniti postojeći zapis u Azure DNS.

### <a name="to-create-an-a-record-set-with-a-single-record"></a>Da biste stvorili A zapis postavljen s jednim zapisom

    $rs = New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Slijed operacije da biste stvorili zapis može biti *piped*, što znači da se objekt skup zapisa prenesite pomoću kanala umjesto prosljeđivanje kao parametar. Ako, na primjer:

    New-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Ttl 60 -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Add-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="additional-record-type-examples"></a>Primjeri dodatnu vrstu zapisa

[AZURE.INCLUDE [dns-add-record-ps-include](../../includes/dns-add-record-ps-include.md)]

## <a name="modify-existing-record-sets"></a>Izmjena postojećeg skupove zapisa

Upute za izmjenu postojeći skup zapisa su slične korake koje poduzmete prilikom stvaranja zapisa. Slijed operacije je na sljedeći način:

1.  Dohvaćanje postojeći zapis zakazivati pomoću `Get-AzureRmDnsRecordSet`.

2.  Izmjena zapis odredio dodavanja zapisa, uklanjanjem zapisa, da biste mijenjanje zapisa parametara ili promjenu zapisa postavite vrijeme trajanja (TTL). Ovo je operaciju izvanmrežno. Promijenit će se samo lokalni objekt koji predstavlja skup zapisa.

3.  Zapiši promjene pomoću na `Set-AzureRmDnsRecordSet` cmdlet. Zamjenjuje postojeći zapis u Azure DNS.


### <a name="to-update-a-record-in-an-existing-record-set"></a>Da biste ažurirali zapisa u postojeći skup zapisa

U ovom primjeru smo promjena IP adrese postojećeg "" zapis:

    $rs = Get-AzureRmDnsRecordSet -name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Ipv4Address = "134.170.185.46"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Na `Set-AzureRmDnsRecordSet` cmdlet koristi provjere e-oznake da biste bili sigurni da istovremene promjene se prebrisati. Korištenje na *-prebrisati* zastavice izostavlja te provjere. Dodatne informacije potražite u članku [o etags i oznake](dns-getstarted-create-dnszone.md#tagetag).

### <a name="to-modify-an-soa-record"></a>Da biste izmijenili zapis SOA

Ne možete dodavati niti ukloniti zapise iz zapisa SOA automatski stvara koji se postaviti na vrh zone (naziv = "@"). Međutim, možete mijenjati sve parametre zapisa SOA (osim "glavnog računala)" i zapisa postavite TTL.

Sljedeći primjer prikazuje način da biste promijenili svojstvo *e-pošte* SOA zapis:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Records[0].Email = "admin.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-modify-ns-records-at-the-zone-apex"></a>Da biste izmijenili NS zapisa na vrh zone

Nije moguće dodavanje, uklanjanje ili mijenjanje zapisa u automatski stvara NS zapisa skup vrh zone (naziv = "@"). Samo promjene koje je dopušteno je da biste izmijenili skup zapisa TTL.

Sljedeći primjer prikazuje način da biste promijenili svojstvo TTL skupa zapisa poslužitelja naziva:

    $rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    $rs.Ttl = 300
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="to-add-records-to-an-existing-record-set"></a>Da biste dodali zapise u postojeći skup zapisa

U ovom primjeru dodat ćemo dva dodatna MX zapisa u postojeći skup zapisa:

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail2.contoso.com" -Preference 10
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail3.contoso.com" -Preference 20
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="remove-a-record-from-an-existing-record-set"></a>Uklanjanje zapisa iz postojeći skup zapisa

Zapisi se mogu ukloniti iz zapisa postavite pomoću `Remove-AzureRmDnsRecordConfig`. Zapis koji se uklanja mora biti podudaranja s postojećeg zapisa u sve parametre. Promjene izvršene mora biti pomoću `Set-AzureRmDnsRecordSet`.

Uklanjanjem posljednji zapis u skupu zapisa izbrišite skup zapisa. Dodatne informacije potražite [Brisanje skup zapisa](#delete-a-record-set) .


    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "1.2.3.4"
    Set-AzureRmDnsRecordSet -RecordSet $rs

Slijed operacije da biste uklonili zapis u skupu zapisa možete također biti piped, što znači da se objekt skup zapisa prenesite pomoću kanala umjesto prosljeđivanje kao parametar. Ako, na primjer:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordConfig -Ipv4Address "1.2.3.4" | Set-AzureRmDnsRecordSet

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Uklanjanje zapisa poslovnog AAAA u skupu zapisa

    $rs = Get-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv6Address "2607:f8b0:4009:1803::1005"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-cname-record-from-a-record-set"></a>Uklanjanje CNAME zapis u skupu zapisa

Budući da skupu CNAME zapisa može sadržavati najviše jedan zapis, uklonite taj zapis ostavlja prazan skup zapisa.

    $rs =  Get-AzureRmDnsRecordSet -name "test-cname" -RecordType CNAME -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Cname "www.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-mx-record-from-a-record-set"></a>Uklanjanje MX zapis u skupu zapisa

    $rs = Get-AzureRmDnsRecordSet -name "test-mx" -RecordType MX -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Exchange "mail.contoso.com" -Preference 5
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-ns-record-from-record-set"></a>Uklanjanje zapisa poslovnog NS skup zapisa

    $rs = Get-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname "ns1.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-an-srv-record-from-a-record-set"></a>Uklanjanje SRV zapis u skupu zapisa

    $rs = Get-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs –Priority 0 –Weight 5 –Port 8080 –Target "sip.contoso.com"
    Set-AzureRmDnsRecordSet -RecordSet $rs

### <a name="remove-a-txt-record-from-a-record-set"></a>Uklanjanje TXT zapis u skupu zapisa

    $rs = Get-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Value "This is a TXT record"
    Set-AzureRmDnsRecordSet -RecordSet $rs

## <a name="delete-a-record-set"></a>Brisanje skupu zapisa

Skupove zapisa koje se mogu izbrisati pomoću na `Remove-AzureRmDnsRecordSet` cmdlet. Ne možete izbrisati s SOA i postavlja NS zapisa pri vrh zone (naziv = "@") koji su stvoreni automatski kada je stvorena u zonu. Oni izbrisat će se automatski ako je izbrisana zone.

Da biste uklonili skup zapisa, koristite jedan od sljedeća tri načina:

### <a name="specify-all-the-parameters-by-name"></a>Navedite sve parametre prema nazivu

Neobavezna *– prisilno* parametar može se koristiti izostavlja odzivnik za potvrdu.

    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup [-Force]


### <a name="specify-the-record-set-by-name-and-type-and-specify-the-zone-by-object"></a>Navesti skup slogova po naziv i vrsta, a u zonu objekt

    $zone = Get-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet -Name "test-a" -RecordType A -Zone $zone [-Force]

### <a name="specify-the-record-set-by-object"></a>Odredite zapis odredio objekta

    $rs = Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup
    Remove-AzureRmDnsRecordSet –RecordSet $rs [-Overwrite] [-Force]

Kada odredite zapisa postavite pomoću objekta, omogućuje provjere e-oznake da biste bili sigurni istovremene promjene neće se izbrisati. Neobavezna *-prebrisati* zastavice ukida te provjere. Dodatne informacije potražite u [Etags i oznake](dns-getstarted-create-dnszone.md#tagetag) .

Objekt skup zapisa može biti piped i umjesto predugački kao parametar:

    Get-AzureRmDnsRecordSet -Name "test-a" -RecordType A -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup | Remove-AzureRmDnsRecordSet [-Overwrite] [-Force]

## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o Azure DNS potražite u članku [Pregled Azure DNS-a](dns-overview.md). Informacije o automatizaciji DNS potražite u članku [Stvaranje DNS zone i zapis postavlja pomoću .NET SDK](dns-sdk.md).

Dodatne informacije o obrnutim DNS zapisa potražite [u](dns-reverse-dns-record-operations-ps.md)članku Upravljanje obrnutim DNS zapise za vaše servise pomoću komponente PowerShell.
