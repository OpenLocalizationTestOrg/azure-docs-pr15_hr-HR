<properties 
   pageTitle="Početak rada s spremišta Lake podataka pomoću REST API-JA | Microsoft Azure" 
   description="Korištenje WebHDFS REST API-ji za izvođenje operacije na spremište Lake podataka" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/27/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-rest-apis"></a>Početak rada s spremišta Lake podataka za Azure pomoću REST API-ji

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-JA](data-lake-store-get-started-rest-api.md)
- [Azure EŽA](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

U ovom se članku će Saznajte kako pomoću WebHDFS REST API-ji i podataka Lake spremište REST API-ji za upravljanje računima, kao i operacije datotečnom sustavu Azure podataka Lake spremište. Spremište Lake podataka za Azure izlaže vlastitu REST API-ji za postupke za upravljanje računom. Međutim, jer spremišta podataka Lake kompatibilan s HDFS i Hadoop zajednici, podržava pomoću WebHDFS REST API-ji za operacije datotečnom sustavu.

>[AZURE.NOTE] Detaljne informacije o REST API-JA podrške za spremište Lake podataka potražite u članku [Azure podataka Lake spremište REST API Reference](https://msdn.microsoft.com/library/mt693424.aspx).

## <a name="prerequisites"></a>Preduvjeti

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- **Stvaranje aplikacije komponente Azure Active Directory**. Pomoću aplikacije za Azure AD spremišta podataka Lake aplikacijom Azure AD za provjeru autentičnosti. Postoje različite postupke za provjeru s Azure AD koje su **provjere autentičnosti za krajnjeg korisnika** ili **provjere autentičnosti servis za servis**. Upute i dodatne informacije o provjeri autentičnosti potražite u članku [provjere autentičnosti s trgovinom Lake podatke pomoću servisa Azure Active Directory](data-lake-store-authenticate-using-active-directory.md).

- [cURL](http://curl.haxx.se/). U ovom se članku koristi zakretanja da bismo pokazali kako REST API-JA pozive protiv spremišta podataka Lake računa.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Kako autentičnost pomoću servisa Azure Active Directory?

Dva načina prikaza možete koristiti za provjeru autentičnosti pomoću servisa Azure Active Directory.

### <a name="end-user-authentication-interactive"></a>Provjera autentičnosti krajnjeg korisnika (interaktivne)

U ovom scenariju aplikacije traži od korisnika da se prijavite i sve operacije se izvode u kontekstu korisnika. Izvršite sljedeće korake za interaktivno provjeru autentičnosti.

1. Putem aplikacije, preusmjeravanje korisnika na sljedeći URL:

        https://login.microsoftonline.com/<TENANT-ID>/oauth2/authorize?client_id=<CLIENT-ID>&response_type=code&redirect_uri=<REDIRECT-URI>

    >[AZURE.NOTE] \<PREUSMJERAVANJE URI > treba kodirati za korištenje u URL-a. Tako, da biste postigli https://localhost, koristite `https%3A%2F%2Flocalhost`)

    Za potrebe ovog praktičnog vodiča možete zamjena vrijednosti rezervirano mjesto u URL-iznad i zalijepiti ga u adresnoj traci preglednika web. Će biti preusmjereni za provjeru autentičnosti pomoću svoje Azure prijava. Kada uspješno prijave, odgovor se prikazuje u adresnu traku web-preglednika. Odgovor bit će u sljedećem obliku:
        
        http://localhost/?code=<AUTHORIZATION-CODE>&session_state=<GUID>

2. Snimite autorizacije koda iz odgovor. Za ovaj vodič kopirajte kod za provjeru autentičnosti iz adresne trake web-preglednika i prenesite ga u zahtjevu za objavu krajnju točku tokena kao što je prikazano u nastavku:

        curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token \
        -F redirect_uri=<REDIRECT-URI> \
        -F grant_type=authorization_code \
        -F resource=https://management.core.windows.net/ \
        -F client_id=<CLIENT-ID> \
        -F code=<AUTHORIZATION-CODE>

    >[AZURE.NOTE] U ovom slučaju na \<PREUSMJERAVANJE URI > potrebna šifrirana.

3. Je odgovor JSON objekt koji sadrži token za pristup (npr., `"access_token": "<ACCESS_TOKEN>"`) i osvježavanje token (npr., `"refresh_token": "<REFRESH_TOKEN>"`). Aplikacije koristi token za pristup prilikom pristupanja Lake spremišta podataka za Azure i token Osvježi da biste dobili novi token pristup kada istekne token za pristup.

        {"token_type":"Bearer","scope":"user_impersonation","expires_in":"3599","expires_on":"1461865782","not_before": "1461861882","resource":"https://management.core.windows.net/","access_token":"<REDACTED>","refresh_token":"<REDACTED>","id_token":"<REDACTED>"}

4.  Kada istekne token za pristup možete zatražiti novi token za pristup pomoću token osvježavanja kao što je prikazano u nastavku:

         curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
            -F grant_type=refresh_token \
            -F resource=https://management.core.windows.net/ \
            -F client_id=<CLIENT-ID> \
            -F refresh_token=<REFRESH-TOKEN>
 
Dodatne informacije o provjere autentičnosti korisnika interaktivne potražite u članku [autorizacija kod dodijeliti tijek](https://msdn.microsoft.com/library/azure/dn645542.aspx).

### <a name="service-to-service-authentication-non-interactive"></a>Provjera autentičnosti servisa servisa (koji nije interaktivan)

U ovom scenariju u aplikaciju nudi vlastitu vjerodajnice za izvođenje operacije. To, morate izdati zahtjev za objavu poput prikazano u nastavku. 

    curl -X POST https://login.microsoftonline.com/<TENANT-ID>/oauth2/token  \
      -F grant_type=client_credentials \
      -F resource=https://management.core.windows.net/ \
      -F client_id=<CLIENT-ID> \
      -F client_secret=<AUTH-KEY>

Izlaz iz zahtjev neće sadržavati stavku oznaku ovlaštenja (označena tako da `access-token` u nastavku Izlaz) koji će se naknadno proći s pozive REST API-JA. Spremanje token za provjeru autentičnosti u tekstnoj datoteci; trebat će vam u nastavku ovog članka.

    {"token_type":"Bearer","expires_in":"3599","expires_on":"1458245447","not_before":"1458241547","resource":"https://management.core.windows.net/","access_token":"<REDACTED>"}

U ovom se članku koristi na **koji nije interaktivan** pristup. Dodatne informacije o koji nije interaktivan (servis za servis pozivi) potražite u članku [servis za pozive za servis pomoću vjerodajnica](https://msdn.microsoft.com/library/azure/dn645543.aspx).

## <a name="create-a-data-lake-store-account"></a>Stvaranje računa spremišta Lake podataka

Ovaj postupak temelji se na REST API-JA poziv koji su definirani [u nastavku](https://msdn.microsoft.com/library/mt694078.aspx).

Koristite sljedeću naredbu zakretanja. Zamjena ** \<yourstorename >** vašim imenom spremišta Lake podataka.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -H "Content-Type: application/json" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview -d@"C:\temp\input.json"

U naredbu iznad zamijenite \< `REDACTED` \> s token autorizacije koju ste vratili neke starije verzije. Opseg zahtjev za tu naredbu nalazi se u datoteci **input.json** kojemu je na `-d` parametar iznad. Sadržaj datoteke input.json otprilike ovako:

    {
    "location": "eastus2",
    "tags": {
        "department": "finance"
        },
    "properties": {}
    }   

## <a name="create-folders-in-a-data-lake-store-account"></a>Stvaranje mapa u spremištu Lake podataka računa

Ovaj postupak temelji se na WebHDFS REST API-JA poziv koji su definirani [u nastavku](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Make_a_Directory).

Koristite sljedeću naredbu zakretanja. Zamjena ** \<yourstorename >** vašim imenom spremišta Lake podataka.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=MKDIRS

U naredbu iznad zamijenite \< `REDACTED` \> s token autorizacije koju ste vratili neke starije verzije. Ta naredba stvara direktorij pod nazivom **mytempdir** u korijenskoj mapi računa spremišta Lake podataka.

Ako se operacija uspješno ne dovrši trebali biste vidjeti odgovor ovako:

    {"boolean":true}

## <a name="list-folders-in-a-data-lake-store-account"></a>Popis mapa u spremištu Lake podataka računa

Ovaj postupak temelji se na WebHDFS REST API-JA poziv koji su definirani [u nastavku](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#List_a_Directory).

Koristite sljedeću naredbu zakretanja. Zamjena ** \<yourstorename >** vašim imenom spremišta Lake podataka.

    curl -i -X GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS

U naredbu iznad zamijenite \< `REDACTED` \> s token autorizacije koju ste vratili neke starije verzije.

Ako se operacija uspješno ne dovrši trebali biste vidjeti odgovor ovako:

    {
    "FileStatuses": {
        "FileStatus": [{
            "length": 0,
            "pathSuffix": "mytempdir",
            "type": "DIRECTORY",
            "blockSize": 268435456,
            "accessTime": 1458324719512,
            "modificationTime": 1458324719512,
            "replication": 0,
            "permission": "777",
            "owner": "NotSupportYet",
            "group": "NotSupportYet"
        }]
    }
    }

## <a name="upload-data-into-a-data-lake-store-account"></a>Prijenos podataka na račun za spremište Lake podataka

Ovaj postupak temelji se na WebHDFS REST API-JA poziv koji su definirani [u nastavku](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Create_and_Write_to_a_File).

Prijenos podataka pomoću WebHDFS REST API-JA je dva koraka, kao što je opisano u nastavku.

1. Pošaljite zahtjev HTTP STAVITI bez slanja datoteke podatke koje želite prenijeti. U sljedeću naredbu, zamijenite ** \<yourstorename >** vašim imenom spremišta Lake podataka.

        curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/?op=CREATE

    Izlaz za tu naredbu će biti sadržavati privremeno preusmjeravanje URL-a poput onoj prikazanoj u nastavku.

        HTTP/1.1 100 Continue

        HTTP/1.1 307 Temporary Redirect
        ...
        ...
        Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=CREATE&write=true
        ...
        ...

2. Sada mora poslati drugi zahtjev HTTP STAVITI na URL-a za svojstvo **mjesta** u odgovoru na popisu. Zamjena ** \<yourstorename >** vašim imenom spremišta Lake podataka.

        curl -i -X PUT -T myinputfile.txt -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=CREATE&write=true

    Izlaz bit će otprilike ovako:

        HTTP/1.1 100 Continue

        HTTP/1.1 201 Created
        ...
        ...

## <a name="read-data-from-a-data-lake-store-account"></a>Čitanje podataka s računa za spremište Lake podataka

Ovaj postupak temelji se na WebHDFS REST API-JA poziv koji su definirani [u nastavku](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Open_and_Read_a_File).

Čitanje podataka iz spremišta podataka Lake račun je dva koraka.

* Pošaljite zahtjev za početak na krajnjoj `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN`. To će se vratiti na mjesto da biste poslali sljedeći GET zahtjev za.
* Zatim poslati zahtjev za početak na krajnjoj `https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN&read=true`. Prikazat će sadržaj datoteke.

No Budući da nema razlike u ulaznih parametara između prvi i drugi korak, možete koristiti u `-L` pošaljite zahtjev za prvi parametar. `-L`mogućnost zapravo spaja dva zahtjeva u jedan i će postati zakretanja ponavljanje zahtjeva na novo mjesto. Na kraju, Izlaz iz svih zahtjeva poziva prikazat će se tako što je prikazano ispod. Zamjena ** \<yourstorename >** vašim imenom spremišta Lake podataka.

    curl -i -L GET -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=OPEN

Trebali biste vidjeti na Izlaz sličnu ovoj:

    HTTP/1.1 307 Temporary Redirect
    ...
    Location: https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/somerandomfile.txt?op=OPEN&read=true
    ...
    
    HTTP/1.1 200 OK
    ...
    
    Hello, Data Lake Store user!

## <a name="rename-a-file-in-a-data-lake-store-account"></a>Preimenovanje datoteke u račun za spremište Lake podataka

Ovaj postupak temelji se na WebHDFS REST API-JA poziv koji su definirani [u nastavku](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Rename_a_FileDirectory).

Da biste preimenovali datoteku, koristite sljedeću naredbu zakretanja. Zamjena ** \<yourstorename >** vašim imenom spremišta Lake podataka.

    curl -i -X PUT -H "Authorization: Bearer <REDACTED>" -d "" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile.txt?op=RENAME&destination=/mytempdir/myinputfile1.txt

Trebali biste vidjeti na Izlaz sličnu ovoj:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-file-from-a-data-lake-store-account"></a>Brisanje datoteke s računa za spremište Lake podataka

Ovaj postupak temelji se na WebHDFS REST API-JA poziv koji su definirani [u nastavku](http://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/WebHDFS.html#Delete_a_FileDirectory).

Da biste izbrisali datoteku, koristite sljedeću naredbu zakretanja. Zamjena ** \<yourstorename >** vašim imenom spremišta Lake podataka.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://<yourstorename>.azuredatalakestore.net/webhdfs/v1/mytempdir/myinputfile1.txt?op=DELETE

Trebali biste vidjeti na Izlaz kao što je sljedeća:

    HTTP/1.1 200 OK
    ...
    
    {"boolean":true}

## <a name="delete-a-data-lake-store-account"></a>Brisanje računa spremišta Lake podataka

Ovaj postupak temelji se na REST API-JA poziv koji su definirani [u nastavku](https://msdn.microsoft.com/library/mt694075.aspx).

Da biste izbrisali račun za spremište Lake podataka, koristite sljedeću naredbu zakretanja. Zamjena ** \<yourstorename >** vašim imenom spremišta Lake podataka.

    curl -i -X DELETE -H "Authorization: Bearer <REDACTED>" https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.DataLakeStore/accounts/<yourstorename>?api-version=2015-10-01-preview

Trebali biste vidjeti na Izlaz kao što je sljedeća:

    HTTP/1.1 200 OK
    ...
    ...

## <a name="see-also"></a>Vidi također

- [Otvori izvor velikih skupova podataka aplikacije kompatibilne sa Lake spremišta podataka za Azure](data-lake-store-compatible-oss-other-applications.md)
 
