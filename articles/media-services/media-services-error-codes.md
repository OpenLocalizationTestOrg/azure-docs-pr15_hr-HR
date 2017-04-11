<properties
    pageTitle="Kodovi pogrešaka Azure Media Services | Microsoft Azure"
    description="U temi daje pregled kodovi pogreške servisa Azure Media Services."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>

# <a name="azure-media-services-error-codes"></a>Kodovi pogrešaka Azure Media Services

Kada pomoću servisa Microsoft Azure Media Services, možete dobiti šifre pogrešaka HTTP iz servisa ovisno o problemima kao što su istječe za akcije koje nisu podržane u Media Services tokeni za provjeru autentičnosti. **Kodovi pogrešaka HTTP** kojih može doći Media Services i o mogućim uzrocima za njih je.  
  
## <a name="400-bad-request"></a>400 Neispravan zahtjev

Zahtjev sadrži podatke koji nisu valjani i Odbaci zbog nekog od sljedećih razloga:

- Navedeni su nepodržanu verziju API-JA. Najnovija verzija, potražite u članku [Postavljanje Media Services REST API razvoj](media-services-rest-how-to-use.md).
- API verziju Media Services nije naveden. Informacije o tome kako odrediti API verzija, potražite u članku [Povezivanje sa Media Services s Media Services REST API -JA](media-services-rest-connect-programmatically.md). 
   
    >[AZURE.NOTE] Ako koristite .NET ili Java SDK-ovi za povezivanje s Media Services, API verzija navedena je za vas kad god pokušajte i izvršiti neke akcije na temelju Media Services.
