<properties 
   pageTitle="Dodavanje prostora za pohranu Azure pomoću povezani servisi u Visual Studio | Microsoft Azure"
   description="Dodavanje prostora za pohranu Azure aplikacije pomoću dijaloškog okvira Visual Studio dodavanje povezani servisi"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="adding-azure-storage-by-using-visual-studio-connected-services"></a>Dodavanje prostora za pohranu Azure pomoću Visual Studio povezani servisi

## <a name="overview"></a>Pregled

S Visual Studio 2015, možete se povezati sve C# u oblaku, .NET pozadinskog mobilne usluge, ASP.NET web-mjesto ili servis, servisa platforme ASP.NET 5 ili servisa Azure WebJob za pohranu Azure pomoću dijaloški okvir **Dodavanje povezani servisi** . Funkcija povezanog servis zbraja sve potrebne reference i kod vezu, a komponenta mijenja konfiguracijske datoteke. Dijaloški okvir i vodi vas na dokumentaciju koja piše što sljedeće korake da biste pokrenuli blobova, redovi i tablice.

## <a name="supported-project-types"></a>Vrste podržanih projekta

Dijaloški okvir povezani servisi omogućuju povezivanje sa spremištem Azure u sljedećim vrstama projekta.

- Projekti Web platforme ASP.NET

- ASP.NET 5 projekata

- Web-mjesto .NET oblaka servisa Web uloge i u okvir za projekte ulogu suradnika

- .NET mobilne usluge projekata

- Azure WebJob projekata


## <a name="connect-to-azure-storage-using-the-connected-services-dialog"></a>Povezivanje sa spremištem Azure pomoću dijaloškog okvira povezani servisi

1. Provjerite je li račun za Azure. Ako nemate račun za Azure, možete se prijaviti za [besplatnu probnu verziju](http://go.microsoft.com/fwlink/?LinkId=518146). Nakon što dodate račun za Azure, možete stvoriti račune za pohranu, mobilne usluge za stvaranje i konfiguriranje Azure Active Directory.

1. Otvaranje projekta u Visual Studio, otvaranje kontekstnog izbornika za čvor **reference** u pregledniku rješenja, a zatim **Dodavanje servisa povezani**.

    ![Dodavanje veze servisa](./media/vs-azure-tools-connected-services-storage/IC796702.png)

1. U dijaloškom okviru **Dodavanje servisa povezani** odaberite **Azure prostora za pohranu**, a zatim odaberite gumb **Konfiguracija** . Možda zatražiti da se prijavite u Azure ako to još niste učinili.

    ![Dodavanje servisa povezani dijaloški okvir – prostora za pohranu](./media/vs-azure-tools-connected-services-storage/IC796703.png)

1. U dijaloškom okviru **Spremište Azure** odaberite postojeći račun za pohranu i odaberite **Dodaj**.

    Ako morate stvoriti novi račun za pohranu, prijeđite na sljedeći korak. U suprotnom, preskočite do koraka 6.

    ![Dijaloški okvir za Azure prostora za pohranu](./media/vs-azure-tools-connected-services-storage/IC796704.png)

1. Da biste stvorili novi prostor za pohranu račun: 

    1. Odaberite gumb **Stvori novi račun za pohranu** pri dnu dijaloškog okvira Azure prostora za pohranu.

    1. Ispunite dijaloški okvir **Stvaranje računa za pohranu** , a zatim odaberite gumb **Stvori** .
    
        ![Dijaloški okvir za Azure prostora za pohranu](./media/vs-azure-tools-connected-services-storage/create-storage-account.png)

        Kada ste u dijaloškom okviru **Spremište Azure** , pojavit će se novi prostor za pohranu na popisu.

    1. Na popisu odaberite novi prostor za pohranu, a zatim odaberite **Dodaj**.

1. U odjeljku servis reference čvor projekta WebJob pojavit će se servis za pohranu povezani.

    ![Azure prostora za pohranu u programu project web poslova](./media/vs-azure-tools-connected-services-storage/IC796705.png)

1. Pregledajte stranici početak rada koji će se prikazati i Saznajte kako se projekt izmjene. Kada dodate povezani servisa, u pregledniku pojavljuje se stranica za početak rada. Pregled predloženih sljedeće korake i primjeri kod ili prešli na stranicu što se dogodilo da biste vidjeli što reference dodani u projekt i kako su izmijenjeni datotekama kod i konfiguraciji.

## <a name="how-your-project-is-modified"></a>Kako je izmijenjena projekta

Kada završite dijaloški okvir, Visual Studio dodaje reference i mijenja određene datoteke konfiguracije. Određene promjene ovise o vrsti projekta. 

 - Projekti ASP.NET potražite u članku [što se dogodilo – ASP.NET projekata](http://go.microsoft.com/fwlink/p/?LinkId=513126). 
 - ASP.NET 5 projektima, potražite u članku [što se dogodilo – ASP.NET 5 projekata](http://go.microsoft.com/fwlink/p/?LinkId=513124). 
 - Oblak servisa projektima (web uloge i uloge suradnika), potražite u članku [što se dogodilo – projekata u Oblaku](http://go.microsoft.com/fwlink/p/?LinkId=516965). 
 - WebJob projektima, potražite u članku [što se dogodilo - WebJob projektima](./storage/vs-storage-webjobs-what-happened.md).

## <a name="next-steps"></a>Daljnji koraci

1. Primjere koda početak rada koristite kao vodič, stvoriti vrstu prostora za pohranu koji želite, a zatim pokrenite pisanja koda za pristup računu za pohranu!

1. Postavljanje pitanja i pomoć
     - [MSDN Forum: Azure prostora za pohranu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata)

     - [Blog tima za Azure prostora za pohranu](http://blogs.msdn.com/b/windowsazurestorage/)

     - [Prostor za pohranu uz azure.microsoft.com](https://azure.microsoft.com/services/storage/)

     - [Dokumentacija za pohranu na azure.microsoft.com](https://azure.microsoft.com/documentation/services/storage/)

