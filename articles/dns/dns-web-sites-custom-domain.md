<properties
   pageTitle="Stvaranje prilagođenih DNS zapisa za web-aplikaciji | Microsoft Azure  "
   description="Upute za stvaranje DNS zapisa za web-aplikacije pomoću Azure DNS prilagođenu domenu."
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

# <a name="create-dns-records-for-a-web-app-in-a-custom-domain"></a>Stvaranje DNS zapisa za web-aplikacije u prilagođenoj domeni

Azure DNS možete koristiti za hostiranje prilagođenu domenu za web apps. Ako, na primjer, stvarate Azure web-aplikaciju programa i želite da korisnici za pristup nekom korištenje contoso.com ili www.contoso.com kao programa FQDN.

Da biste to učinili, morate stvoriti dva zapisa:

- Zapis korijenski "A" koja pokazuje na contoso.com
- "CNAME" zapis za "www" naziv koji upućuje na A zapis

Imajte na umu da ako stvorili A zapis za web-aplikacije u Azure, A zapis mora biti ručno ažurirati ako podlozi IP adresa za promjene web app.

## <a name="before-you-begin"></a>Prije početka

Prije nego počnete, morate najprije stvorite DNS zone u Azure DNS i delegiranje zone u registraru Azure DNS.

1. Da biste stvorili DNS zone, slijedite korake u odjeljku [Stvaranje DNS zone](dns-getstarted-create-dnszone.md).
2. Delegatu DNS-a za Azure DNS-a, slijedite korake u [delegiranje DNS domene](dns-domain-delegation.md).

Nakon stvaranja zonu i prenošenjem za Azure DNS-a, pa možete stvoriti zapise za svoju prilagođenu domenu.


## <a name="1-create-an-a-record-for-your-custom-domain"></a>1. stvorili A zapis za prilagođenu domenu

A zapis koristi se za mapiranje naziva IP adrese. U sljedećem primjeru smo dodijeliti @ kao A zapis za IPv4 adresa:

### <a name="step-1"></a>Korak 1

Stvorili A zapis i dodijelite $rs tjednog prikaza kalendara

    $rs= New-AzureRMDnsRecordSet -Name "@" -RecordType "A" -ZoneName "contoso.com" -ResourceGroupName "MyAzureResourceGroup" -Ttl 600

### <a name="step-2"></a>Korak 2

Dodajte vrijednost IPv4 u prethodno stvorena skup zapisa "@" korištenjem varijable $rs dodijeljeni. IPv4 vrijednost dodijeljeno bit će IP adresa za web-aplikacije.

Da biste pronašli IP adresa za web-aplikacije, slijedite korake u članku [Konfiguriranje prilagođenog naziva domene na servisu Azure aplikacije](../web-sites-custom-domain-name.md#Find-the-virtual-IP-address).

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Ipv4Address <your web app IP address>

### <a name="step-3"></a>Korak 3

Pohranite promjene skup zapisa. Korištenje `Set-AzureRMDnsRecordSet` da biste prenijeli promjene na postavljanje Azure DNS zapis:

    Set-AzureRMDnsRecordSet -RecordSet $rs

## <a name="2-create-a-cname-record-for-your-custom-domain"></a>2. stvorite CNAME zapis za prilagođenu domenu

Ako domenu već upravlja DNS-a Azure (potražite u članku [delegiranje DNS domene](dns-domain-delegation.md), možete koristiti sljedeće primjer stvorite CNAME zapis za contoso.azurewebsites.net.

### <a name="step-1"></a>Korak 1

Otvorite ljusku PowerShell i stvaranje novog skupa CNAME zapisa i dodjeljivanje $rs tjednog prikaza kalendara. U ovom se primjeru stvoriti vrstu skup zapisa CNAME s"Live" 600 sekundi u DNS zone pod nazivom "contoso.com".

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "www" -RecordType "CNAME" -Ttl 600

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Korak 2

Nakon stvaranja CNAME zapisa postavljanje potrebnih za stvaranje vrijednost pseudonim koji će pokažite na web-aplikaciji.

Korištenjem prethodno dodijeljene varijable "$rs" koristite naredbu komponente PowerShell da biste stvorili pseudonim za contoso.azurewebsites.net aplikaciju za web.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "contoso.azurewebsites.net"

    Name              : www
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Korak 3

Zapiši promjene pomoću na `Set-AzureRMDnsRecordSet` cmdlet:

    Set-AzureRMDnsRecordSet -RecordSet $rs

Možete provjeriti zapis je pravilno stvoren pomoću upita pomoću alata nslookup, "www.contoso.com" kao što je prikazano u nastavku:

    PS C:\> nslookup
    Default Server:  Default
    Address:  192.168.0.1

    > www.contoso.com
    Server:  default server
    Address:  192.168.0.1

    Non-authoritative answer:
    Name:    <instance of web app service>.cloudapp.net
    Address:  <ip of web app service>
    Aliases:  www.contoso.com
    contoso.azurewebsites.net
    <instance of web app service>.vip.azurewebsites.windows.net

## <a name="create-an-awverify-record-for-web-apps"></a>Stvorite zapis "awverify" za web-aplikacije


Ako odlučite koristiti A zapis za web-aplikacije, morate proći kroz postupak provjere valjanosti da biste bili sigurni da ste vlasnik prilagođenu domenu. Ovaj korak za potvrdu dovršetka stvaranjem posebno CNAME zapis pod nazivom "awverify". U ovom se odjeljku primjenjuje se samo zapisa.


### <a name="step-1"></a>Korak 1

Stvorite zapis "awverify". U primjeru u nastavku ćemo će stvoriti zapis "aweverify" za contoso.com potvrđuje vlasništvo za prilagođenu domenu.

    $rs = New-AzureRMDnsRecordSet -ZoneName contoso.com -ResourceGroupName myresourcegroup -Name "awverify" -RecordType "CNAME" -Ttl 600

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee1856
    RecordType        : CNAME
    Records           : {}
    Tags              : {}


### <a name="step-2"></a>Korak 2

Nakon stvaranja skup zapisa "awverify", dodijelite CNAME zapis postaviti pseudonim. U primjeru u nastavku ćemo će dodijeliti postavite pseudonim awverify.contoso.azurewebsites.net CNAMe zapis.

    Add-AzureRMDnsRecordConfig -RecordSet $rs -Cname "awverify.contoso.azurewebsites.net"

    Name              : awverify
    ZoneName          : contoso.com
    ResourceGroupName : myresourcegroup
    Ttl               : 600
    Etag              : 8baceeb9-4c2c-4608-a22c-229923ee185
    RecordType        : CNAME
    Records           : {awverify.contoso.azurewebsites.net}
    Tags              : {}

### <a name="step-3"></a>Korak 3

Zapiši promjene pomoću na `Set-AzureRMDnsRecordSet cmdlet`, kao što je prikazano u nastavku naredbi.

    Set-AzureRMDnsRecordSet -RecordSet $rs



## <a name="next-steps"></a>Daljnji koraci

Slijedite korake u [konfiguraciji prilagođenog naziva domene za aplikacije servisa za](../app-service-web/web-sites-custom-domain-name.md) konfiguriranje web-aplikaciju u programa za potvrdu da koristi prilagođenu domenu.








