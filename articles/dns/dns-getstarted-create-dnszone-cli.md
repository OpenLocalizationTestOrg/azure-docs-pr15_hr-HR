<properties
   pageTitle="Stvaranje DNS zone pomoću EŽA | Microsoft Azure"
   description="Saznajte kako stvoriti DNS zone za Azure DNS korak po korak da biste pokrenuli hostiranje DNS-a domene pomoću EŽA"
   services="dns"
   documentationCenter="na"
   authors="sdwheeler"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/16/2016"
   ms.author="sewhee"/>

# <a name="create-an-azure-dns-zone-using-cli"></a>Stvaranje programa Azure DNS zone pomoću EŽA


> [AZURE.SELECTOR]
- [Portal za Azure](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure EŽA](dns-getstarted-create-dnszone-cli.md)


U ovom se članku će vas voditi kroz korake da biste stvorili DNS zone pomoću EŽA. Možete stvoriti i pomoću programa PowerShell ili portala za Azure DNS zone.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]


## <a name="before-you-begin"></a>Prije početka

Ove upute koristite Microsoft Azure EŽA. Ne zaboravite ažurirati na najnovije Azure EŽA (0.9.8 ili noviji) za korištenje naredbi za Azure DNS-a. Vrsta `azure -v` da biste provjerili koju verziju Azure EŽA trenutno instalirani na vašem računalu.

## <a name="step-1---set-up-azure-cli"></a>Korak 1 – postavljanje EŽA Azure

### <a name="1-install-azure-cli"></a>1. instalirati Azure EŽA

Možete instalirati Azure EŽA za Windows, Linux ili Mac Sljedeći koraci nije potrebno dovršiti prije nego što možete upravljati DNS Azure pomoću Azure EŽA. Dodatne informacije o dostupna je na [instalirati EŽA Azure](../xplat-cli-install.md). Naredbe DNS-a potreban Azure EŽA verziju 0.9.8 ili noviji.

Sve naredbe davatelja mreže na EŽA mogu pronaći i pomoću sljedeće naredbe:

    azure network

### <a name="2-switch-cli-mode"></a>2. način EŽA promjenu

Azure DNS koristi Azure Voditelj resursa. Provjerite je li prebaciti EŽA način za korištenje OKVIRA naredbi.

    azure config mode arm

### <a name="3-sign-in-to-your-azure-account"></a>3. prijavite se na račun za Azure

Zatražit će se za provjeru s vjerodajnice. Imajte na umu da možete koristiti samo ORGID računa.

    azure login -u "username"

### <a name="4-select-the-subscription"></a>4. Odaberite pretplatu

Odabir pretplate Azure da biste koristili.

    azure account set "subscription name"

### <a name="5-create-a-resource-group"></a>5. Stvaranje grupa resursa

Azure Voditelj resursa zahtijeva da svih grupa resursa navedite mjesto. Koristi se kao zadano mjesto za resurse u toj grupi resursa. Međutim, budući da svi resursi DNS globalni, ne regionalnih, odabir resursa grupi mjesto ima bez utjecaja na Azure DNS.

Ako koristite postojeću grupu resursa, preskočite ovaj korak.

    azure group create -n myresourcegroup --location "West US"


### <a name="6-register"></a>6. dnevnik

Azure DNS servis upravlja davatelj Microsoft.Network resursa. Pretplate Azure mora biti registriran da biste koristili ovu davatelja resursa mogli koristiti Azure DNS-a. Ovo je jednokratni postupak za svaku pretplatu.

    azure provider register --namespace Microsoft.Network


## <a name="step-2---create-a-dns-zone"></a>Korak 2 – da biste stvorili DNS zone

DNS zone je stvorena u `azure network dns zone create` naredbe. Po želji možete stvoriti DNS zone zajedno s oznakama. Oznake su popis parove naziv vrijednosti i koriste Azure resursa Upravitelj na natpis resurse za naplatu ili grupiranja svrhe. Dodatne informacije o oznake potražite u članku [Korištenje oznake da biste organizirali Azure resurse](../resource-group-using-tags.md).

U Azure DNS zone imena treba navesti bez na prekidanje **"."**. Na primjer, kao "**contoso.com**" umjesto "**contoso.com.**".


### <a name="to-create-a-dns-zone"></a>Da biste stvorili DNS zone

U primjeru u nastavku stvara DNS zone naziva *contoso.com* u grupu resursa pod nazivom *MyResourceGroup*.

U primjeru možete koristiti za stvaranje DNS zone, zamjenjujući vlastite vrijednosti.

    azure network dns zone create myresourcegroup contoso.com

### <a name="to-create-a-dns-zone-and-tags"></a>Da biste stvorili DNS zone i oznake.

