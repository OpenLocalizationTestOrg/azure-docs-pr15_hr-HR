<properties
   pageTitle="Azure Voditelj resursa zahtjev ograničenja | Microsoft Azure"
   description="U članku se opisuje kako koristiti ograničavanje Voditelj resursa Azure zahtjevima do ograničenja pretplate."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="tomfitz"/>

# <a name="throttling-resource-manager-requests"></a>Ograničavanje zahtjeve Voditelj resursa

Za svaku pretplatu i klijentu resursima ograničenja čitanja zahtjevi za 15.000 sat i pisanja zahtjeva za iznos od 1200 po satu. Ako aplikacija ili skriptu dostigne ta ograničenja, morate throttle vašim zahtjevima. Ova tema prikazuje kako utvrditi imate prije nego što ograničenje zahtjeva za preostale te kako reagirati kada ste dostigli ograničenje.

Kada dođete do ograničenje, primit ćete Šifra stanja HTTP **429 Too zahtjeva za mnoge**.

Broj zahtjeva za implementaciju ograničen je na pretplatu ili vaš klijent. Ako imate više, Istodobni aplikacije upućivanje zahtjeva za pretplatu, zahtjeve iz te aplikacije dodat će se zajedno da biste odredili broj zahtjeva za preostale.

Pretplata je opsegu zahtjevi se one involve prosljeđivanje id pretplate, kao što su dohvaćanje resursa grupe u svoju pretplatu. Zahtjevi za klijent iz djelokruga obuhvaćaju id pretplate, kao što su dohvaćanje valjani Azure mjesta.

## <a name="remaining-requests"></a>Preostali zahtjeva

Broj zahtjeva za preostale možete utvrditi tako Provjera zaglavlja odgovor. Svaki zahtjev sadrži vrijednosti za broj preostale čitanje i zahtjevi za pisanje. U sljedećoj tablici opisane odgovor zaglavlja možete provjeriti za te vrijednosti:

| Zaglavlja odgovora | Opis |
| --------------- | ----------- |
| x-MS-ratelimit-remaining-Subscription-reads | Pretplate iz djelokruga čita preostalo |
| x-MS-ratelimit-remaining-Subscription-writes | Pretplate iz djelokruga piše preostalo |
| x-MS-ratelimit-remaining-tenant-reads | Klijent iz djelokruga čita preostalo |
| x-MS-ratelimit-remaining-tenant-writes | Klijent iz djelokruga piše preostalo |
| x-MS-ratelimit-remaining-Subscription-Resource-Requests | Pretplata ograničena resursa Vrsta zahtjeva preostalo.<br /><br />Ta vrijednost zaglavlja vraća samo ako uslugu sadrži nadjačati zadanog ograničenja. Voditelj resursa dodaje tu vrijednost umjesto pretplate čitanja ili zapisivanja. |
| x-MS-ratelimit-remaining-Subscription-Resource-Entities-Read | Pretplata ograničena resursa Vrsta zahtjeva za zbirke preostalo.<br /><br />Ta vrijednost zaglavlja vraća samo ako uslugu sadrži nadjačati zadanog ograničenja. Ta vrijednost sadrži broj preostale zbirke zahtjeva (popis resursa). |
| x-MS-ratelimit-remaining-tenant-Resource-Requests | Klijent ograničena resursa Vrsta zahtjeva preostalo.<br /><br />Ovo zaglavlje samo dodaje zahtjeva na razini klijenta za, a samo ako je neki servis je nadjačati zadanog ograničenja. Voditelj resursa dodaje tu vrijednost umjesto klijentu čitanja ili zapisivanja. |
| x-MS-ratelimit-remaining-tenant-Resource-Entities-Read | Klijent ograničena resursa Vrsta zahtjeva za zbirke preostalo.<br /><br />Ovo zaglavlje samo dodaje zahtjeva na razini klijenta za, a samo ako je neki servis je nadjačati zadanog ograničenja. |

## <a name="retrieving-the-header-values"></a>Dohvaćanje vrijednosti za zaglavlja

Dohvaćanje vrijednosti te zaglavlja u kod ili skriptu je ne razlikuje se od dohvaćanje bilo koja vrijednost zaglavlja. 

Ako, na primjer, u **C#**, dohvatite vrijednost zaglavlja iz objekta **HttpWebResponse** pod nazivom **odgovor** sljedeći kod:

    response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)

U **PowerShell**dohvatiti vrijednost zaglavlja s pozovite WebRequest operacije.

    $r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
    $r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
    
Ili, ako želite vidjeti preostale zahtjeva za ispravljanje pogrešaka, možete unijeti na **-ispravljanje pogrešaka** parametar na vašem cmdleta **ljuske PowerShell** .

    Get-AzureRmResourceGroup -Debug
    
Koja vraća velike količine podataka, uključujući sljedeću vrijednost odgovor:

    ...
    DEBUG: ============================ HTTP RESPONSE ============================

    Status Code:
    OK

    Headers:
    Pragma                        : no-cache
    x-ms-ratelimit-remaining-subscription-reads: 14999
    ...

U **EŽA Azure**pomoću mogućnosti više opširno dohvatiti vrijednost zaglavlja.

    azure group list -vv --json

Koja vraća velike količine podataka, uključujući sljedeće objekta:

    ...
    silly: returnObject
    {
      "statusCode": 200,
      "header": {
        "cache-control": "no-cache",
        "pragma": "no-cache",
        "content-type": "application/json; charset=utf-8",
        "expires": "-1",
        "x-ms-ratelimit-remaining-subscription-reads": "14998",
        ...

## <a name="waiting-before-sending-next-request"></a>Čekanje prije no što pošaljete zahtjev za sljedeće

Kada dođete do ograničenje zahtjev, resursima vraća Šifra stanja **429** HTTP i **Pokušajte ponovno nakon** vrijednosti u zaglavlju. Na **Pokušaj nakon** vrijednost određuje broj sekundi prije nego što treba čekati aplikacije (ili stanje mirovanja) prije no što pošaljete zahtjev za sljedeće. Ako pošaljete zahtjev prije isteka pokušaj vrijednost, vaš zahtjev nije obrađen i novu vrijednost pokušajte ponovno.
