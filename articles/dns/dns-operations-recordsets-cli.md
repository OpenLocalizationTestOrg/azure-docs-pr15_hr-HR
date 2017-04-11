<properties
   pageTitle="Upravljanje DNS zapisa skupova i zapisa u DNS Azure pomoću Azure EŽA | Microsoft Azure"
   description="Upravljanje DNS zapisa skupova i zapisa u Azure DNS hostiranje vaše domene na Azure DNS-a. Sve naredbe EŽA za operacije na skupove zapisa i zapisa."
   services="dns"
   documentationCenter="na"
   authors="jtuliani"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/22/2016"
   ms.author="jtuliani"/>

# <a name="manage-dns-records-and-record-sets-by-using-cli"></a>Upravljanje DNS zapise i skupove zapisa pomoću EŽA


> [AZURE.SELECTOR]
- [Portal za Azure](dns-operations-recordsets-portal.md)
- [Azure EŽA](dns-operations-recordsets-cli.md)
- [PowerShell](dns-operations-recordsets.md)


U ovom se članku objašnjava upravljanje skupove zapisa i zapisa zone DNS-a pomoću različite platforme Azure sučelja naredbenog retka (EŽA).

Važno je da razlika između skupove zapisa u DNS-a i pojedinačne DNS zapisa. Skup zapisa je skup zapisa u zoni koji imaju isti naziv i imaju iste vrste. Dodatne informacije potražite u članku [objašnjenje zapisa skupova i zapisa](dns-getstarted-create-recordset-cli.md).


## <a name="configure-the-cross-platform-azure-cli"></a>Konfiguriranje EŽA za različite platforme Azure

Azure DNS je Azure samo za resursima servisa. Ne morate upravljanje API servisa Azure. Provjerite je li EŽA Azure konfiguriran za korištenje načina Voditelj resursa, pomoću na `azure config mode arm` naredbe.

Ako vidite **pogreška: 'dns' nije azure naredba**, je najvjerojatnije zato što koriste Azure EŽA u načinu rada za upravljanje servisom Azure, a ne u načinu Voditelj resursa.

## <a name="create-a-new-record-set-and-record"></a>Stvorite novi skup zapisa i zapisa

Da biste stvorili novi zapis postavljanje na portalu za Azure, potražite u članku [Stvaranje skup zapisa i zapisa](dns-getstarted-create-recordset-cli.md).


## <a name="retrieve-a-record-set"></a>Dohvaćanje skupu zapisa

Da biste dohvatili postojeći skup zapisa, koristite `azure network dns record-set show`. Određivanje grupa resursa, naziv zone zapisa postavite relativni naziv i vrstu zapisa. Koristite primjeru niže vrijednosti zamjenjuje vlastitu.

    azure network dns record-set show myresourcegroup contoso.com www A


## <a name="list-record-sets"></a>Popis zapisa skupovima

Sastavite popis svih zapisa u DNS zone pomoću na `azure network dns record-set list` naredbe. Morate navesti naziv grupe resursa i naziv zone.

### <a name="to-list-all-record-sets"></a>Da biste dobili popis sve skupove zapisa

U ovom se primjeru vraća sve skupove zapisa, bez obzira na naziv i vrsta zapisa:

    azure network dns record-set list myresourcegroup contoso.com

### <a name="to-list-record-sets-of-a-given-type"></a>Da biste popis zapisa skupove navedene vrste

U ovom se primjeru vraća sve skupove zapisa koje zadovoljavaju dane vrste zapisa (u ovom slučaju "U" zapisa):

    azure network dns record-set list myresourcegroup contoso.com A


## <a name="add-a-record-to-a-record-set"></a>Dodavanje zapisa u skupu zapisa

Dodavanje zapisa skupove zapisa pomoću na `azure network dns record-set add-record`naredbe. Parametri za dodavanje zapisa u skupu zapisa razlikuju se ovisno o vrsti zapisa koji je postavljen. Na primjer, kada koristite skup zapisa vrste "A", možete navesti samo zapise s parametrom `-a <IPv4 address>`.

Da biste stvorili skup zapisa, koristite na `azure network dns record-set create`naredbe. Određivanje grupa resursa, naziv zone zapisa postavite relativni naziv, vrstu zapisa i vrijeme Live (TTL). Ako u `--ttl` parametar nije definiran, vrijednost (u sekundama) zadano radi sa četiri.

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300


Kada stvorite "U" skup zapisa, dodavanje IPv4 adresa pomoću na `azure network dns record-set add-record`naredbe.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1


Sljedeći primjeri pokazuju kako stvoriti skup zapisa svaku vrstu zapisa. Svaki skup zapisa sadrži jedan zapis.

[AZURE.INCLUDE [dns-add-record-cli-include](../../includes/dns-add-record-cli-include.md)]


## <a name="update-a-record-in-a-record-set"></a>Ažuriranje zapisa u skupu zapisa