- Nedefinirano svojstava nije naveden. Naziv svojstva se poruka o pogrešci. Moguće je navesti samo svojstva koja su članovi pripadni entitet. Popis entiteti i njihovim svojstvima potražite u članku [Azure Media Services REST API Reference](http://msdn.microsoft.com/library/azure/hh973617.aspx) .
- Vrijednost svojstva koje nisu valjane nije naveden. Naziv svojstva se poruka o pogrešci. Pogledajte prethodnu vezu za valjano svojstvo vrste i njihove vrijednosti.
- Vrijednost svojstva nedostaje i potreban je.
- Dio URL-a naveden sadrži neispravni vrijednost.
- Došlo je do pokušaja ažuriranje WriteOnce svojstva.
- Došlo je do pokušaja stvoriti zadatak koji je unos resursa s primarni AssetFile koja nije navedena ili nije moguće odrediti.
- Došlo je do pokušaja ažuriranje SAS Locator. SAS locators mogu samo stvoriti ili izbrisati. Strujanje locators može ažurirati. Dodatne informacije potražite u članku [Locators](http://msdn.microsoft.com/library/azure/hh974308.aspx).
- Nepodržane operacija ili upit je poslana. 

## <a name="401-unauthorized"></a>401 neovlašteno

Zahtjev nije moguće provjeriti autentičnost (prije se može biti ovlašteni) zbog nekog od sljedećih razloga:

- Nedostaje zaglavlje provjere autentičnosti.
- Neispravni provjere autentičnosti vrijednost zaglavlja.
    - Token istekao. Ako koristite REST API-ji izravno, potražite u članku [Povezivanje sa Media Services s Media Services REST API -JA](media-services-rest-connect_programmatically.md) da biste saznali kako stvoriti novi token za provjeru autentičnosti. Ako koristite .NET ili Java SDK-ovi, stvorite CloudMediaContext ili MediaContract objekt da biste generirali token. Dodatne informacije o tome kako to učiniti potražite u članku [Povezivanje sa Media Services s Media Services SDK za .NET](media-services-dotnet-connect-programmatically.md).
    - Token potpis nije valjan.</li></ul></li></ul>

## <a name="403-forbidden"></a>403 zabranjeno

Zahtjev nije dopuštena zbog nekog od sljedećih razloga:

- Media Services račun nije moguće pronaći ili je izbrisana.
- Račun servisa Media Services je onemogućen, a zatim vrstu zahtjeva nije HTTP GET. Operacije servisa koji će vratiti kao i 403 odgovor.
- Token za provjeru autentičnosti sadrže korisničke podatke vjerodajnica: AccountName i/ili SubscriptionId. Ove informacije možete pronaći u proširenje korisničkog Sučelja Media Services za račun na portalu za upravljanje Azure Media Services.
- Nije moguće pristupiti resurs.
    - Došlo je do pokušaja pomoću MediaProcessor koja nije dostupna za vaš račun Media Services.
    - Došlo je do pokušaja ažuriranje JobTemplate definira Media Services.
    - Došlo je do pokušaja prebrisati Locator neke druge Media Services računa.
    - Došlo je do pokušaja prebrisati ContentKey neke druge Media Services računa.

- Resurs nije moguće stvoriti zbog ograničenja servisa koje je dostupno za račun servisa Media Services. Dodatne informacije o kvotama servisa potražite u članku [kvotama i ograničenja](media-services-quotas-and-limitations.md).

## <a name="404-not-found"></a>404 nije pronađeno

Zahtjev nije dopuštena na resursa zbog nekog od sljedećih razloga:

- Došlo je do pokušaja ažuriranje entitet koji ne postoji.
- Došlo je do pokušaja brisanje entitet koji ne postoji.
- Došlo je do pokušaja stvaranje entitet koja se povezuje s entitet koji ne postoji.
- Došlo je do pokušaja DOBITI entitet koji ne postoji.
- Došlo je do pokušaja Navedite račun za pohranu koji je povezan s računa za servise za medijske sadržaje.  

## <a name="409-conflict"></a>409 sukoba

Zahtjev nije dopuštena zbog nekog od sljedećih razloga:

- Više od jedne AssetFile ima čiji je naziv naveden unutar sredstava.
- Da biste stvorili drugi primarni AssetFile unutar imovine Izvršen je pokušaj.
- Došlo je do pokušaja stvoriti na ContentKey s navedenom Id koji se već koristi.
- Pokušaj unesena da biste stvorili na Locator navedeni Id koji se već koristi.
- Više od jedne IngestManifestFile ima čiji je naziv naveden unutar na IngestManifest.
- Da biste se povezali drugi prostora za pohranu šifriranje ContentKey resursa za pohranu šifrirane Izvršen je pokušaj.
- Da biste se povezali iste ContentKey imovine Izvršen je pokušaj.
- Došlo je do pokušaja stvaranje locator na imovinu čiji spremnik za pohranu nema podataka ili više nije povezan s sredstava.
- Došlo je do pokušaja stvaranje locator na imovinu koji već ima 5 locators koristi. (Azure prostora za pohranu primjenjuje ograničenje od pet zajednički pristup pravila na jedan spremnik za pohranu).
- Povezivanje računa za pohranu sredstava programa IngestManifestAsset nije isti kao račun za pohranu koji se koristi nadređenog IngestManifest.  

## <a name="500-internal-server-error"></a>500 Interna pogreška poslužitelja

Tijekom obrade zahtjeva za Media Services naiđe na neke pogreške koji sprječava obrada nastavak. To može biti zbog nekog od sljedećih razloga:

- Stvaranje resursa ili posao neće uspjeti jer je privremeno nedostupan kvota za servis računa za servise za medijske sadržaje.
- Stvaranje programa resursa ili IngestManifest blob prostora za pohranu spremnik neće uspjeti jer je na račun za pohranu računi informacije trenutno nije dostupan.
- Druga neočekivana pogreška. 

## <a name="503-service-unavailable"></a>503 Servis nije dostupan

Poslužitelj je trenutno nije moguće prima zahtjeve. Ta se pogreška može uzrokovanih viškom zahtjevi za uslugu. Media Services ograničavanje mehanizam ograničava korištenje resursa za aplikacije koje viškom zahtjev za uslugu.

>[AZURE.NOTE]Provjera poruka o pogrešci i pogreške s kodom niza da biste saznali više pojedinosti o razlog imate 503 pogreške. Ta se pogreška uvijek znači ograničavanje.

Opisi statusa mogući su:

- "Poslužitelj nije dostupan. Prethodnih izvođenja ovu vrstu zahtjeva potrebno više od {0} sekundi."
- "Poslužitelj nije dostupan. Više za {0} u sekundi može biti ograničio vrijeme."
- "Poslužitelj nije dostupan. Više od biti ograničio vrijeme zahtjeva za {0} {1} sekundi."

Za rukovanje tu pogrešku, preporučujemo korištenje logike eksponencijalne natrag isključivanje pokušajte ponovno. To znači da pomoću progresivno dulje čekanje između ponovne pokušaje za odgovorima uzastopnih pogrešaka.  Dodatne informacije potražite u članku [Tranzitne kvara zadužen za blokiranje aplikacije](https://msdn.microsoft.com/library/hh680905.aspx). 

>[AZURE.NOTE]Ako koristite [Azure Media Services SDK za .net](https://github.com/Azure/azure-sdk-for-media-services/tree/master)logike pokušaj 503 pogreške implementirana po SDK-a.  
  
## <a name="see-also"></a>Vidi također  

[Media Services upravljanje šifre pogrešaka](http://msdn.microsoft.com/library/windowsazure/dn167016.aspx)

## <a name="next-steps"></a>Daljnji koraci

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
