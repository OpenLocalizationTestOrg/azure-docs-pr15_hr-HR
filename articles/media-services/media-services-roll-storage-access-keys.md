<properties 
    pageTitle="Ažuriranje Media Services nakon vodoravnim prostora za pohranu pristupnih tipki | Microsoft Azure" 
    description="Članci dajte upute o tome kako ažurirati Media Services nakon vodoravnim prostora za pohranu pristupnih tipki." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="milangada;cenkdin;juliako"/>

#<a name="update-media-services-after-rolling-storage-access-keys"></a>Ažuriranje Media Services nakon vodoravnim prostora za pohranu pristupnih tipki

Prilikom stvaranja novog računa servisa Azure Media Services i vas se da biste odabrali Azure prostora za pohranu računa koji se koristi za pohranu medijskog sadržaja. Imajte na umu da možete [dodati više prostora za pohranu računa](meda-services-managing-multiple-storage-accounts.md) na račun servisa Media Services.

Prilikom stvaranja novog računa za pohranu Azure stvara dva 512-bitni prostora za pohranu pristupnih tipki, koji se koriste za pristup računu za pohranu za provjeru autentičnosti. Veza za pohranu poboljšati sigurnost, preporučuje se povremeno Obnovi i rotiranje pristup ključ za pohranu. Da bi se omogućuju vam da biste zadržali veze s računom za pohranu korištenje jedne tipkovnog dok Obnovi ostale pristupna tipka se daju dvije tipke programa access (primarnih i sekundarnih). Ovaj postupak se zove "vodoravnim pristupnih tipki".

Media Services ovisi o ključa za pohranu dobili ga. Konkretno, locators koji se koriste za strujanje ili preuzeti svoje imovine ovise o tipka za pristup navedeni prostora za pohranu. Prilikom stvaranja računa AMS traje ovisnosti na tipka za pristup primarni prostora za pohranu po zadanom, no kao korisnik možete ažurirati ključa za pohranu koji ima AMS. Morate provjeriti da biste omogućili Media Services znati tipku koristiti tako da slijedite korake opisane u ovoj temi. Osim toga, kada vodoravnim prostora za pohranu pristupnih tipki, morate da biste bili sigurni da biste ažurirali svoje locators pa neće prekida na servisu strujanje (ovaj korak je i opisana u temi).

>[AZURE.NOTE]Ako imate više računa za pohranu, obavljate postupak s svaki račun za pohranu.
>
>Prije izvršavanja korake opisane u ovoj temi na računu za proizvodnju, provjerite je li da biste testirali ih na račun stara radnog.


## <a name="step-1-regenerate-secondary-storage-access-key"></a>Korak 1: Obnovi sekundarne prostora za pohranu tipkovni prečac