### <a name="to-add-another-ip-address-1234-to-an-existing-a-record-set-www"></a>Da biste dodali druge IP adrese (1.2.3.4) u postojeće "" zapisa postavite ("www"):

    azure network dns record-set add-record  myresourcegroup contoso.com  A
    -a 1.2.3.4
    info:    Executing command network dns record-set add-record
    Record set name: www
    + Looking up the dns zone "contoso.com"
    + Looking up the DNS record set "www"
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/a/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/a
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:        IPv4 address                : 192.168.1.1
    data:        IPv4 address                : 1.2.3.4
    data:
    info:    network dns record-set add-record command OK

### <a name="to-remove-an-existing-value-from-a-record-set"></a>Da biste uklonili postojeću vrijednost u skupu zapisa
Korištenje `azure network dns record-set delete-record`.

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 1.2.3.4
    info:    Executing command network dns record-set delete-record
    + Looking up the DNS record set "www"
    Delete DNS record? [y/n] y
    + Updating DNS record set "www"
    data:    Id                              : /subscriptions/################################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/A/www
    data:    Name                            : www
    data:    Type                            : Microsoft.Network/dnszones/A
    data:    Location                        : global
    data:    TTL                             : 4
    data:    A records:
    data:    IPv4 address                    : 192.168.1.1
    data:
    info:    network dns record-set delete-record command OK



## <a name="remove-a-record-from-a-record-set"></a>Uklanjanje zapisa u skupu zapisa

Zapisi se mogu ukloniti iz zapisa postavite pomoću `azure network dns record-set delete-record`. Zapis koji se uklanja mora biti podudaranja s postojećeg zapisa u sve parametre.

Uklanjanjem posljednji zapis u skupu zapisa izbrišite skup zapisa. Dodatne informacije potražite u odjeljku ovog članka pod nazivom [Brisanje skup zapisa](#delete).

    azure network dns record-set delete-record myresourcegroup contoso.com www A -a 192.168.1.1

    azure network dns record-set delete myresourcegroup contoso.com www A

### <a name="remove-an-aaaa-record-from-a-record-set"></a>Uklanjanje zapisa poslovnog AAAA u skupu zapisa

    azure network dns record-set delete-record myresourcegroup contoso.com test-aaaa  AAAA -b "2607:f8b0:4009:1803::1005"

### <a name="remove-a-cname-record-from-a-record-set"></a>Uklanjanje CNAME zapis u skupu zapisa

    azure network dns record-set delete-record myresourcegroup contoso.com test-cname CNAME -c www.contoso.com


### <a name="remove-an-mx-record-from-a-record-set"></a>Uklanjanje MX zapis u skupu zapisa

    azure network dns record-set delete-record myresourcegroup contoso.com "@" MX -e "mail.contoso.com" -f 5

### <a name="remove-an-ns-record-from-record-set"></a>Uklanjanje zapisa poslovnog NS skup zapisa

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-ns" NS -d "ns1.contoso.com"

### <a name="remove-a-ptr-record-from-a-record-set"></a>Uklanjanje PTR zapis u skupu zapisa
U ovom slučaju "Moje-arpa-zone.com" predstavlja zone ARPA koji predstavlja vaš raspon IP.  Svaki PTR zapis u ovoj zoni odgovara IP adresa u dosegu IP.

    azure network dns record-set delete-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"

### <a name="remove-an-srv-record-from-a-record-set"></a>Uklanjanje SRV zapis u skupu zapisa

    azure network dns record-set delete-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 -w 5 -o 8080 -u "sip.contoso.com"

### <a name="remove-a-txt-record-from-a-record-set"></a>Uklanjanje TXT zapis u skupu zapisa

    azure network dns record-set delete-record myresourcegroup contoso.com  "test-TXT" TXT -x "this is a TXT record"

## <a name="delete"></a>Brisanje skupu zapisa

Skupove zapisa koje se mogu izbrisati pomoću na `Remove-AzureRmDnsRecordSet` cmdlet. Ne možete izbrisati s SOA i postavlja NS zapisa pri vrh zone (naziv = "@") koji su stvoreni automatski kada je stvorena u zonu. Oni izbrisat će se automatski ako je izbrisana zone.

U sljedećem primjeru "U" zapisa postavite "test-a" bit će uklonjen s DNS zone "contoso.com":

    azure network dns record-set delete myresourcegroup contoso.com  "test-a" A

Neobavezni *-q* parametar može se koristiti izostavlja odzivnik za potvrdu.


## <a name="next-steps"></a>Daljnji koraci

Dodatne informacije o Azure DNS potražite u članku [Pregled Azure DNS-a](dns-overview.md). Informacije o automatizaciji DNS potražite u članku [Stvaranje DNS zone i zapis postavlja pomoću .NET SDK](dns-sdk.md).

Ako želite raditi s obrnutim DNS zapisa potražite u članku [Upravljanje obrnutim DNS zapise za vaše servise pomoću EŽA Azure](dns-reverse-dns-record-operations-cli.md).