Azure DNS EŽA podržava oznake DNS zone naveden pomoću neobavezna *-oznaka* parametar. Sljedeći primjer prikazuje način da biste stvorili DNS zone s dvije oznake, project = pokazni videozapis i env = test.

Koristite primjeru u nastavku da biste stvorili DNS zone i oznake, zamjenjujući vlastite vrijednosti.

    azure network dns zone create myresourcegroup contoso.com -t "project=demo";"env=test"

## <a name="view-records"></a>Prikaz zapisa

Stvaranje DNS zone stvara se i sljedeći DNS zapisi:

- Zapis 'Pokretanje izvora' (SOA). Ovo je prezentacija u korijenu svaki DNS zone.

- Na mjerodavne zapisa poslužitelja naziva (NS) zapisa. Te prikazati koji naziv poslužitelja hostira zone. Azure DNS koristi skup poslužitelje naziva i tako da drugi poslužitelje naziva se mogu dodijeliti različite zona u Azure DNS-a. Dodatne informacije potražite u članku [delegat domene Azure DNS-a](dns-domain-delegation.md) .

Da biste prikazali ove zapise, koristite `azure network dns-record-set show`.<BR>
*Korištenje: mreže dns skup zapisa prikaz < resursa grupni >< dns zone-ime > <name><type>*


U primjeru u nastavku ako pokrenete naredbu s resursa za grupu *myresourcegroup*, zapis naziv skupa *"@"* (za korijenske zapis), i upišite *SOA*, ona će yield sljedeći rezultat:


    azure network dns record-set show myresourcegroup "contoso.com" "@" SOA
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/SOA/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/SOA
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    SOA record:
    data:      Email                         : msnhst.microsoft.com
    data:      Expire time                   : 604800
    data:      Host                          : edge1.azuredns-cloud.net
    data:      Minimum TTL                   : 300
    data:      Refresh time                  : 900
    data:      Retry time                    : 300
    data:                                    :
<BR>
Da biste pogledali NS zapise stvorene pomoću zoni, koristite sljedeću naredbu:

    azure network dns record-set show myresourcegroup "contoso.com" "@" NS
    info:    Executing command network dns-record-set show
    + Looking up the DNS record set "@"
    data:    Id                              : /subscriptions/#######################/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com/NS/@
    data:    Name                            : @
    data:    Type                            : Microsoft.Network/dnszones/NS
    data:    Location                        : global
    data:    TTL                             : 3600
    data:    NS records
    data:        Name server domain name     : ns1-05.azure-dns.com
    data:        Name server domain name     : ns2-05.azure-dns.net
    data:        Name server domain name     : ns3-05.azure-dns.org
    data:        Name server domain name     : ns4-05.azure-dns.info
    data:
    info:    network dns-record-set show command OK

>[AZURE.NOTE] Zapis postavlja na korijenskom (ili *Vrh*) DNS Zone korištenja **@** kao što je naziv skupa zapisa.

## <a name="test"></a>Test

Možete testirati i DNS zone pomoću alata za DNS kao što su nslookup, ISTRAŽUJTE, ili `Resolve-DnsName` cmdlet ljuske PowerShell.

Ako još niste delegirani vaše domene koje želite koristiti novu zonu u Azure DNS, morate Izravni DNS upit izravno na neki od poslužitelje naziva zone. Poslužitelje naziva zone su dani NS zapise, kao što je naveden po "Prikaži skup zapisa dns azure mreže". Je li zamjenu odgovarajuće vrijednosti za vašu zonu u naredbi u nastavku.

Sljedeći primjer koristi ISTRAŽUJTE upita za domenu contoso.com pomoću poslužitelje naziva dodijeljeni DNS zone. Upit ne može pokažite na naziv poslužitelja za koji koristi * @ * i pomoću ISTRAŽUJTE naziv zone.

     <<>> DiG 9.10.2-P2 <<>> @ns1-05.azure-dns.com contoso.com
    (1 server found)
    global options: +cmd
    Got answer:
    ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 60963
    flags: qr aa rd; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1
    WARNING: recursion requested but not available

    OPT PSEUDOSECTION:
    EDNS: version: 0, flags:; udp: 4000
    QUESTION SECTION:
    contoso.com.                        IN      A

    AUTHORITY SECTION:
    contoso.com.         300     IN      SOA     edge1.azuredns-cloud.net.
    msnhst.microsoft.com. 6 900 300 604800 300

    Query time: 93 msec
    SERVER: 208.76.47.5#53(208.76.47.5)
    WHEN: Tue Jul 21 16:04:51 Pacific Daylight Time 2015
    MSG SIZE  rcvd: 120

## <a name="next-steps"></a>Daljnji koraci

Nakon stvaranja DNS zone, stvaranje [skupa zapisa i zapisa](dns-getstarted-create-recordset-cli.md) da biste pokrenuli raščlanjuju imena za svoju domenu Internet.
