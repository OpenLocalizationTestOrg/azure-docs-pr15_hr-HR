<properties 
   pageTitle="Azure aplikacije Čarobnjak za objavljivanje | Microsoft Azure"
   description="Azure aplikacije Čarobnjak za objavljivanje"
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

# <a name="publish-azure-application-wizard"></a>Azure aplikacije Čarobnjak za objavljivanje

## <a name="overview"></a>Pregled

Kada razvijate web-aplikaciju u Visual Studio, možete objaviti tu aplikaciju jednostavnije sa servisom Azure oblak pomoću čarobnjaka za **Objavljivanje Azure aplikacije** . Prvi dio iznose koraci koje morate završiti pomoću čarobnjaka i preostale objašnjavaju značajke čarobnjaka.

>[AZURE.NOTE] U ovoj se temi se o implementaciji servise u oblaku, ne na web-mjesta. Informacije o implementaciji na web-mjesta potražite u članku [Kako implementirati Azure Web-mjesta](https://social.msdn.microsoft.com/Search/windowsazure?query=How%20to%20Deploy%20an%20Azure%20Web%20Site&Refinement=138&ac=4#refinementChanges=117&pageNumber=1&showMore=false).

## <a name="prerequisites"></a>Preduvjeti

Prije objavljivanja web-aplikaciju za Azure, morate koristiti Microsoftov račun i Azure pretplate, a morate pridružiti web-aplikaciju Azure oblaku. Ako ste već obavili te zadatke, možete preskočiti sa sljedećim odjeljkom.

1. Pronađite web-mjesto Microsoftova računa i u okvir za Azure pretplate. Možete pokušati besplatna Azure pretplata besplatne mjesec dana [ovdje](https://azure.microsoft.com/pricing/free-trial/)

1. Stvaranje servis u oblaku i račun za pohranu na Azure. To možete učiniti s poslužitelja Explorer u Visual Studio ili pomoću [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Omogućivanje web-aplikaciju za Azure. Da biste omogućili web-aplikaciju za objavljivanje Azure s Visual Studio, morat ćete povezati s projektom servisa Azure oblaka programa Visual Studio. Stvaranje projekta servisa pridružene oblaka, otvorite izbornički prečac za projekt za web-aplikaciju, a zatim Pretvori, **pretvorite Azure oblaka servisa Project**.

1. Nakon dodavanja projekta servisa oblaka rješenje ponovno otvorite isti izbor prečaca, a zatim odaberite **Objavi**. Dodatne informacije o omogućivanju aplikacije za Azure potražite u članku [Kako: migriranje i objavljivanje web-aplikacije sa servisom Cloud Azure s Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx).

>[AZURE.NOTE] Ne zaboravite da biste pokrenuli Visual Studio s administratorske vjerodajnice (Pokreni kao Administrator).

1. Kada budete spremni objaviti svoju aplikaciju, otvorite izbornički prečac za projekt servisa Azure oblaka, a zatim odaberite **Objavi**. Na sljedeći način prikažite čarobnjak za objavljivanje Azure aplikacije.

## <a name="choosing-your-subscription"></a>Odabir pretplate

### <a name="to-choose-a-subscription"></a>Da biste odabrali pretplatu

1. Prije nego što prvi put koristite čarobnjak, morate se prijaviti. Odaberite vezu **Prijava** . Prijavite se na portal za Azure kada se to od vas zatraži, a Azure korisničko ime i lozinku. 

    ![To je jedna od objavljivanje zaslonima čarobnjaka](./media/vs-azure-tools-publish-azure-application-wizard/IC799159.png)

    Popis pretplata popunjava pretplate za račun. Možda će prikazati i pretplate s pretplatničke datoteke koje ste prethodno uvezli.

1. Na popisu **Odaberite pretplate** odaberite pretplatu da biste koristili ovu implementacije.

   Ako odaberete **<... upravljanje >**, pojavljuje se dijaloški okvir **Upravljanje pretplatama** i možete odabrati pretplatu i korisnički račun koji želite koristiti. Na kartici **računi** prikazuju se svi računi, a **pretplate na** kartici prikazuju se svi pretplate povezane s računima. Možete odabrati i regija iz koje će se koristiti Azure resursa, kao i stvaranje ili uvoz certifikata za vašu pretplatu na portalu Azure. Ako ste uvezli nijednu pretplatu iz datoteke pretplate, povezane potvrde pojavit će se na kartici **potvrde** . Kada završite, odaberite gumb za **Zatvaranje** .

    ![Upravljanje pretplatama](./media/vs-azure-tools-publish-azure-application-wizard/IC799160.png)

    >[AZURE.NOTE] Datoteke pretplate može sadržavati više od jedne pretplate.

1. Odaberite gumb **Dalje** da biste nastavili. 

    Ako još nisu servise u oblaku u vašoj pretplati, potrebnih za stvaranje servis u oblaku u Azure za hostiranje projekta. Pojavit će se dijaloški okvir **Stvaranje servis u Oblaku i račun za pohranu** .

    Unesite novi naziv za servis u oblaku. Naziv mora biti jedinstvena u Azure. Zatim navedite regija ili grupu afinitet podatkovnom centru koji se nalaze u blizini ili Većina klijente. Ovaj naziv koristi se za novi račun za pohranu koji Azure stvara za servis u oblaku.

1. Izmijenite postavke za ovaj implementaciju, a zatim je objavite tako da odaberete gumb **Objavi** (u sljedećem odjeljku navedeni su dodatni Detalji o raznim postavkama). Da biste pregledali postavke prije objavljivanja, odaberite gumb **Dalje** .

    >[AZURE.NOTE] Ako ste odabrali Objavi u ovom ćete koraku, možete nadzirati status u ovom implementacije u Visual Studio.

Uobičajeni i napredne postavke za implementaciju možete izmijeniti pomoću čarobnjaka za **Objavljivanje Azure aplikacije** . Na primjer, možete odabrati postavku za implementaciju aplikacije u okruženju test prije nego što pustite ga. Sljedeća ilustracija prikazuje karticu **Uobičajene postavke** za Azure implementacije.

![Uobičajene postavke](./media/vs-azure-tools-publish-azure-application-wizard/IC749013.png)

## <a name="configuring-your-publish-settings"></a>Konfiguriranje vaše postavke objavljivanja

### <a name="to-configure-the-publish-settings"></a>Da biste konfigurirali postavke objavljivanja

1. Na popisu **u oblaku** poduzmite jedan od sljedećih skupova korake:

   1. U okvir padajućeg popisa odaberite postojeći oblaku. Pojavit će se mjesto centar podataka za servis. Trebali biste zapišete to mjesto i provjerite je li vaš račun mjesto za pohranu u centru za iste podatke.

    1. Odaberite **Stvori novo** da biste stvorili neki servis u oblaku te Azure hosts. U dijaloškom okviru **Stvaranje servis u Oblaku** Navedite naziv za servis, a zatim navedite regija ili afinitet grupe da biste odredili lokaciju podatkovnog centra u koje želite smjestiti ovaj servis u oblaku. Naziv mora biti jedinstvena u Azure.

1. Na popisu **okruženje** odaberite **proizvodnje** ili **pripremna**. Pripremna okruženje odaberite ako želite implementirati aplikaciju okruženje za testiranje. Možete premjestiti aplikacije da biste radnog okruženja kasnije.

1. Na popisu **Sastavi konfiguracije** odaberite **ispravljanje pogrešaka** ili **izdanje**.

1. Na popisu **Konfiguracija servisa** odaberite **oblak** ili **Lokalni**.

    Ako želite omogućiti daljinski povezivanja sa servisom, odaberite potvrdni okvir **Omogući udaljene radne površine za sve uloge** . Ta mogućnost primarno se koristi za otklanjanje poteškoća. Kada potvrdite ovaj okvir, pojavit će se dijaloški okvir **Konfiguracija udaljene radne površine** . Odaberite postavke veze za promjenu konfiguracije.

    Potvrdite okvir **Omogući Web implementacije svih uloga web** da biste omogućili implementaciju web za servis. Morate omogućiti udaljene radne površine da biste koristili ovu značajku. Dodatne informacije potražite u članku [[Objavljivanje neki servis u Oblaku pomoću alata za Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx). Dodatne informacije o implementacija Web potražite u članku [[Objavljivanje neki servis u Oblaku pomoću alata za Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)](https://msdn.microsoft.com/library/azure/ff683672.aspx).

1. Odaberite karticu **Napredne postavke** . U polje **implementacije oznake** prihvatite zadani naziv ili unesite naziv vašem odabiru. Da biste dodali datum oznaku implementaciju, ostavite potvrđenim okvirom.

    ![Treći zaslonu čarobnjaka za objavljivanje](./media/vs-azure-tools-publish-azure-application-wizard/IC749014.png)

1. Na popisu **prostora za pohranu računa** odaberite račun za pohranu da biste koristili ovu implementacije. Usporedite mjesta centre za podatke za servis u oblaku i račun za pohranu. Najbolje ova mjesta mora biti isti.

    >[AZURE.NOTE] Račun za Azure prostora za pohranu pohranjuje paket za implementaciju aplikacije. Kada je implementirati aplikaciju, paket uklanja se s računa za pohranu.

1. Ako želite uvesti samo ažurirane komponente, potvrdite okvir **Ažuriraj implementacije** . Ovu vrstu implementacije može biti brže od puno implementacije. Odaberite vezu za **Postavke** da biste otvorili **implementacije ažuriranje postavki** dijaloškom okviru prikazano na sljedećoj slici. 

    ![Postavke za implementaciju](./media/vs-azure-tools-publish-azure-application-wizard/IC617060.png)

    Možete odabrati jednu od dvije mogućnosti za implementaciju, rastuće ili istodobno. Selektivnog uvođenja ažurira jednu distribuiranih instancu odjednom, tako da se aplikacija ostaje na Internetu i dostupan korisnicima. Istodobno implementacije ažurira sve instance distribuiranih odjednom. Istodobno ažuriranje je brže nego rastuće ažuriranja, ali ako odaberete tu mogućnost, vaša aplikacija možda neće biti dostupne tijekom postupka ažuriranja.

    Potvrdite okvire za u ako nije moguće ažurirati implementacije učinite cijelog implementacije ako želite cijeli implementacije odvija automatski ne uspijete implementacije sustava ažuriranja. Potpuna implementacije vraća adresu virtualne IP (VIP) za servis u oblaku. Dodatne informacije potražite u članku [Kako: zadržati konstantnim virtualna IP adresa za servise u Oblaku](https://msdn.microsoft.com/library/azure/jj614593.aspx).


1. Da biste na servisu za ispravljanje pogrešaka, potvrdite okvir **Omogući IntelliTrace** , ili Ako implementirate konfiguracije za **ispravljanje pogrešaka** i želite za ispravljanje pogrešaka u oblaku u Azure, potvrdite okvir **Omogući udaljene ispravljanje pogrešaka za sve uloge** da biste implementirali servise udaljene za ispravljanje pogrešaka.

2. Da biste profil aplikacije, potvrdite okvir **Omogući Profiliranje** , a zatim vezu **Postavke** da biste prikazali profiliranja mogućnosti. 


    >[AZURE.NOTE] Morate koristiti Visual Studio Ultimate da biste omogućili IntelliTrace ili Profiliranje sloju interakcije (opis), a ne može se omogućiti i u isto vrijeme.

    Dodatne informacije potražite u članku [ispravljanje pogrešaka objavljivanja servis u Oblaku s IntelliTrace i Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx) i [testiranje performanse servis u Oblaku](https://msdn.microsoft.com/library/azure/hh369930.aspx).

1. Odaberite **Dalje** da biste pogledali sažetak stranice za aplikaciju.

## <a name="publishing-your-application"></a>Aplikacija za objavljivanje

1. Možete odabrati da biste stvorili objavljivanje profil od postavki koje ste odabrali. Na primjer, možete stvoriti jedan profil za testiranje okruženja, a drugi za proizvodnju. Da biste spremili ovaj profil, odaberite ikonu **Spremi** . Čarobnjak će stvoriti profil i sprema u projekta za Visual Studio. Da biste promijenili naziv profila, otvorite popis **cilj profila** , a zatim **<... upravljanje >**.

    ![Sažetak zaslonu čarobnjaka za objavljivanje](./media/vs-azure-tools-publish-azure-application-wizard/IC749015.png)

    >[AZURE.NOTE] Objavljivanje profil pojavit će se u pregledniku rješenja u Visual Studio, a postavke profila zapisuju u datoteku s nastavkom .azurePubxml. Postavke se spremaju kao atributa XML oznake.

1. Odaberite **Objavi** da biste objavili svoje aplikacije. Možete nadzirati status postupak u **izlaznom** prozoru u Visual Studio.

## <a name="see-also"></a>Vidi također

[Kako: migriranje i objavljivanje web-aplikacije sa servisom Azure oblaka iz Visual Studio](https://msdn.microsoft.com/library/azure/hh420322.aspx)

[Objavljivanje na servis u Oblaku pomoću alata za Azure](https://msdn.microsoft.com/library/azure/ff683672.aspx)

[Ispravljanje pogrešaka na objavljeni u Oblaku s IntelliTrace i Visual Studio](https://msdn.microsoft.com/library/azure/ff683671.aspx)

[Testiranje performanse servis u Oblaku](https://msdn.microsoft.com/library/azure/hh369930.aspx)

