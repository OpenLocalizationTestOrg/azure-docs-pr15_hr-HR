<properties
    pageTitle="Upravljanje resursima spremište blobova platforme Azure pomoću programa Explorer prostora za pohranu (pretpregled) | Microsoft Azure"
    description="Upravljanje spremnika blobova platforme Azure i blob-ova pomoću programa Explorer prostora za pohranu (pretpregled)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="manage-azure-blob-storage-resources-with-storage-explorer-preview"></a>Upravljanje resursima spremište blobova platforme Azure pomoću programa Explorer prostora za pohranu (pretpregled)

## <a name="overview"></a>Pregled

[Spremište blobova platforme Azure](./storage/storage-dotnet-how-to-use-blobs.md) je servis za pohranu velike količine nestrukturirane podatke, kao što je tekst ili binarni podataka koji se može pristupiti iz bilo kojeg mjesta na svijetu putem HTTP ili HTTPS.
Spremište blobova platforme možete koristiti da biste ponudili podatke javno svijeta ili radi pohrane podataka aplikacije privatno. U ovom se članku ćete saznati kako pomoću programa Explorer prostora za pohranu (pretpregled) da biste radili s blob spremnika i blob-ova.

## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili korake u ovom članku, potrebno je sljedeće:

- [Preuzmite i instalirajte Explorer prostora za pohranu (pretpregled)](http://www.storageexplorer.com)
- [Povezivanje s Azure prostora za pohranu račun ili servis](./vs-azure-tools-storage-manage-with-storage-explorer.md#connect-to-a-storage-account-or-service)

## <a name="create-a-blob-container"></a>Stvaranje spremnika blobova platforme

Sve blob polja moraju se nalaziti u spremniku blob koji je jednostavno logičke grupiranja blob-ova. Račun može sadržavati neograničen broj spremnika i svakog spremnika mogu pohraniti neograničen broj blob-ova.

Sljedeći koraci objašnjavaju kako stvoriti spremniku blob u programu Explorer prostora za pohranu (pretpregled).

1.  Otvorite Eksplorer za pohranu (pretpregled).
1.  U lijevom oknu proširite račun za pohranu u kojem želite li stvoriti kontejner s blob.
1.  Desnom tipkom miša kliknite **Blob spremnika**i – na kontekstnom izborniku – odaberite **Stvaranje spremnika Blob**.

    ![Stvaranje blob spremnika kontekstnog izbornika][0]

1.  Tekstni okvir će se pojaviti ispod **Blob spremnika** mape. Unesite naziv za svoje blob kontejner. U odjeljku [pravila za imenovanje spremnik](./storage/storage-dotnet-how-to-use-blobs.md#create-a-container) za popis pravila i ograničenja imenovanja blob spremnika.

    ![Stvaranje spremnika Blob tekstnog okvira][1]

1.  Pritisnite **Enter** kad ste završili stvaranje kontejnera za blob ili **Esc** da biste odustali. Nakon što spremniku blob uspješno, prikazat će se u mapi **Blob spremnika** za račun odabrane prostora za pohranu.

    ![Spremnik blob stvorili][2]

## <a name="view-a-blob-containers-contents"></a>Prikaz sadržaja spremnika blobova platforme

Blob spremnika sadrže blob-ova i mapa (koje mogu sadržavati blob-ova).

Sljedeći koraci objašnjavaju kako prikazati sadržaj spremniku blob u programu Explorer prostora za pohranu (pretpregled):

1.  Otvorite Eksplorer za pohranu (pretpregled).
1.  U lijevom oknu proširite prostora za pohranu račun koji sadrži kontejner s blob koju želite pogledati.
1.  Proširite računa spremišta **Blobova spremnika**.
1.  Desnom tipkom miša kliknite kontejner s blob koju želite pregledati, a – na kontekstnom izborniku – odaberite **Otvori uređivač spremnik Blob**.
Možete i dvokliknuti spremnik blob koju želite pogledati.

    ![Kontekstni izbornik za otvaranje blob spremnik uređivača][19]

1.  U glavnom oknu prikazat će se sadržaja spremnika blob.

    ![Uređivač spremnik blobova platforme][3]

## <a name="delete-a-blob-container"></a>Izbrišite spremnik blobova platforme

Blob spremnika mogu jednostavno stvorili i izbrisati prema potrebi. (Da biste vidjeli kako brisati pojedinačne blob-ova, pogledajte odjeljak, [Upravljanje blob-ova u spremniku blob](./#managing-blobs-in-a-blob-container).)

Sljedeći koraci objašnjavaju kako izbrisati blob spremnika u programu Explorer prostora za pohranu (pretpregled):

1.  Otvorite Eksplorer za pohranu (pretpregled).
1.  U lijevom oknu proširite prostora za pohranu račun koji sadrži kontejner s blob koju želite pogledati.
1.  Proširite računa spremišta **Blobova spremnika**.
1.  Desnom tipkom miša kliknite kontejner s blob koju želite izbrisati, a – na kontekstnom izborniku – odaberite **Izbriši**.
Možete pritisnuti i **Brisanje** da biste izbrisali kontejner s trenutno odabrane blob.

    ![Brisanje blob spremnik kontekstnog izbornika][4]

1.  Odaberite **da** da biste dijaloški okvir za potvrdu.

    ![Brisanje blob spremnik potvrdu][5]

## <a name="copy-a-blob-container"></a>Kopiranje spremniku blobova platforme

Explorer prostora za pohranu (pretpregled) omogućuje vam da biste kopirali spremniku blob u međuspremnik i zatim ga zalijepite blob spremnika u neki drugi račun za pohranu. (Da biste vidjeli kako kopirati pojedinačne blob-ova, pogledajte odjeljak, [Upravljanje blob-ova u spremniku blob](./#managing-blobs-in-a-blob-container).)

Sljedeći koraci objašnjavaju kako kopirati spremniku blob s jednog prostora za pohranu računa na drugi.

1.  Otvorite Eksplorer za pohranu (pretpregled).
1.  U lijevom oknu proširite prostora za pohranu račun koji sadrži kontejner s blob koje želite kopirati.
1.  Proširite računa spremišta **Blobova spremnika**.
1.  Desnom tipkom miša kliknite spremnik blobova platforme koje želite kopirati, a – na kontekstnom izborniku – odaberite **Kopiraj Blob kontejner**.

    ![Kopiranje blob spremnik kontekstnog izbornika][6]

1.  Desnom tipkom miša kliknite račun za pohranu željeno "cilja" u koju želite zalijepiti kontejner s blob i – na kontekstnom izborniku – odaberite **Zalijepi kontejner Blob**.

    ![Lijepljenje blob spremnik kontekstnog izbornika][7]

## <a name="get-the-sas-for-a-blob-container"></a>Na SAS pribaviti spremniku blobova platforme

[Zajednički pristup potpis (SAS)](./storage/storage-dotnet-shared-access-signature-part-1.md) omogućuje prijenos ovlasti pristup resursa u račun za pohranu.
To znači da možete dodijeliti klijent ograničene dozvole za objekte na vašem računu za pohranu za određeno vrijeme i određeni skup dozvola, bez potrebe za zajedničko korištenje ključeva za pristup računu.

Sljedeći koraci objašnjavaju kako stvoriti SAS za spremniku blob:

1.  Otvorite Eksplorer za pohranu (pretpregled).
1.  U lijevom oknu proširite račun za pohranu koji sadrži blob spremnik za koju želite dobiti SAS.
1.  Proširite računa spremišta **Blobova spremnika**.
1.  Desnom tipkom miša kliknite željeni blob spremnika i – na kontekstnom izborniku - potvrdite **Dohvati potpis pristup zajednički koristiti**.

    ![Početak SAS kontekstnog izbornika][8]

1.  U dijaloškom okviru **Zajedničko korištenje programa Access potpis** navedite pravilo, datume početka i isteka, vremenska zona i pristup razine za resurs.

    ![Mogućnosti SAS][9]

1.  Kada završite s određivanje mogućnosti SAS, odaberite **Stvori**.

1.  Drugi dijaloški okvir **Zajedničko korištenje programa Access potpis** prikazat će se zatim koja sadrži popis spremnik blob zajedno s URL-ove i QueryStrings možete koristiti za pristup resursu za pohranu.
Odaberite **Kopiraj** uz URL koji želite kopirati u međuspremnik.

    ![Kopiranje URL-ovi SAS][10]

1.  Kada to učinite, odaberite **Zatvori**.

## <a name="manage-access-policies-for-a-blob-container"></a>Upravljanje pravilnicima za pristup za spremniku blobova platforme

Sljedeći koraci objašnjavaju kako upravljati (Dodavanje i uklanjanje) pristup pravila za spremniku blob:

1.  Otvorite Eksplorer za pohranu (pretpregled).
1.  U lijevom oknu proširite račun za pohranu koji sadrži spremnik blob čije pravilnike za pristup želite upravljati.
1.  Proširite računa spremišta **Blobova spremnika**.
1.  Odaberite željeni blob kontejner i – na kontekstnom izborniku – odaberite **Upravljanje pravilima programa Access**.

    ![Upravljanje Kontekstni izbornik za pravila programa access][11]

1.  Dijaloški okvir **Pravilnike za pristup** prikazat će se popis sva pravila programa access već stvorene za odabrani blob kontejner.

    ![Mogućnosti programa Access pravila][12]        

1.  Slijedite ove korake ovisno o pristup pravila upravljanja zadatka:

    - **Dodavanje novog pravila za pristup** – odaberite **Dodaj**. Kada je generiran, dijaloški okvir **Pristup pravila** prikazat će se pravila novododani pristup (uz zadane postavke).
    - **Uređivanje pravila programa access** – unesite sve željene izmjene pa odaberite **Spremi**.
    - **Uklanjanje pravila programa access** – odaberite **Ukloni** uz pravilnik o pristupu želite ukloniti.

## <a name="set-the-public-access-level-for-a-blob-container"></a>Postavljanje razine javni pristup za spremniku blobova platforme

Prema zadanim postavkama, svaki spremnik blob je postavljeno na "Bez pristupa javno".

Sljedeći koraci objašnjavaju kako odrediti razinu pristupa javno spremniku blob.

1.  Otvorite Eksplorer za pohranu (pretpregled).
1.  U lijevom oknu proširite račun za pohranu koji sadrži spremnik blob čije pravilnike za pristup želite upravljati.
1.  Proširite računa spremišta **Blobova spremnika**.
1.  Odaberite željeni blob kontejner, a – na kontekstnom izborniku - odaberite **Postavi javni pristup razinu**.

    ![Postavljanje razine Kontekstni izbornik za javni pristup][13]

1.  U dijaloškom okviru **Postavi razinu pristupa javno spremnik** odredite razinu pristupa željeno.

    ![Postavljanje mogućnosti za razine javni pristup][14]

1.  Odaberite **Primijeni**.

## <a name="managing-blobs-in-a-blob-container"></a>Upravljanje blob-ova u spremniku blobova platforme

Nakon što stvorite spremniku blob, prijenos blob na njih blob, preuzmite blob na lokalnom računalu, otvorite blob na lokalnom računalu i još mnogo toga.

Sljedeći koraci objašnjavaju kako upravljati blob polja (i mape) u spremniku blob.

1.  Otvorite Eksplorer za pohranu (pretpregled).
1.  U lijevom oknu proširite prostora za pohranu račun koji sadrži kontejner s blob želite upravljati.
1.  Proširite računa spremišta **Blobova spremnika**.
1.  Dvokliknite spremnik blob koju želite pogledati.
1.  U glavnom oknu prikazat će se sadržaja spremnika blob.

    ![Prikaz blob spremnik][3]

1.  U glavnom oknu prikazat će se sadržaja spremnika blob.

1.  Slijedite ove korake ovisno o zadatak koji želite izvesti:

    - **Prijenos datoteka na spremniku blobova platforme**

        1.  Na alatnoj traci glavnom oknu odaberite **Prenesi**, a zatim **Prijenos datoteka** na padajućem izborniku.

            ![Izbornik datoteka Prenesi][15]

        1.  U dijaloškom okviru **prijenos datoteka** odaberite gumb trotočku (****...) na desnoj strani tekstnog okvira **datoteke** odaberite datoteke koje želite prenijeti.

            ![Prijenos datoteka mogućnosti][16]

        1.  Određivanje vrste **Blob vrsta**. U članku [Početak rada s Azure blobova pomoću .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) objašnjava razlike između različitih vrsta blob.

        1.  Možete i navesti ciljnu mapu u koju ćete prenijeti odabrane datoteke. Ako ne postoji ciljne mape, će biti stvorena.

        1.  Odaberite **Prijenos**.

    - **Prijenos mape u spremniku blobova platforme**

        1.  Na alatnoj traci glavnom oknu na padajućem izborniku odaberite **Prenesi**, a zatim **Prijenos mapa** .

            ![Prijenos izbornik mape][17]

        1.  U dijaloškom okviru **Prijenos mapa** odaberite gumb trotočku (****...) na desnoj strani tekstnog okvira **mape** da biste odabrali mapu čiji sadržaj koji želite prenijeti.

            ![Prijenos mogućnosti mape][18]

        1.  Određivanje vrste **Blob vrsta**. U članku [Početak rada s Azure blobova pomoću .NET](./storage/storage-dotnet-how-to-use-blobs.md#blob-service-concepts) objašnjava razlike između različitih vrsta blob.

        1.  Možete i navesti ciljnu mapu u koju ćete prenijeti sadržaj odabrane mape. Ako ne postoji ciljne mape, će biti stvorena.

        1.  Odaberite **Prijenos**.

    - **Preuzmite blob na lokalnom računalu**

        1.  Odaberite blob koju želite preuzeti.

        1.  Na glavnom oknu alatnoj traci odaberite **Preuzmi**.

        1.  U dijaloškom okviru **Odredite gdje želite spremiti preuzete blob-om** navedite mjesto na koje želite da se blob preuzeti, a želite joj dodijeliti naziv.  

        1.  Odaberite **Spremi**.

    - **Otvorite blob na lokalnom računalu**

        1.  Odaberite blob koju želite otvoriti.

        1.  Na alatnoj traci glavnom oknu odaberite **Otvori**.

        1.  Blob-om će se preuzeti i otvoriti pomoću aplikacije pridružene s blob podlozi vrstu datoteke.

    - **Kopiranje blob u međuspremnik**

        1.  Odaberite blob koje želite kopirati.

        1.  Na alatnoj traci glavnom oknu odaberite **Kopiraj**.

        1.  U lijevom oknu pronađite u drugi spremnik blob i dvokliknite je da biste vidjeli u oknu glavni.

        1.  Na alatnoj traci glavnom oknu odaberite **Zalijepi** da biste stvorili kopiju blob-om.

    - **Brisanje blob**

        1.  Odaberite blob koju želite izbrisati.

        1.  Na alatnoj traci glavnom oknu odaberite **Izbriši**.

        1.  Odaberite **da** da biste dijaloški okvir za potvrdu.

## <a name="next-steps"></a>Daljnji koraci

- Pogledajte [najnovije Explorer prostora za pohranu (pretpregled) pustite bilješke i videozapise](http://www.storageexplorer.com).
- Saznajte kako [stvoriti aplikacije koje koriste Azure blob-ova, tablice, redove, i datoteke](https://azure.microsoft.com/documentation/services/storage/).

[0]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-create-context-menu.png
[1]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create.png
[2]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-create-done.png
[3]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-editor.png
[4]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-context-menu.png
[5]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-delete-confirmation.png
[6]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-copy-context-menu.png
[7]: ./media/vs-azure-tools-storage-explorer-blobs/blob-containers-paste-context-menu.png
[8]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-context-menu.png
[9]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-options.png
[10]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-get-sas-urls.png
[11]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-context-menu.png
[12]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-manage-access-policies-options.png
[13]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-context-menu.png
[14]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-set-public-access-level-options.png
[15]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-menu.png
[16]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-files-options.png
[17]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-menu.png
[18]: ./media/vs-azure-tools-storage-explorer-blobs/blob-upload-folder-options.png
[19]: ./media/vs-azure-tools-storage-explorer-blobs/blob-container-open-editor-context-menu.png