Započnite s ponovno stvaranje ključa za sekundarne pohranu. Prema zadanim postavkama sekundarne ključa ne koristi Media Services.  Informacije o tome kako poništiti ključeva za pohranu potražite u članku [način: prikaz, kopiranje i regenerate prostora za pohranu pristupne ključeve](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
  
##<a id="step2"></a>Korak 2: Ažuriranje Media Services da biste koristili novi ključ sekundarne prostora za pohranu

Ažurirajte Media Services za korištenje tipki pristupa sekundarne prostora za pohranu. Da biste sinkronizirali ključa za regenerated pohranu s Media Services možete koristiti jedan od sljedeća dva načina.

- Pomoću portala za Azure: da biste pronašli naziv i ključ vrijednosti, idite na portal za Azure i odaberite svoj račun. Pojavit će se prozor postavke na desnoj strani. U prozoru postavke odaberite tipke. Ovisno o tome koje želite Media Services da biste Sinkroniziraj s ključa za pohranu, odaberite Sinkroniziraj primarni ključ ili sekundarni ključa gumb Sinkroniziraj. U ovom slučaju pomoću sekundarne tipke.

- Koristite Media Services upravljanje REST API-JA.

Sljedeći primjer koda prikazuje kako se izgraditi na https://endpoint/***subscriptionId/services/mediaservices/računi/accountName*/StorageAccounts/*storageAccountName*/Key zahtjev da bi sinkronizacija ključa za navedeni pohranu s Media Services. U ovom slučaju se koristi vrijednost ključa sekundarne prostora za pohranu. Dodatne informacije potražite u članku [Kako: korištenje Media Services upravljanje REST API-JA](http://msdn.microsoft.com/library/azure/dn167656.aspx).
    
    public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
    {
        var clientCert = GetCertificate(CertThumbprint);
        
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
        Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
        request.Method = "PUT";
        request.ContentType = "application/json; charset=utf-8";
        request.Headers.Add("x-ms-version", "2011-10-01");
        request.Headers.Add("Accept-Encoding: gzip, deflate");
        request.ClientCertificates.Add(clientCert);
        
        
        using (var streamWriter = new StreamWriter(request.GetRequestStream()))
        {
            streamWriter.Write("\"");
            streamWriter.Write(storageAccountKey);
            streamWriter.Write("\"");
            streamWriter.Flush();
        }
        
        using (var response = (HttpWebResponse)request.GetResponse())
        {
            string jsonResponse;
            Stream receiveStream = response.GetResponseStream();
            Encoding encode = Encoding.GetEncoding("utf-8");
            if (receiveStream != null)
            {
                var readStream = new StreamReader(receiveStream, encode);
                jsonResponse = readStream.ReadToEnd();
            }
        }
    }

Nakon ovaj korak ažurirati postojeće locators (koje imaju ovisnosti na stari ključa za pohranu) kao što je prikazano u sljedećem koraku.

>[AZURE.NOTE]Pričekajte 30 minuta prije izvođenja sve operacije s Media Services (na primjer, stvaranje nove locators) da bi se spriječilo utjecaja na čekanju zadatke.

##<a name="step-3-update-locators"></a>Korak 3: Ažuriranje locators

>[AZURE.NOTE]Kada vodoravnim prostora za pohranu pristupnih tipki, morate da biste bili sigurni da biste ažurirali svoje postojeće locators pa prekida u servis za strujanje.

Pričekajte najmanje 30 minuta nakon sinkronizacije novog ključa za pohranu s AMS. Nakon toga možete ponovno stvoriti vaš zahtjev locators da bi preuzeli ovisnost ključa za navedeni pohranu i zadržati postojeći URL.

Imajte na umu da kada ažuriranje (ili ponovno stvorite) SAS locator, URL pretvorit će se uvijek.

>[AZURE.NOTE] Da biste provjerili je li zadržati postojeći URL-ove locators vaš zahtjev, morate izbrisati postojeće locator i stvorite novi koristeći istu ID.

U sljedećem primjeru .NET pokazuje kako stvoriti locator koristeći istu ID.

privatni statične ILocator RecreateLocator(CloudMediaContext context, ILocator locator) {/ / spremanje svojstava postojeće locator.
VAR resursa = locator. Resursa; VAR accessPolicy = locator. AccessPolicy; VAR locatorId = locator. ID-jeve. VAR DatumPočetka] = locator. StartTime; VAR locatorType = locator. Vrsta; VAR locatorName = locator. Naziv;

Izbrišite stari locator.
Lokator. DELETE();

Ako (locator. ExpirationDateTime < = DateTime.UtcNow) {vratiti novi iznimku (String.Format ("nije moguće ponovno stvoriti locator Id = {0} jer je njegova vrijeme isteka locator u prošlosti", locator. ID-ja)); }
    
        // Create new locator using saved properties.
        var newLocator = context.Locators.CreateLocator(
            locatorId,
            locatorType,
            asset,
            accessPolicy,
            startDate,
            locatorName);
    
    
    
        return newLocator;
    }


##<a name="step-5-regenerate--primary-storage-access-key"></a>Korak 5: Obnovi tipkovnog primarni prostora za pohranu

Obnovi tipkovnog primarni prostora za pohranu. Informacije o tome kako poništiti ključeva za pohranu potražite u članku [način: prikaz, kopiranje i regenerate prostora za pohranu pristupne ključeve](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

##<a name="step-6-update-media-services-to-use-the-new-primary-storage-key"></a>Korak 6: Ažuriranje Media Services da biste koristili novi ključ primarni prostora za pohranu
    
Koristite isti postupak kao što je opisano u [koraku 2](media-services-roll-storage-access-keys.md#step2) samo ovaj put sinkronizirati novog ključa pristup primarni prostora za pohranu s računa za servise za medijske sadržaje.

>[AZURE.NOTE]Pričekajte 30 minuta prije izvođenja sve operacije s Media Services (na primjer, stvaranje nove locators) da bi se spriječilo utjecaja na čekanju zadatke.

##<a name="step-7-update-locators"></a>Korak 7: Ažuriranje locators  

Nakon 30 minuta možete ponovno stvoriti vaš zahtjev locators da bi preuzeli ovisnost novog ključa primarni prostora za pohranu i zadržati postojeći URL.

Koristite isti postupak opisan u [3](media-services-roll-storage-access-keys.md#step-3-update-locators).


##<a name="media-services-learning-paths"></a>Tečajevi za Media Services

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Slanje povratnih informacija

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



###<a name="acknowledgments"></a>Potvrda 

Želimo svjesni sljedeće osobe pridonio pri stvaranju ovaj dokument: Cenk Dingiloglu, Milan Gada Seva Titov.
