<properties 
   pageTitle="Dodavanje servisa Mobile pomoću povezani servisi u Visual Studio | Microsoft Azure"
   description="Dodajte mobilne usluge pomoću dijaloškog okvira Visual Studio dodavanje povezani servisi"
   services="visual-studio-online"
   documentationCenter="na"
   authors="mlhoop"
   manager="douge"
   editor="" />
<tags 
   ms.service="visual-studio-online"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="mobile"
   ms.date="12/16/2015"
   ms.author="mlearned" />

# <a name="adding-mobile-services-by-using-visual-studio-connected-services"></a>Dodavanje servisa Mobile pomoću Visual Studio povezani servisi

S Visual Studio 2015, možete se povezati pomoću dijaloškog okvira **Dodavanje servisa povezani** servisa Azure Mobile Services. Možete se povezati s bilo koju aplikaciju klijenta za C#, bilo koju aplikaciju JavaScript ili više platformi Cordova aplikacije. Kada se povežete, možete stvoriti i pristup podacima, stvorite prilagođeni API-ji i Zakazani zadaci ili dodajte podrška za slanje obavijesti.  Povezani servisi operacija dodaje sve odgovarajuće reference i kod veze. Također možete iskoristiti prednost ugrađenu podršku za provjeru autentičnosti s raznim sheme popularne identiteta, kao što su Azure AD Facebook, Twitter i Microsoft Accounts.

## <a name="supported-project-types"></a>Vrste podržanih projekta

>[AZURE.NOTE] U Visual Studio 2015, dodavanje servisa Azure Mobile univerzalni za Windows (Windows 10) projektima pomoću dijaloški okvir Dodavanje povezani servisi nije podržano. Možete dodati Azure mobilne usluge instalacijom odgovarajuću paketa pomoću upravitelja paket NuGet projekta.

Dijaloški okvir povezani servisi možete koristiti za povezivanje s Azure mobilne usluge u sljedećim vrstama projekta.

- .NET Windows 8.1 spremište, telefona i univerzalni aplikacije projekata

- JavaScript Windows 8.1 spremište, telefona i univerzalni aplikacije projekata

- Projekti stvoren pomoću programa Visual Studio Tools za Apache Cordova


## <a name="connect-to-azure-mobile-services-using-the-add-connected-services-dialog"></a>Povezivanje s Azure mobilne usluge pomoću dijaloškog okvira dodavanje povezani servisi

1. Provjerite je li račun za Azure. Ako nemate račun za Azure, možete se prijaviti za [besplatnu probnu verziju](http://go.microsoft.com/fwlink/?LinkId=518146).

1. Otvaranje dijaloškog okvira **Dodavanje povezani servisi** .
 - Za aplikacije .NET otvaranje projekta u Visual Studio, otvaranje kontekstnog izbornika za čvor **reference** u pregledniku rješenja, a zatim odaberite **Dodavanje servisa povezani**
 
        ![Connecting to Azure Mobile Service](./media/vs-azure-tools-connected-services-add-mobile-services/IC797635.png)

 - Za projekte aplikacije Apache Cordova, otvaranje projekta u Visual Studio, otvaranje kontekstnog izbornika za čvor projekta u programu Explorer rješenja i odaberite **Dodavanje servisa povezani**.

1. U dijaloškom okviru **Dodavanje servisa povezani** odaberite **Azure mobilne usluge**, a zatim odaberite gumb **Konfiguracija** . Možda zatražiti da se prijavite u Azure ako to još niste učinili.

    ![Dodavanje Azure servis za mobilne uređaje](./media/vs-azure-tools-connected-services-add-mobile-services/IC797636.png)

1. U dijaloškom okviru **Azure mobilne usluge** odaberite postojeći servis za mobilne uređaje ako postoji. Ako vam je potrebna da biste stvorili novi Azure servis za mobilne uređaje, slijedite postupak u nastavku da biste to učinili. U suprotnom, prijeđite na sljedeći korak.

    Da biste stvorili novi račun za servis za mobilne uređaje:
    1. Odaberite vezu **Servisa za stvaranje **pri dnu dijaloškog okvira.
        ![Dodavanje nove veze servis za mobilne uređaje](./media/vs-azure-tools-connected-services-add-mobile-services/IC797637.png)




    2. U dijaloškom okviru **Stvaranje mobilne usluge** možete odabrati JavaScript mobilne usluge pozadinskog ili .NET mobilne usluge pozadinskog s padajućeg popisa **izvođenja** . 
  
        ![Stvaranje servis za mobilne uređaje](./media/vs-azure-tools-connected-services-add-mobile-services/IC797638.png)

        Usluga pozadinskog JavaScript je jednostavnih i naprednih. Ako stvorite JavaScript mobilne usluge pozadinskog, JavaScript kod poslužiteljsko pohranjen u oblaku, ali skripte poslužitelja možete urediti tako da pomoću programa Explorer poslužitelja ili portala za upravljanje Azure. 

        Mobilna usluga pozadinskog .NET vam punu snagu i fleksibilnost Web API i Framework entitet. Ako stvorite .NET mobilne usluge pozadinskog projekta je za vas stvara i dodati rješenje. 

    1. Odaberite **područje** na mjesto na kojem želite da se servis za mobilne uređaje, a zatim unesite korisničko ime i lozinku za poslužitelj.
 
    1. Nakon što unesete sve potrebne informacije, odaberite gumb **Stvori** da biste stvorili servis za mobilne uređaje.
    2. Novi servis za mobilne uređaje prikazivati na popisu servisa **Azure mobilne usluge** u dijaloškom okviru. Na popisu odaberite novi servis za mobilne uređaje, a zatim odaberite gumb **Dodaj** da biste dodali na servis u projekt.
    

1. Pregled početak rada stranicu koja će se prikazati i Saznajte kako se projekt izmjene. Kada dodate povezani servisa, u pregledniku pojavljuje se stranica za početak rada. Pregled predloženih sljedeće korake i primjeri kod ili prešli na stranicu što se dogodilo da biste vidjeli što reference dodani u projekt i kako su izmijenjeni datotekama kod i konfiguraciji.

1. Primjere koda koristite kao vodič, započnite upisivati kod da biste pristupili servis za mobilne uređaje!

## <a name="how-your-project-is-modified"></a>Kako je izmijenjena projekta

Kako Visual Studio mijenja projekta ovisi o vrsti projekta. C# klijentskim aplikacijama, potražite u članku [što se dogodilo – C# projekata](http://go.microsoft.com/fwlink/p/?LinkId=513119). Aplikacije klijenta JavaScript, potražite u članku [što se dogodilo – JavaScript projekata](http://go.microsoft.com/fwlink/p/?LinkId=513120). Cordova aplikacije, potražite u članku [što se dogodilo – Cordova projektima](http://go.microsoft.com/fwlink/p/?LinkId=513116).


##<a name="next-steps"></a>Daljnji koraci

Postavljanje pitanja i pomoć: 

 - [MSDN Forum: Azure mobilne usluge](https://social.msdn.microsoft.com/forums/azure/home?forum=azuremobile)

 - [Azure mobilne usluge na Blog tima za Microsoft Azure](https://azure.microsoft.com/blog/topics/mobile/)

 - [Azure mobilnih usluga po azure.microsoft.com](https://azure.microsoft.com/services/mobile-services/)

 - [Azure mobilne usluge Poteškoć azure.microsoft.com](https://azure.microsoft.com/documentation/services/mobile-services/)



