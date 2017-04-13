<properties
   pageTitle="Stvaranje projekta programa Azure pomoću Visual Studio | Microsoft Azure"
   description="Stvaranje projekta programa Azure pomoću Visual Studio"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="creating-an-azure-project-with-visual-studio"></a>Stvaranje projekta programa Azure pomoću Visual Studio

Alati za Azure za Visual Studio navedite predložak koji omogućuje stvaranje neki servis u oblaku za Azure. Alati pomoći konfigurirati za ispravljanje pogrešaka i Implementacija servisa u oblaku Azure.

Servis za rješenja programa Azure oblaka sadrži sljedeće vrste projekata:

- **Azure projekta**

    Azure projekt ima pridruživanja projektima uloga u rješenje. Uključuje i servis definicija i datoteke za konfiguraciju servisa. Datoteka za definiciju servisa definira runtime postavke za aplikaciju uključujući potrebni su koje uloge, krajnje točke i veličina virtualnog računala. Konfiguracijska datoteka servisa konfigurira koliko je instanci uloge koje će se izvoditi i vrijednosti postavke definira uloge. Dodatne informacije o tim postavkama potražite u članku [Kako: Konfiguriranje uloge servisa oblaka za Azure s Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

- **Project web uloga**

    Uloga suradnika izvodi obradu pozadine. Uloga suradnika možete komunicirati s servise za pohranu i druge internetske servise. Uloga suradnika može imati bilo koji broj HTTP, HTTPS ili TCP krajnje točke.

    - **Uloga Web ASP.NET**, za izgradnju ASP.NET aplikacija s web-sučelja
    - **Uloga Web MVC5 platforme ASP.NET**
    - **Uloga Web MVC4 platforme ASP.NET**
    - **Uloga Web MVC3 platforme ASP.NET**
    - **WCF servisa Web uloge**, za izgradnju WCF servisa
    - **Silverlight poslovnih aplikacija Web uloga** (potreban je Visual Studio 2012)

- **Uloga suradnika predmemorije**

    Uloga koja omogućuje namjenski predmemorije u aplikaciji.

- **Ulogu suradnika s red čekanja Bus servisu**

    Red bus servisa koja nudi poruke stavljanja funkcije možete komunicirati s radnog procesa. Dodatne informacije potražite [u](http://go.microsoft.com/fwlink/?LinkId=260560)članku korištenje servisa Bus redova.

## <a name="to-create-an-azure-cloud-service-project-in-visual-studio"></a>Da biste stvorili za projekt servisa Azure oblak u Visual Studio

1. Pokrenite Microsoft Visual Studio kao administrator.

1. Na traci izbornika odaberite **datoteku**, **Novi** **projekt**.

1. U oknu **Projekta vrste** odaberite **oblak** projekta za Visual C# ili Visual Basic čvorove predložak.

1. U oknu **Predlošci** odaberite **Azure u Oblaku**.

1. Odredite koju verziju sustava .NET Framework koji želite koristiti za razvoj projekta.

1. Unesite naziv i mjesto projekta i naziv rješenja. Odaberite gumb **u redu** .

1. U dijaloškom okviru **Novi projekt Azure** odaberite uloge koje želite dodati, a zatim gumb sa strelicom desno da biste ih dodali u rješenje. Možete dodati proizvoljan broj uloge god želite.

1. Da biste preimenovali uloge koje ste dodali u projekt, postavite pokazivač miša na ulozi u dijaloškom okviru **Novi projekt Azure** , a zatim odaberite ikonu **preimenovati** s desne strane ulogu. Uloge možete preimenovati i unutar rješenje kada je dodana.
