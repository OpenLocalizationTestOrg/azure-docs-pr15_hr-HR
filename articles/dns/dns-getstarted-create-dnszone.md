<properties
   pageTitle="Početak rada s Azure DNS | Microsoft Azure"
   description="Saznajte kako stvoriti DNS zone za Azure DNS. Ovo je korak po korak da biste stvorili da biste pokrenuli hostiranje DNS-a domene pomoću komponente PowerShell sustava prvi DNS zone."
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

# <a name="create-a-dns-zone-using-powershell"></a>Stvaranje DNS zone pomoću komponente Powershell

> [AZURE.SELECTOR]
- [Portal za Azure](dns-getstarted-create-dnszone-portal.md)
- [PowerShell](dns-getstarted-create-dnszone.md)
- [Azure EŽA](dns-getstarted-create-dnszone-cli.md)

U ovom se članku će vas voditi kroz korake da biste stvorili DNS zone pomoću komponente PowerShell. Možete stvoriti i pomoću EŽA ili portala za Azure DNS zone.

[AZURE.INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="tagetag"></a>O Etags i oznaka

### <a name="etags"></a>Etags

Pretpostavimo da dvije osobe ili dva procesa, pokušajte da biste izmijenili DNS zapisa u isto vrijeme. Koji je pobijedio? I ne pića znate da su ste upravo prebrisati promjene stvorio netko drugi?

Azure DNS koristi Etags sigurno učiniti istovremene promjene isti resurs. Svaki resurs DNS (zone ili skup zapisa) ima e-oznake pridružen. Kad god se dohvaća resursa, njegov e-oznake i dohvatiti. Kada ažurirate resursa, imate mogućnost za prosljeđivanje natrag na e-oznake tako Azure DNS možete provjeriti jesu u e-oznake na poslužitelju podudaranja. Budući da svakom ažuriranju resursu rezultira e-oznake koja se regenerated, ne podudaraju se e-oznake označava došlo je do istovremene promjene. Etags se koriste prilikom stvaranja novog resursa da biste bili sigurni da je resurs još ne postoji.

Azure DNS PowerShell po zadanom koristi Etags blokira istovremene promjene zone i snimite skupove. Neobavezna *-prebrisati* parametar može se koristiti za izostavi provjere e-oznake, u tom se slučaju istovremene promjene koje su se pojavile prebrisat će se.

Na razini Azure DNS REST API-JA, Etags određeni su klauzulom pomoću HTTP zaglavlja.  Njihovo ponašanje izražen je u tablici u nastavku:

|Zaglavlje|Ponašanje|
|------|--------|
|Ništa|STAVI uvijek slijedi (bez provjere e-oznake)|
|Ako rezultat<etag>|STAVI uspijeva samo ako postoji resursa i uspoređuje e-oznake|
|Ako rezultat *     | STAVI uspijeva samo ako postoji resursa|
|IF-ništa – match * |  STAVI uspijeva samo ako ne postoji resursa|

### <a name="tags"></a>Oznaka

Oznake razlikuju se od Etags. Oznake su popis parove naziv vrijednosti i koriste Azure resursa Upravitelj na natpis resurse za naplatu ili grupiranja svrhe. Dodatne informacije o oznake potražite u članku [Korištenje oznake da biste organizirali Azure resurse](../resource-group-using-tags.md).

Azure DNS PowerShell podržava oznake na zone i skupove zapisa navedene pomoću mogućnosti `-Tag` parametar.


## <a name="before-you-begin"></a>Prije početka

Provjerite imate li sljedeće stavke prije početka konfiguraciju.

- Azure pretplate. Ako već nemate Azure pretplatu, možete aktivirati [MSDN pretplatnika pogodnosti](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ili znak prema gore za [besplatan račun](https://azure.microsoft.com/pricing/free-trial/).

- Morat ćete instalirati najnoviju verziju programa Azure resursima PowerShell cmdleti (1.0 ili noviji). Dodatne informacije o instaliranju cmdleta ljuske PowerShell potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md) .

## <a name="step-1---sign-in"></a>Korak 1 - Prijava

Otvorite konzole za PowerShell i povezati s računom. Dodatne informacije potražite u članku [Pomoću komponente Windows PowerShell s Voditelj resursa](../powershell-azure-resource-manager.md).

Pomoću sljedeće ogledne možete povezati:

    Login-AzureRmAccount

Provjerite pretplate za račun.

    Get-AzureRmSubscription

Navedite pretplatu u koju želite koristiti.

    Select-AzureRmSubscription -SubscriptionName "Replace_with_your_subscription_name"

## <a name="step-2---create-a-resource-group"></a>Korak 2 – da biste stvorili grupu resursa

Azure Voditelj resursa zahtijeva da svih grupa resursa navedite mjesto. Koristi se kao zadano mjesto za resurse u toj grupi resursa. Međutim, budući da svi resursi DNS globalni, ne regionalnih, odabir resursa grupi mjesto ima bez utjecaja na Azure DNS.

Ako koristite postojeću grupu resursa, preskočite ovaj korak.

    New-AzureRmResourceGroup -Name MyAzureResourceGroup -location "West US"


## <a name="step-3---register"></a>Korak 3 – Register

Azure DNS servis upravlja davatelj Microsoft.Network resursa. Pretplate Azure mora biti registriran da biste koristili ovu davatelja resursa mogli koristiti Azure DNS-a. Ovo je jednokratni postupak za svaku pretplatu.

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network


## <a name="step-4----create-a-dns-zone"></a>Korak 4 – da biste stvorili DNS zone

DNS zone stvoren je pomoću na `New-AzureRmDnsZone` cmdlet. Postoje Primjeri dolje za stvaranje DNS zone sa ili bez oznaka. Dodatne informacije o oznaka, u odjeljku [oznake](#tags) u ovom članku.

>[AZURE.NOTE] U Azure DNS zone imena treba navesti bez na prekidanje **"."**. Na primjer, kao "**contoso.com**" umjesto "**contoso.com.**".

### <a name="to-create-a-dns-zone"></a>Da biste stvorili DNS zone

U primjeru u nastavku stvara DNS zone naziva *contoso.com* u grupu resursa pod nazivom *MyResourceGroup*. U primjeru možete koristiti za stvaranje DNS zone, zamjenjujući vlastite vrijednosti.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup

### <a name="to-create-a-dns-zone-with-tags"></a>Da biste stvorili DNS zone s oznakama

Sljedeći primjer pokazuje kako stvoriti DNS zone dva oznakama *projekta = pokazni videozapis* i *env = test*. U primjeru možete koristiti za stvaranje DNS zone, zamjenjujući vlastite vrijednosti.

    New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyAzureResourceGroup -Tag @( @{ Name="project"; Value="demo" }, @{ Name="env"; Value="test" } )

## <a name="view-records"></a>Prikaz zapisa

Stvaranje DNS zone stvara se i sljedeći DNS zapisi:

- Zapis za *Pokretanje programa za izdavanje certifikata* (SOA). Ovo je prezentacija u korijenu svaki DNS zone.

- Na mjerodavne zapisa poslužitelja naziva (NS) zapisa. Te prikazati koji naziv poslužitelja hostira zone. Azure DNS koristi skup poslužitelje naziva i tako da drugi poslužitelje naziva može biti dodijeljena različitim zonama u Azure DNS. Potražite u članku [delegatu domene Azure DNS](dns-domain-delegation.md) dodatne informacije.

Da biste prikazali ove zapise, koristite `Get-AzureRmDnsRecordSet`:

    Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyAzureResourceGroup

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 2b855de1-5c7e-4038-bfff-3a9e55b49caf
    RecordType        : SOA
    Records           : {[ns1-01.azure-dns.com,msnhst.microsoft.com,900,300,604800,300]}
    Tags              : {}

    Name              : @
    ZoneName          : contoso.com
    ResourceGroupName : MyResourceGroup
    Ttl               : 3600
    Etag              : 5fe92e48-cc76-4912-a78c-7652d362ca18
    RecordType        : NS
    Records           : {ns1-01.azure-dns.com, ns2-01.azure-dns.net, ns3-01.azure-dns.org,
                  ns4-01.azure-dns.info}
    Tags              : {}


Zapis postavlja na korijenskom (ili *Vrh*) DNS Zone korištenja **@** kao što je naziv skupa zapisa.


## <a name="test"></a>Test

Pomoću alata za DNS kao što su nslookup, istražujte ili [cmdlet ljuske PowerShell za rješavanje DnsName](https://technet.microsoft.com/library/jj590781.aspx)možete testirati i DNS zone.

Ako još niste delegirani vaše domene koje želite koristiti novu zonu u Azure DNS-a, morat ćete izravno DNS upit izravno na neki od poslužitelje naziva zone. Poslužitelje naziva zone ponudit će vam u NS zapise, kao što je navedeno po `Get-AzureRmDnsRecordSet` iznad. Je li zamjenu odgovarajuće vrijednosti za vašu zonu u nastavku naredbu.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = msnhst.microsoft.com
            serial  = 1
            refresh = 900 (15 mins)
            retry   = 300 (5 mins)
            expire  = 604800 (7 days)
            default TTL = 300 (5 mins)


## <a name="next-steps"></a>Daljnji koraci

Nakon stvaranja DNS zone, stvaranje [skupa zapisa i zapisa](dns-getstarted-create-recordset.md) da biste pokrenuli raščlanjuju imena za svoju domenu Internet.

