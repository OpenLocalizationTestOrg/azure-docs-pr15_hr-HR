<properties 
   pageTitle="Početak rada s spremišta podataka Lake | Azure" 
   description="Pomoću portala za stvaranje spremišta podataka Lake račun i izvođenje osnovnih operacija Lake pohrana podataka" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/13/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-the-azure-portal"></a>Početak rada s spremišta Lake podataka za Azure pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [REST API-JA](data-lake-store-get-started-rest-api.md)
- [Azure EŽA](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Saznajte kako koristiti Azure Portal da biste stvorili Lake spremišta podataka za Azure račun i izvođenje osnovnih operacija kao što su prilikom stvaranja mape, prijenos i preuzimanje podatkovne datoteke, izbrišite račun, itd. Dodatne informacije o spremištu Lake podataka potražite u članku [Pregled programa Azure Lake spremišta podataka](data-lake-store-overview.md).

## <a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Mogući Azure pretplate**. Pogledajte [Početak Azure besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/).

## <a name="do-you-learn-fast-with-videos"></a>Učite brzo s videozapisima?

Pogledajte videozapise u nastavku da biste započeli spremišta Lake podataka.

* [Stvaranje računa spremišta Lake podataka](https://mix.office.com/watch/1k1cycy4l4gen)
* [Upravljanje podacima u spremištu Lake podataka pomoću programa Explorer podataka](https://mix.office.com/watch/icletrxrh6pc)

## <a name="create-an-azure-data-lake-store-account"></a>Stvaranje računa spremišta Lake podataka za Azure

1. Prijavite se novom [Portalu Azure](https://portal.azure.com).

2. Kliknite **NOVO**, kliknite **podataka + prostor za pohranu**, a zatim **Lake spremišta podataka za Azure**. Pročitajte informacije u plohu **Lake spremišta podataka za Azure** , a zatim **Stvori** u donjem lijevom kutu na plohu.

3. U plohu **Lake spremišta podataka za novo** Navedite vrijednosti, kao što je prikazano u nastavku snimke zaslona:

    ![Stvaranje novog računa spremišta Lake podataka za Azure] (./media/data-lake-store-get-started-portal/ADL.Create.New.Account.png "Stvaranje novog računa Lake Azure podataka")

    - **Pretplate**. Odaberite pretplatu u kojoj želite stvoriti novi račun spremišta Lake podataka.
    - **Grupa resursa**. Odaberite postojeću grupu resursa ili kliknite **Stvori grupu resursa** da biste je stvorili. Grupa resursa je spremnik koja sadrži povezane resurse za aplikaciju. Dodatne informacije potražite u članku [Grupa resursa u Azure](azure-resource-manager/resource-group-overview.md#resource-groups).
    - **Lokacija**: Odaberite mjesto na kojem želite stvoriti računa spremišta Lake podataka.

4. Ako želite da se računa spremišta Lake podataka da bi mogao pristupiti iz sustava Startboard, odaberite **Prikvači na Startboard** .

5. Kliknite **Stvori**. Ako odaberete da biste prikvačili poslovnog subjekta u startboard, prijeći ćete natrag da biste na startboard i vidjet ćete tijek dodjeljivanja spremišta podataka Lake računa. Kada je račun za pohranu podataka Lake dodjeli, plohu računa se prikazuju.

6. Proširite **Essentials** padajućeg popisa da biste vidjeli podatke o vašem računu spremišta Lake podataka kao što su resurs grupirajte je dio, mjesto, itd. Kliknite ikonu za **Brzi početak rada** da biste vidjeli veze na druge resurse vezane uz spremišta Lake podataka.

    ![Račun za vaše spremište Lake podataka za Azure] (./media/data-lake-store-get-started-portal/ADL.Account.QuickStart.png "Račun za vaše Lake Azure podataka")

## <a name="createfolder"></a>Stvaranje mape u račun za Azure podataka Lake spremišta

U odjeljku računa spremišta Lake podataka za upravljanje i spremanje podataka možete stvoriti mape.

1. Otvorite spremišta podataka Lake račun koji ste upravo stvorili. U lijevom oknu kliknite **Pregledaj**, kliknite **Trgovina Lake podataka**i zatim plohu spremišta Lake podataka kliknite naziv računa u kojoj želite stvoriti mape. Ako je prikvačena na startboard poslovnog subjekta, kliknite tu pločicu računa.

2. U vašem plohu računa spremišta Lake podataka kliknite **Explorer podataka**.

    ![Stvaranje mape u spremištu Lake podataka računa] (./media/data-lake-store-get-started-portal/ADL.Create.Folder.png "Stvaranje mape u spremištu Lake podataka računa")

3. U vašem plohu računa spremišta Lake podataka kliknite **Nova mapa**, unesite naziv za novu mapu i zatim kliknite **u redu**.
    
    ![Stvaranje mape u spremištu Lake podataka računa] (./media/data-lake-store-get-started-portal/ADL.Folder.Name.png "Stvaranje mape u spremištu Lake podataka računa")
    
    Mapu novostvoreni će biti navedeno plohu **Explorer podataka** . Možete stvoriti ugniježđene mape do bilo kojoj razini.

    ![Stvaranje mape u račun Lake podataka] (./media/data-lake-store-get-started-portal/ADL.New.Directory.png "Stvaranje mape u račun Lake podataka")


## <a name="uploaddata"></a>Prijenos podataka na račun za Azure podataka Lake spremište

Možete prenijeti podatke s poslovnim subjektom Lake spremišta podataka za Azure izravno na korijenskoj razini ili u mapu koju ste stvorili u račun. U snimke zaslona ispod, slijedite korake da biste prenijeli datoteke na nekoj podmapi iz plohu **Explorer podataka** . U ovom snimku zaslona, datoteka se prenosi nekoj podmapi prikazano elemenata (označeno crvenim okvirom).

Ako tražite ogledne podatke da biste prenijeli, možete dobiti mapu **Podataka Ambulance** [Azure podataka Lake brojka spremište](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData).

![Prijenos podataka] (./media/data-lake-store-get-started-portal/ADL.New.Upload.File.png "Prijenos podataka")


## <a name="properties"></a>Svojstva i akcije koje su dostupne na pohranjene podatke

Kliknite novododani datoteku da biste otvorili plohu **Svojstva** . U ovom plohu dostupnih svojstva povezana s datoteka i akcije koje možete izvesti na datoteci. Cijeli put datoteke možete kopirati i u vašem računu Lake spremišta podataka za Azure istaknuta u okviru crvena snimku zaslona.

![Svojstva na podatke] (./media/data-lake-store-get-started-portal/ADL.File.Properties.png "Svojstva na podatke")

* Kliknite **Pregled** da biste vidjeli pretpregled datoteka izravno iz web-preglednika. Možete navesti oblikovanje na pregled. Kliknite **Pretpregled**kliknite **oblik** u plohu **Pretpregled datoteka** , a u **Oblik datoteke pretpregled** plohu navedite mogućnosti kao što su broj redaka za prikaz, kodiranje koje će se koristiti graničnik koji će se koristiti itd.

  ![Oblik datoteke pretpregleda] (./media/data-lake-store-get-started-portal/ADL.File.Preview.png "Oblik datoteke pretpregleda")

* Kliknite **Preuzmi** da biste datoteku preuzeli na računalo.

* Kliknite **preimenujte datoteku** da biste preimenovali datoteku.

* Kliknite **Izbriši datoteke** da biste izbrisali datoteku.


## <a name="secure-your-data"></a>Zaštitu podataka

Možete zaštititi podatke pohranjene u vašem računu Lake spremišta podataka za Azure pomoću servisa Azure Active Directory i kontrole pristupa (ACL). Upute o tome kako to učiniti potražite u članku [Zaštita podataka u spremištu Lake za Azure podataka](data-lake-store-secure-data.md).


## <a name="delete-azure-data-lake-store-account"></a>Brisanje Azure podataka Lake spremište računa

Da biste izbrisali račun Lake spremišta podataka za Azure iz vaše plohu spremišta Lake podataka, kliknite **Izbriši**. Da biste potvrdili akciju, će se zatražiti da biste unijeli naziv računa koji želite izbrisati. Unesite naziv računa, a zatim kliknite **Izbriši**.

![Brisanje podataka Lake računa] (./media/data-lake-store-get-started-portal/ADL.Delete.Account.png "Brisanje podataka Lake računa")


## <a name="next-steps"></a>Daljnji koraci

- [Zaštite podataka u spremištu Lake podataka](data-lake-store-secure-data.md)
- [Korištenje analize Lake Azure podataka s spremišta Lake podataka](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Azure HDInsight pomoću spremišta Lake podataka](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Pristup dijagnostičke zapisnike za spremište Lake podataka](data-lake-store-diagnostic-logs.md)
