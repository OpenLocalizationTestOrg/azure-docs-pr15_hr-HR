<properties
    pageTitle="Početak rada s Azure prostora za pohranu u pet minuta | Microsoft Azure"
    description="Brzo steći potrebna znanja na Microsoft Azure blob-ova, tablice i redova pomoću Azure prostora za pohranu brzog pokretanja, Visual Studio i emulator Azure prostora za pohranu. Pokrenite prvi aplikacije Azure prostora za pohranu u pet minuta."
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="get-started-with-azure-storage-in-five-minutes"></a>Početak rada s Azure prostora za pohranu u pet minuta

## <a name="overview"></a>Pregled

Jednostavno je prvi koraci u razvoju s Azure prostora za pohranu. Pomoću ovog praktičnog vodiča prikazuje kako nabaviti aplikaciju za pohranu Azure prema gore i brzo pokretanje. Koristit ćete predloške za brzo pokretanje uključene Azure SDK za .NET. Ove brzog pokretanja sadrže jeste li spremni za izvođenje kod koji pokazuje neki osnovni programiranje scenariji s Azure prostora za pohranu.

Dodatne informacije o Azure prostora za pohranu prije čitanje kod, pogledajte [Sljedeće korake](#next-steps).

## <a name="prerequisites"></a>Preduvjeti

Morat ćete sljedeći preduvjeti prije nego što počnete:

1. Kompiliranje i izraditi aplikaciju, potreban vam je verzija programa [Visual Studio](https://www.visualstudio.com/) instaliran na vašem računalu.

2. Instalirajte najnoviju verziju [Azure SDK za .NET](https://azure.microsoft.com/downloads/). SDK sadrži ogledne projekata Azure brzi početak rada, emulator Azure prostora za pohranu i [Azure prostora za pohranu klijenta biblioteke za .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

3. Provjerite jeste li povezani s [.NET Framework 4,5](http://www.microsoft.com/download/details.aspx?id=30653) instaliran na vašem računalu, kao što je potreban je po projektima uzorka Azure brzi početak rada koje ćemo hoćete li koristiti ovog praktičnog vodiča.

    Ako niste sigurni koju verziju sustava .NET Framework je instaliran na vašem računalu, pročitajte članak [Kako: odredite koji .NET Framework instalirano verzija](https://msdn.microsoft.com/vstudio/hh925568.aspx). Ili pritisnite gumb **Start** ili s logotipom sustava Windows, upišite **Upravljačka ploča**. Nakon toga kliknite **programi** > **Programi i značajke**, i odrediti hoće li 4,5 za .NET Framework je na popisu instaliranih programa.

4. Potreban vam je Azure pretplate i račun za Azure prostora za pohranu.

    - Azure pretplatu, pročitajte članak [Besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/), [Mogućnosti za kupnju](https://azure.microsoft.com/pricing/purchase-options/)i [Nudi člana](https://azure.microsoft.com/pricing/member-offers/) (za članove MSDN, Microsoftovoj partnerskoj mreži i BizSpark i druge Microsoftove programe).
    - Stvaranje računa za pohranu u Azure, potražite u članku [upute za stvaranje računa za pohranu](storage-create-storage-account.md#create-a-storage-account).

## <a name="run-your-first-azure-storage-application-against-azure-storage-in-the-cloud"></a>Pokrenuti prvi aplikacija za pohranu Azure Azure prostora za pohranu u oblaku

Nakon što dodate račun, možete stvoriti jednostavan aplikaciju za pohranu Azure pomoću jednog od uzorka projekata Azure brzog pokretanja u Visual Studio. Pomoću ovog praktičnog vodiča usredotočuje se na projektima uzorka za pohranu Azure: **Azure prostora za pohranu: blob-ova**, **Azure prostora za pohranu: datoteke**, **Azure prostora za pohranu: redova**, i **Azure prostora za pohranu: tablice**:

1. Pokrenuti Visual Studio.
2. Na izborniku **datoteka** kliknite **Novi projekt**.
3. U dijaloškom okviru **Novi projekt** kliknite **instalirani** > **Predlošci** > **Visual C#** > **oblaka** > **Početak rada** > **Podatkovne usluge**.
    na. Odaberite neku od sljedećih predložaka: **Azure prostora za pohranu: blob-ova**, **Azure prostora za pohranu: datoteke**, **Azure prostora za pohranu: redova**, ili **Azure prostora za pohranu: tablice**.
    b. Provjerite je li **.NET Framework 4,5** kao framework cilj.
    - 3.c. Navedite naziv projekta i stvaranje novog rješenja za Visual Studio, kao što je prikazano:

    ![Azure brzog pokretanja][Image1]

Preporučujemo vam da biste pregledali izvornog koda prije pokretanja aplikacije. Da biste pregledali kod, odaberite **Eksplorer za rješenja** na izborniku **Prikaz** u Visual Studio. Zatim, dvaput pritisnite Program.cs datoteku.

Nakon toga izvršavaju primjer aplikacije:

1.  U Visual Studio, na izborniku **Prikaz** odaberite **Preglednik rješenja** . Otvorite App.config datoteka i komentara out niz za povezivanje za emulator Azure prostora za pohranu:

    `<!--<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>-->`

2.  Uklonite niz za povezivanje servisa za pohranu za Azure i navedite prostora za pohranu naziv i pristup ključ računa u datoteci App.config:`<add key="StorageConnectionString" value="DefaultEndpointsProtocol=https;AccountName=[AccountName];AccountKey=[AccountKey]"`

    Da biste dohvatili ključa za pohranu računa programa access, potražite u članku [Upravljanje tipke za pristup prostora za pohranu](storage-create-storage-account.md#manage-your-storage-access-keys).

3.  Kad unesete naziv računa za pohranu i tipkovni prečac u datoteci App.config, na izborniku **datoteka** kliknite **Spremi sve** da biste spremili sve datoteke za projekt.
4.  Na izborniku **Sastavljanje** kliknite **Stvaranje rješenja**.
5.  Na izborniku **za ispravljanje pogrešaka** pritisnite **F11** da biste pokrenuli rješenje korak po korak ili pritisnite **F5** da biste pokrenuli rješenja.


## <a name="run-your-first-azure-storage-application-locally-against-the-azure-storage-emulator"></a>Lokalno pokrenuti prvi aplikacije Azure prostora za pohranu za pohranu Emulator Azure

[Azure prostora za pohranu Emulator](storage-use-emulator.md) nudi lokalnom okruženju u kojem se oponaša servisa blobova platforme Azure, reda čekanja i tablice u svrhu razvoj. Da biste testirali svoju aplikaciju za pohranu lokalno, bez stvaranja za Azure pretplatu ili račun za pohranu i bez povećavanja sve trošak možete koristiti emulator prostora za pohranu.

Da biste isprobali, stvaranje jednostavne aplikacije za pohranu Azure pomoću jednog od uzorka projekata Azure brzog pokretanja u Visual Studio. Pomoću ovog praktičnog vodiča usredotočuje se na projektima uzorka **Spremište blobova platforme Azure**, **Spremištem tablica Azure**i **Pohranu reda čekanja Azure** :

1. Pokrenuti Visual Studio.
2. Na izborniku **datoteka** kliknite **Novi projekt**.
3. U dijaloškom okviru **Novi projekt** kliknite **instalirani** > **Predlošci** > **Visual C#** > **oblaka** > **Početak rada** > **Podatkovne usluge**.
    na. Odaberite neku od sljedećih predložaka: **Azure prostora za pohranu: blob-ova**, **Azure prostora za pohranu: datoteke**, **Azure prostora za pohranu: redova**, ili **Azure prostora za pohranu: tablice**.
    b. Provjerite je li **.NET Framework 4,5** kao framework cilj.
    c. Navedite naziv projekta i stvaranje novog rješenja za Visual Studio, kao što je prikazano:

    ![Azure brzog pokretanja][Image1]

4.  U Visual Studio, na izborniku **Prikaz** odaberite **Preglednik rješenja** . Ako dodate jedan, otvorite App.config datoteka i komentara out niz za povezivanje za vaš račun za Azure prostora za pohranu. Zatim uklonite niz za povezivanje za emulator Azure prostora za pohranu:

    `<add key="StorageConnectionString" value = "UseDevelopmentStorage=true;"/>`

Preporučujemo vam da biste pregledali izvorni kod prije pokretanja aplikacije. Da biste pregledali kod, odaberite **Eksplorer za rješenja** na izborniku **Prikaz** u Visual Studio. Zatim, dvaput pritisnite Program.cs datoteku.

Nakon toga pokrenuti aplikaciju uzorak u prostor za pohranu Emulator Azure:

1.  Pritisnite gumb **Start** ili s logotipom sustava Windows, traženje *emulator spremište na platformi Microsoft Azure*i pokrenite aplikaciju. Kada se pokrene u emulator, vidjet ćete ikonu i obavijesti u područje za prikaz zadatka za Windows.
2.  U Visual Studio, kliknite **Sastavi rješenje** na izborniku **Sastavljanje** .
3.  Na izborniku **za ispravljanje pogrešaka** pritiskom na tipku **F11** da biste pokrenuli rješenje korak po korak ili pritisnite **F5** da biste pokrenuli rješenje od početka do kraja.

## <a name="next-steps"></a>Daljnji koraci

Potražite u sljedećim resursima da biste saznali više o Azure pohranu:

* [Uvod u Microsoft Azure prostora za pohranu](storage-introduction.md)
* [Početak rada pomoću programa Explorer Azure prostora za pohranu](../vs-azure-tools-storage-manage-with-storage-explorer.md)
* [Početak rada s Azure blobova pomoću .NET](storage-dotnet-how-to-use-blobs.md)
* [Početak rada sa spremištem tablica Azure pomoću .NET](storage-dotnet-how-to-use-tables.md)
* [Početak rada s spremištem reda čekanja Azure pomoću .NET](storage-dotnet-how-to-use-queues.md)
* [Početak rada s Azure pohrani u sustavu Windows](storage-dotnet-how-to-use-files.md)
* [Prijenos podataka s AzCopy naredbenog retka uslužni](storage-use-azcopy.md)
* [Dokumentacija Azure prostora za pohranu](https://azure.microsoft.com/documentation/services/storage/)
* [Microsoft Azure prostora za pohranu klijenta biblioteke za .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx)
* [Servise za pohranu Azure REST API-JA](https://msdn.microsoft.com/library/azure/dd179355.aspx)

[Image1]: ./media/storage-getting-started-guide/QuickStart.png
