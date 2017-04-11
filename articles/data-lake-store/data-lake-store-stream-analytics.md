<properties
   pageTitle="Strujanje podatke iz strujanje analize u spremištu podataka Lake | Azure"
   description="Pomoću analize strujanje Azure toka podataka u spremištu Lake podataka za Azure"
   services="data-lake-store,stream-analytics" 
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a>Strujanja podataka iz Azure spremišta blobova platforme u spremištu Lake podataka pomoću analize strujanje Azure

U ovom se članku će Saznajte kako koristiti Lake spremišta podataka za Azure kao na izlaz za zadatak programa Azure strujanje analize. U ovom se članku objašnjava jednostavni scenarij u kojem se čita podatke iz Azure spremišta blobova (input) i zapisuje podatke za pohranu podataka Lake (rezultat).

>[AZURE.NOTE] Trenutku, stvaranje i konfiguriranje spremišta podataka Lake izlaza strujanje analize podržano je samo u [Azure klasični Portal](https://manage.windowsazure.com). Dakle, neki dijelovi ovog praktičnog vodiča će koristiti Portal klasični Azure.

## <a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

- **Omogućivanje pretplate Azure** javno pretpregled spremište Lake podataka. Pročitajte [upute](data-lake-store-get-started-portal.md#signup).

- **Račun za azure prostora za pohranu**. Blob kontejner s ovog računa će se koristiti za unos podataka za posao strujanje analize. U ovom ćete praktičnom vodiču pretpostavlja da stvorite račun za pohranu pod nazivom **datalakestoreasa** i spremnik subjekta pod nazivom **datalakestoreasacontainer**. Nakon stvaranja spremnik, prenesite datoteku s oglednim podacima. Datoteku s oglednim podacima možete pristupiti iz [Azure podataka Lake brojka spremište](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt). Možete koristiti različite klijenata, kao što su [Azure prostora za pohranu Explorer](http://storageexplorer.com/)za prijenos podataka u spremniku blob.

    >[AZURE.NOTE] Ako stvorite račun s portala za Azure, provjerite je li stvoriti s **Klasični** model implementacije. Na taj način račun za pohranu može se pristupiti na portalu klasični Azure jer je koristimo da biste stvorili strujanje analize posao. Upute za stvaranje računa za pohranu na portalu Azure pomoću klasični implementaciju, potražite u članku [Stvaranje računa za pohranu Azure](../storage/storage-create-storage-account/#create-a-storage-account).
    >
    > Ili možete stvoriti račun za pohranu na portalu klasični Azure.

- **Račun za azure podataka Lake trgovine**. Slijedite upute na [Početak rada s spremišta Lake podataka za Azure pomoću portala za Azure](data-lake-store-get-started-portal.md).  


## <a name="create-a-stream-analytics-job"></a>Stvaranje zadatka strujanje Analytics

Započnite stvaranjem strujanje analize posao koja sadrži ulazni izvor i na odredišni izlaz. Za ovaj vodič izvorišnog web-mjesta u spremniku blobova platforme Azure a odredište spremišta Lake podataka.

1. Prijavite se [Portal za Azure klasični](https://manage.windowsazure.com).

2. U donjem lijevom kutu zaslona, kliknite **Novo**, **Podatkovne usluge**, **Strujanje analize**, **Brzo stvaranje**. Sadrže vrijednosti, kao što je prikazano u nastavku, a zatim **Stvorite zadatak strujanje analize**.

    ![Stvaranje zadatka strujanje Analytics] (./media/data-lake-store-stream-analytics/create.job.png "Stvaranje analize toka posla")

## <a name="create-a-blob-input-for-the-job"></a>Stvaranje unosa blobova platforme za posao

1. Otvorite stranicu za strujanje analize posla, kliknite karticu **unosa** , a zatim **Dodaj ulazni** da biste pokrenuli čarobnjak.

2. Na stranici **Dodavanje ulazni za vaš posao** odaberite **strujanja podataka**, a zatim strelicu prema naprijed.

    ![Dodaj ulazni za vaš posao] (./media/data-lake-store-stream-analytics/create.input.1.png "Dodaj ulazni za vaš posao")

3. Na stranici **Dodavanje strujanja podataka za vaš posao** odaberite **blobova**pa zatim kliknite strelicu prema naprijed.

    ![Dodavanje strujanja podataka za posao] (./media/data-lake-store-stream-analytics/create.input.2.png "Dodavanje strujanja podataka za posao")

4. Na stranici s **postavkama spremište blobova platforme** detalje za spremište blobova platforme koje ćete koristiti kao izvor podataka za unos.

    ![Navedite blob-om za pohranu postavke] (./media/data-lake-store-stream-analytics/create.input.3.png "Navedite blob-om za pohranu postavke")

    * **Enter pseudonima za unos**. Ovo je jedinstveni naziv omogućavaju posao za unos.
    * **Odaberite račun za pohranu**. Provjerite je li je račun za pohranu u području isti kao strujanje analize posao ili će uzrokovati dodatnih troškova premještanje podataka iz područja.
    * **Osiguraj spremnik za pohranu**. Možete odabrati da biste stvorili novi spremnik ili odaberite postojeći kontejner.

    Kliknite strelicu prema naprijed.

5. Na stranici s **postavkama serijalizacije** postavljanje serijaliziranog oblika kao **CSV**, graničnik kao na **kartici**kodiranje kao **UTF8**, a zatim kliknite kvačicu.

    ![Osigurati serijalizacije postavke] (./media/data-lake-store-stream-analytics/create.input.4.png "Osigurati serijalizacije postavke")

6. Kada ste gotovi s čarobnjakom, unos blob dodat će se na kartici **unosa** i stupac **Dijagnostika** treba prikazati **u redu**. Veze na unos možete testirati i izričito pomoću gumba **Testiraj vezu** pri dnu.

## <a name="create-a-data-lake-store-output-for-the-job"></a>Stvaranje spremišta podataka Lake izlaz za posao

1. Otvorite stranicu za strujanje analize posla, kliknite karticu **izlaze** , a zatim **Dodaj u izlaz** da biste pokrenuli čarobnjak.

2. Na stranici **Dodavanje na Izlaz u vaš posao** odaberite **Spremišta Lake podataka**, a zatim strelicu prema naprijed.

    ![Dodaj u izlaz za vaš posao] (./media/data-lake-store-stream-analytics/create.output.1.png "Dodaj u izlaz za vaš posao")

3. Na stranici **ovlasti veze** , ako ste već stvorili račun spremišta Lake podataka, kliknite **Autorizirali odmah**. U suprotnom, kliknite Stvori novi račun **prijavite se odmah** . Za ovaj vodič Javite nam pretpostavlja da već imate račun spremišta podataka Lake stvorili (kao što je rečeno u na preduvjeta). Koje će biti automatski ovlašteni pomoću vjerodajnica s kojima ste se prijavili na portalu klasični Azure.

    ![Autorizirajte spremišta Lake podataka] (./media/data-lake-store-stream-analytics/create.output.2.png "Autorizirajte spremišta Lake podataka")

4. Na stranici **Postavke pohrane Lake podataka** unesite podatke kao što je prikazano u nastavku snimku zaslona.

    ![Navođenje podataka Lake spremište postavke] (./media/data-lake-store-stream-analytics/create.output.3.png "Navođenje podataka Lake spremište postavke")

    * **Enter pseudonima izlaz**. Ovo je jedinstveni naziv omogućavaju izlaz posao.
    * **Navedite spremišta podataka Lake računa**. Trebali biste ste već stvorili, kao što je rečeno u preduvjeta.
    * **Odredite put prefiks uzorak ponavljanja**. Potreban je prepoznavanje Izlazna datoteka koji su napisani spremišta podataka Lake posla strujanje analize. Budući da naslovi izlaze napisao posao u obliku GUID, uključujući prefiks koji će vam pomoći prepoznati pisane izlaz. Ako želite dodati oznaku datuma i vremena kao dio prefiks obavezno uvrstite `{date}/{time}` u uzorku prefiks. Ako to uključuje, omogućeni polja **Oblik datuma **i **Oblik vremena** i možete odabrati oblik po izboru.

    Kliknite strelicu prema naprijed.

5. Na stranici s **postavkama serijalizacije** postavljanje serijaliziranog oblika kao **CSV**, graničnik kao na **kartici**kodiranje kao **UTF8**, a zatim kliknite kvačicu.

    ![Odredite izlazni oblik] (./media/data-lake-store-stream-analytics/create.output.4.png "Odredite izlazni oblik")

6. Kada ste gotovi s čarobnjakom, izlazna spremišta podataka Lake dodat će se na kartici **izlaze** i stupac **Dijagnostika** treba prikazati **u redu**. Veza izlaz možete testirati i izričito pomoću gumba **Testiraj vezu** pri dnu.

## <a name="run-the-stream-analytics-job"></a>Pokreni strujanje Analytics

Da biste pokrenuli strujanje analize posao, morate pokrenuti upita na kartici upit. Za ovaj vodič možete pokrenuti primjer upita zamjena rezerviranih mjesta posao ulazni i izlazni pseudonime, kao što je prikazano u nastavku snimke zaslona.

![Izvođenje upita] (./media/data-lake-store-stream-analytics/run.query.png "Izvođenje upita")

Kliknite **Spremi** od dna zaslona, a zatim kliknite **Start**. U dijaloškom okviru odaberite **Prilagođeno vrijeme**, a zatim odaberite datum u prošlosti, kao što je **1/1/2016**. Kliknite kvačicu da biste pokrenuli posao. Može proći nekoliko minuta da biste pokrenuli posao.

![Postavljanje vremena za posao] (./media/data-lake-store-stream-analytics/run.query.2.png "Postavljanje vremena za posao")

Nakon pokretanja posla, kliknite karticu **monitora** da biste vidjeli obradu podataka.

![Praćenje posla] (./media/data-lake-store-stream-analytics/run.query.3.png "Praćenje posla")

Na kraju, možete koristiti [Portal za Azure](https://portal.azure.com) da biste otvorili račun za spremište Lake podataka i provjerite je li podatke uspješno napisan na račun.

![Potvrda izlaz] (./media/data-lake-store-stream-analytics/run.query.4.png "Potvrda izlaz")

U oknu Eksplorer za podataka obavijest izlaz napisan mapu kao što je navedeno u spremištu podataka Lake izlaz postavke (`streamanalytics/job/output/{date}/{time}`).  

## <a name="see-also"></a>Vidi također

* [Stvaranje programa HDInsight klaster da koristi spremište Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
