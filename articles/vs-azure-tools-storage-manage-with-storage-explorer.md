<properties
    pageTitle="Početak rada s programom Explorer za pohranu (pretpregled) | Microsoft Azure"
    description="Upravljanje resursima Azure prostora za pohranu pomoću programa Explorer prostora za pohranu (pretpregled)"
    services="storage"
    documentationCenter="na"
    authors="TomArcher"
    manager="douge"
    editor="" />

 <tags
    ms.service="storage"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/17/2016"
    ms.author="tarcher" />

# <a name="getting-started-with-storage-explorer-preview"></a>Početak rada s programom Explorer za pohranu (pretpregled)

## <a name="overview"></a>Pregled 

Microsoft Azure prostora za pohranu (pretpregled) je samostalnu aplikaciju koja omogućuje jednostavno rad s podacima Azure prostora za pohranu u sustavu Windows, OS X i Linux. U ovom se članku ćete saznati razni načini povezivanja s i upravljanje računima Azure prostora za pohranu.

![Microsoft Azure prostora za pohranu Explorer (pretpregled)][15]

## <a name="prerequisites"></a>Preduvjeti

- [Preuzmite i instalirajte Explorer prostora za pohranu (pretpregled)](http://www.storageexplorer.com)

## <a name="connect-to-a-storage-account-or-service"></a>Povezivanje s računa za pohranu ili usluge

Prostor za pohranu Explorer (pretpregled) omogućuje myriad načina za povezivanje s računima za pohranu. To obuhvaća povezivanje s računima za pohranu pridružene Azure pretplate, povezivanje s računima za pohranu i servise koji se zajednički koristi s druge pretplate za Azure i čak i povezivanje i upravljanje lokalno spremište pomoću prostora za pohranu Emulator Azure:

- [Povezivanje s Azure pretplate](#connect-to-an-azure-subscription) - Upravljanje resursima za pohranu pripadaju Azure pretplatu.
- [Rad s lokalnom razvoj prostora za pohranu](#work-with-local-development-storage) - upravljanje lokalno spremište pomoću prostora za pohranu Emulator Azure. 
- [Prilaganje vanjskih za pohranu](#attach-or-detach-an-external-storage-account) - Upravljanje resursima za pohranu pripadaju drugu pretplatu Azure pomoću računa za pohranu naziv računa i ključa.
- [Dodavanje prostora za pohranu računa pomoću SAS](#attach-storage-account-using-sas) - Upravljanje resursima za pohranu pripadaju drugu pretplatu Azure pomoću SAS.
- [Prilaganje servis pomoću SAS](#attach-service-using-sas) - upravljanje servisima za pohranu određene (spremnik blob, reda čekanja ili tablicu) pripadaju drugu pretplatu Azure pomoću SAS.

## <a name="connect-to-an-azure-subscription"></a>Povezivanje s Azure pretplate

> [AZURE.NOTE] Ako nemate račun za Azure, možete ga [Registrirajte se za besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) ili [aktivirali svoje prednosti pretplatnika Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

1. U Eksploreru za pohranu (pretpregled), odaberite **Postavke računa Azure**. 

    ![Postavke računa za Azure][0]

1. U lijevom oknu sada će se prikazivati svi računi Microsoft ste prijavljeni u. Da biste se povezali s drugim računom, odaberite **Dodaj račun**i slijedite dijaloške okvire da biste se prijavili pomoću Microsoftova računa koji je pridružen barem jedan aktivnu pretplatu Azure.

    ![Dodavanje računa][1]

1. Kada se uspješno prijavite pomoću Microsoftova računa, u lijevom oknu će popuniti Azure pretplate povezanim s tim računom. Odaberite Azure pretplate s kojom želite raditi, a zatim odaberite **Primijeni**. (Odaberete preklapa **sve pretplate** odaberete sve ili ništa od navedenih Azure pretplata.)

    ![Odaberite Azure pretplate][3]

1. U lijevom oknu prikazat će sada račune za pohranu pridružene odabrane Azure pretplate.

    ![Odabrane pretplate Azure][4]

## <a name="work-with-local-development-storage"></a>Rad s lokalnom razvoj prostora za pohranu

Prostor za pohranu Explorer (pretpregled) omogućuje rad na temelju lokalno spremište pomoću prostora za pohranu Emulator Azure. Time kod protiv i testiranje za pohranu nužno bez prostora za pohranu računa implementiran na Azure (jer je račun za pohranu je u tijeku emuliranog po Emulator prostora za pohranu za Azure).

>[AZURE.NOTE] Prostor za pohranu Emulator Azure trenutno je podržano samo za Windows. 

1. U lijevom oknu programa Explorer prostora za pohranu (pretpregled) proširite na **(lokalne i pridruženo** > **Račune za pohranu** > čvor**(razvoj)** .

    ![Lokalni razvoj čvor][21]

1. Ako još niste instalirali prostora za pohranu Emulator Azure, morat ćete učiniti i putem InfoTraka. Ako se prikaže na informativnoj traci, odaberite **preuzeti najnoviju verziju**, a instalirati na emulator. 

    ![Preuzimanje Azure prostora za pohranu Emulator upita][22]

1. Kad instalirate na emulator, imat ćete mogućnost stvaranje i rad s lokalnom blob-ova, redovima i tablice. Da biste saznali kako raditi s svaku vrstu računa za pohranu, odaberite odgovarajuću vezu ispod:

    - [Upravljanje resursima spremište blobova platforme Azure](./vs-azure-tools-storage-explorer-blobs.md)
    - Upravljanje Azure datoteke zajedničko korištenje prostora za pohranu resursima – *uskoro dostupno*
    - Upravljanje resursima za pohranu Azure red – *uskoro dostupno*
    - Upravljanje resursima spremište tablica platforme Azure – *uskoro dostupno*

## <a name="attach-or-detach-an-external-storage-account"></a>Pridruživanje i odvajanje računa vanjskog prostora za pohranu

Prostor za pohranu Explorer (pretpregled) omogućuje da biste priložili Vanjsko pohranjivanje računa tako da se za pohranu računa možete jednostavno zajednički koristiti. U ovom se odjeljku objašnjava kako priložiti (i odvajanje iz) računima vanjskih prostora za pohranu.

### <a name="get-the-storage-account-credentials"></a>Dobiti račun vjerodajnice za pohranu

Da biste zajednički koristili račun vanjskog prostora za pohranu, vlasniku tog računa morate najprije dobivanja vjerodajnica - naziv računa i ključ - za račun i omogućivanje zajedničkog korištenja tih informacija s osobom zanima da biste priložili taj račun (vanjski). Nabavljanje vjerodajnice za pohranu računa možete napraviti putem portala za Azure slijedeći ove korake: 

1.  Prijavite se na [portal za Azure](https://portal.azure.com).
1.  Odaberite **Pregledaj**.
1.  Odaberite **Računi za pohranu**.
1.  U plohu **Prostora za pohranu računa** odaberite račun željeni prostora za pohranu.
1.  U **postavkama** plohu za račun odabrane prostora za pohranu, odaberite **pristupnih tipki**.

    ![Mogućnost tipke programa Access][5]
    
1.  U plohu **pristupnih tipki** kopirajte vrijednosti **Naziv RAČUNA za POHRANU** i **1, KLJUČ** za korištenje prilikom pridruživanja na račun za pohranu. 

    ![Tipke za pristup][6]

### <a name="attach-to-an-external-storage-account"></a>Prilaganje računa vanjskog prostora za pohranu
Da biste priložili računa vanjskog prostora za pohranu, morat ćete naziv poslovnog subjekta i ključa. U odjeljku *dobivanja vjerodajnica računa za pohranu* objašnjeno kako nabaviti te vrijednosti s portala za Azure. Međutim, imajte na umu da na portalu ključ za račun zove "ključa 1" pa gdje Explorer prostora za pohranu (pretpregled) traži ključ za račun, vi ćete unesite (ili zalijepite) vrijednost "ključa 1". 
 
1.  U Eksploreru za pohranu (pretpregled), odaberite **Povezivanje sa spremištem Azure**.

    ![Povezivanje s mogućnost pohrane u Azure][23]

1.  U dijaloškom okviru **Povezivanje sa spremištem Azure** navesti ključ za račun ("ključa 1" vrijednost na portalu za Azure), a zatim odaberite **Dalje**.

    ![Povezivanje s dijaloški Azure prostora za pohranu][24] 

1.  U dijaloškom okviru **Prilaganje vanjskog prostora za pohranu** unesite naziv računa za pohranu u okvir **naziv računa** , navedite sve željene postavke i odaberite **Dalje** po završetku. 

    ![Prilaganje dijaloški vanjskog prostora za pohranu][8]

1.  U dijaloškom okviru **Veza sažetak** provjerite podatke. Ako želite promijeniti ništa, kliknite **natrag** i ponovno unesite željene postavke. Kada završite, odaberite **Poveži**.

1.  Nakon uspostave računa vanjskog prostora za pohranu prikazat će se s tekstom **(vanjski)** dodaju se naziv računa za pohranu. 

    ![Rezultat povezivanja s poslovnim subjektom vanjskog prostora za pohranu][9]

### <a name="detach-from-an-external-storage-account"></a>Odvajanje s računa za vanjske prostora za pohranu

1.  Desnom tipkom miša kliknite račun vanjskog prostora za pohranu koji želite odvojiti i – na kontekstnom izborniku – odaberite **Odvoji**.

    ![Odvajanje putem mogućnosti prostora za pohranu][10]

1.  Kada se prikaže okvir poruke o potvrdi, odaberite **da** da biste potvrdili detachment s računa za vanjske prostora za pohranu.

## <a name="attach-storage-account-using-sas"></a>Prilaganje prostora za pohranu računa pomoću SAS

[SAS (zajednički pristup potpisa)](storage/storage-dotnet-shared-access-signature-part-1.md) omogućuje administrator pretplate na Azure mogućnost da biste dodijelili pristup računu za pohranu na temelju privremene bez potrebe za davanje svoja uvjerenja Azure pretplate. 

Da biste to prikazuju, recimo UserA je administrator Azure pretplate, a UserA želi da biste omogućili UserB za pristup računu za pohranu ograničeno vrijeme s određenim dozvolama za:

1. UserA generira SAS (koji se sastoji od niz za povezivanje za račun za pohranu) za određeno vremensko razdoblje i željene dozvole.
1. UserA na SAS omogućio zanima pristup za pohranu računu - UserB, u našem primjeru osobe.  
1. UserB koristi za pohranu Explorer (pretpregled) da biste priložili račun pripadaju UserA pomoću navedenom SAS. 

### <a name="get-a-sas-for-the-account-you-want-to-share"></a>Dobili SAS za račun koji želite zajednički koristiti

1.  U Eksploreru za pohranu (pretpregled), desnom tipkom miša kliknite račun za pohranu koji želite zajednički koristiti, a – na kontekstnom izborniku - potvrdite **Dohvati zajednički pristup potpis.**

    ![Početak SAS kontekstnog izbornika Mogućnosti][13]

1. U dijaloškom okviru **Zajedničko korištenje programa Access potpis** odredite vremensko razdoblje i dozvole za račun, a zatim odaberite **Stvori**.

    ![Početak dijaloški SAS][14]
 
1. Pojavit će se drugi dijaloški okvir **Zajedničko korištenje potpisa Access** prikazuje na SAS. Odaberite **Kopiraj** pokraj **Niz veze** da biste ga kopirali u međuspremnik. Odaberite **Zatvori** da biste zatvorili dijaloški okvir.

### <a name="attach-to-the-shared-account-using-the-sas"></a>Prilaganje zajedničke račun pomoću na SAS

1.  U Eksploreru za pohranu (pretpregled), odaberite **Povezivanje sa spremištem Azure**.

    ![Povezivanje s mogućnost pohrane u Azure][23]

1.  U dijaloškom okviru **Povezivanje sa spremištem Azure** odredite niz za povezivanje, a zatim odaberite **Dalje**.

    ![Povezivanje s dijaloški Azure prostora za pohranu][24] 

1.  U dijaloškom okviru **Veza sažetak** provjerite podatke. Ako želite promijeniti ništa, kliknite **natrag** i ponovno unesite željene postavke. Kada završite, odaberite **Poveži**.

1.  Kada priloženim tekstom (SAS) dodaju se naziv računa koji ste naveli prikazat će se na račun za pohranu.

    ![Rezultat od priložiti račun SAS][17]

## <a name="attach-service-using-sas"></a>Prilaganje servisa pomoću SAS

U odjeljku [Dodavanje prostora za pohranu račun SAS](#attach-storage-account-using-sas) prikazuje kako administrator Azure pretplate može dati privremeni pristupa računu za pohranu za generiranje (i zajedničko korištenje) SAS za račun za pohranu. Isto tako, SAS možete generirati određenog servisa (spremnik blob, reda čekanja ili tablicu) za pohranu subjekta.  

### <a name="generate-a-sas-for-the-service-you-want-to-share"></a>Generiranje SAS za servis koji želite zajednički koristiti

U ovom kontekstu servisa može biti blob spremnik, reda čekanja ili tablicu. U sljedećim odjeljcima objašnjavaju kako da biste generirali SAS navedenih servisa:

- [Na SAS pribaviti spremniku blobova platforme](./vs-azure-tools-storage-explorer-blobs.md#get-the-sas-for-a-blob-container)
- Nabavljanje sustava SAS za zajedničke datoteke – *uskoro dostupno*
- Početak u SAS red – *uskoro dostupno*
- Nabavljanje sustava SAS za tablicu – *uskoro dostupno*

### <a name="attach-to-the-shared-account-service-using-the-sas"></a>Prilaganje servisa zajedničko račun pomoću na SAS

1.  U Eksploreru za pohranu (pretpregled), odaberite **Povezivanje sa spremištem Azure**.

    ![Povezivanje s mogućnost pohrane u Azure][23]

1.  U dijaloškom okviru **Povezivanje sa spremištem Azure** odredite SAS URI-JA, a zatim odaberite **Dalje**.

    ![Povezivanje s dijaloški Azure prostora za pohranu][24] 

1.  U dijaloškom okviru **Veza sažetak** provjerite podatke. Ako želite promijeniti ništa, kliknite **natrag** i ponovno unesite željene postavke. Kada završite, odaberite **Poveži**.

1.  Kada priloženim upravo priložene servisa prikazat će se u odjeljku čvor **(Servis SAS)** .

    ![Rezultat priložite zajednički servis pomoću SAS][20]

## <a name="search-for-storage-accounts"></a>Pretraživanje za račune za pohranu

Ako imate dugačak popis račune za pohranu, brz način da biste pronašli određenu prostora za pohranu računa je pomoću okvira za pretraživanje pri vrhu lijevog okna. 

Dok upisujete u okvir za pretraživanje, u lijevom oknu prikazat će se samo za pohranu račune koji odgovara vrijednosti pretraživanja koje ste unijeli na koje upućuju. Na sljedećoj je snimci zaslona prikazuje primjer u kojem se nakon pretraživanja za sve račune za pohranu koji se gdje naziv računa spremišta s tekstom "tarcher".

![Prostor za pohranu računa pretraživanja][11]
    
Da biste očistili pretraživanje, odaberite gumb sa **znakom x** u okvir za pretraživanje.

## <a name="next-steps"></a>Daljnji koraci
- [Upravljanje resursima spremište blobova platforme Azure pomoću programa Explorer prostora za pohranu (pretpregled)](./vs-azure-tools-storage-explorer-blobs.md)

[0]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/settings-icon.png
[1]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/add-account-link.png
[3]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/subscriptions-list.png
[4]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-accounts-list.png
[5]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys.png
[6]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/access-keys-copy.png
[8]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-external-storage-dlg.png
[9]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/external-storage-account.png
[10]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage.png
[11]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/storage-account-search.png
[12]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/detach-external-storage-confirmation.png
[13]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-context-menu.png
[14]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/get-sas-dlg1.png
[15]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/mase.png
[17]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-account-using-sas-finished.png
[20]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/attach-service-using-sas-finished.png
[21]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/local-storage-drop-down.png
[22]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/download-storage-emulator.png
[23]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-icon.png
[24]: ./media/vs-azure-tools-storage-manage-with-storage-explorer/connect-to-azure-storage-next.png
