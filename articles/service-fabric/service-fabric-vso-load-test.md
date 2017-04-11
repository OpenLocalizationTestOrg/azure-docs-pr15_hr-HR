<properties
    pageTitle="Učitavanje testiranje aplikacije pomoću Visual Studio Team Services | Microsoft Azure"
    description="Saznajte kako stress test aplikacija tkanina servisa Azure pomoću Visual Studio Team Services."
    services="service-fabric"
    documentationCenter="na"
    authors="cawams"
    manager="timlt"
    editor="" />

<tags
    ms.service="multiple"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="multiple"
    ms.date="07/29/2016"
    ms.author="cawa" />

# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Učitavanje testiranje aplikacije pomoću Visual Studio Team Services

U ovom se članku objašnjava stress test pomoću Microsoft Visual Studio učitavanja testiranje značajki aplikacije. Koristi se za tkanina servisa Azure s praćenjem stanja servisa pozadinskih i bez praćenja stanja servisa web-sučelja. Primjer aplikaciju s kojom je ovdje je programa simulator aviona mjesto. Unesete programa aviona ID, vrijeme odlaska i odredište. Aplikacije pozadinskih obrađuje zahtjeve i sučelje prikazuje na karti aviona koji odgovaraju kriterijima.

Sljedeći dijagram prikazuje aplikacije servisa tkanina koje ćete testiranja.

![Dijagram primjer aviona mjesto aplikacije][0]

## <a name="prerequisites"></a>Preduvjeti
Prije no što počnete, morate učiniti sljedeće:

- Dobivanje korisničkog računa za Visual Studio Team Services. Možete dobiti trgovini besplatno [Visual Studio Team Services](https://www.visualstudio.com).
- Preuzmite i instalirajte Visual Studio 2013 ili Visual Studio 2015. U ovom se članku koristi Visual Studio 2015 Enterprise edition, ali druge izdanja i Visual Studio 2013 surađivati na sličan način.
- Implementacija aplikacije pripremna okruženje. Informacije o tome potražite u članku [Kako implementirati aplikacije na udaljenom klaster pomoću Visual Studio](service-fabric-publish-app-remote-cluster.md) .
- Razumijevanje vaše aplikacije korištenje uzorka. Ti se podaci koriste u programu Publisher uzorak Učitaj.
- Razumijevanje cilj za učitavanje testiranja. Tako ćete tumačenje i analiza rezultata testa Učitaj.

## <a name="create-and-run-the-web-performance-and-load-test-project"></a>Stvaranje i pokretanje projekta Test za učitavanje i performansi

### <a name="create-a-web-performance-and-load-test-project"></a>Stvaranje projekta Test za učitavanje i performansi

1. Otvorite Visual Studio 2015. Odaberite **datoteku** > **Novo** > **projekt** na traci izbornika da biste otvorili dijaloški okvir **Novi projekt** .

2. Proširite čvor **Visual C#** i odaberite **Test** > **performansi i učitavanje Test projekta**. Imenujte projekta, a zatim odaberite gumb **u redu** .

    ![Snimka zaslona dijaloškog okvira novi projekt][1]

    Trebali biste vidjeti novi projekt Test za učitavanje i performanse weba u pregledniku rješenja.

    ![Snimka zaslona s prikazom novi projekt preglednik rješenja][2]

### <a name="record-a-web-performance-test"></a>Zapis test performanse web

1. Otvaranje projekta .webtest.

2. Odaberite ikonu **Dodaj snimanje** da biste pokrenuli snimanje sesiju u pregledniku.

    ![Snimka zaslona s ikonom za dodavanje snimke u pregledniku][3]

    ![Snimka zaslona s gumbom zapis u pregledniku][4]

3. Otvorite aplikaciju servisa tkanina. Ploča za snimanje treba prikazati zahtjeva za web.

    ![Snimka zaslona s web-zahtjeva na ploči za snimanje][5]

4. Izvođenje niza koji očekujete korisnicima izvođenje akcija. Ove se radnje se koriste kao uzorak da biste generirali opterećenje.

5. Kada završite, odaberite gumb **Zaustavi** da biste zaustavili snimanje.

    ![Snimka zaslona s gumbom tabulatora][6]

    Potrebno je .webtest projekta u Visual Studio zabilježene niz zahtjeva za. Dinamični parametara automatski zamjenjuju. Sada možete izbrisati sve dodatni, koji se ponavljaju ovisnost zahtjeva koji nisu dio scenariju test.

6. Spremanje projekta, a zatim odaberite naredbu **Pokreni testirati** da biste pokrenuli test performanse web lokalno i provjerite je li sve ispravno funkcionira.

    ![Snimka zaslona s naredbom pokrenite Test][7]

### <a name="parameterize-the-web-performance-test"></a>Parameterize test performanse web

Test performanse web možete parameterize pretvaranja test performanse kodirani web i zatim uređivanjem kod. Umjesto toga, možete povezati test performanse web popis podataka tako da se zadanu ponovno izračunava test kroz podatke. Dodatne informacije o pretvorbi test performanse web kodirani testiranje potražite u [Generiraj i izvođenje testiranja performanse kodirani web](https://msdn.microsoft.com/library/ms182552.aspx) . Upute za povezivanje podataka s test performanse web potražite u članku [Dodavanje izvora podataka na webu performanse testiranje](https://msdn.microsoft.com/library/ms243142.aspx) .

U ovom primjeru smo ćete pretvorite performansi test web kodirani test da biste mogli ID aviona zamijenite generirani GUID i dodati više zahtjeva za slanje letovi na različitim mjestima.

### <a name="create-a-load-test-project"></a>Stvaranje projekta test učitavanja

Test projekta učitavanja sastoji se od jednog ili više scenariji opisane pomoću testa performanse web i jedinica testu, uz dodatne navedeni učitavanja testiranje postavki. Sljedeći koraci pokazati kako Stvaranje projekta test učitavanja:

1. Na izborniku prečacu performansi i učitavanje Test projekta odaberite **Dodaj** > **Opterećenja Test**. U čarobnjaku za **Testiranje učitavanja** odaberite gumb **Dalje** da biste konfigurirali postavke za testiranje.

2. U odjeljku **Učitavanje uzorak** odaberite želite li opterećenjem konstante korisnika ili korak opterećenja koji počinje s nekoliko korisnika i povećava se korisnici tijekom vremena.

    Ako imate dobar procjenu količinu učitavanja korisnika i želite li vidjeti kako se izvodi trenutni sustav, odaberite **Konstante učitati**. Ako je vaš cilj da biste saznali izvodi li sustav dosljedno pod različitim opterećenje, odaberite **Korak Učitaj**.

3. U odjeljku **Testiranje Mix** odaberite gumb **Dodaj** , a zatim testu koji želite uvrstiti u testu Učitaj. Stupac za **raspodjelu** možete koristiti da biste odredili postotka zbroja testiranja za svaki test.

4. U odjeljku **Postavke za pokretanje** navedite test trajanje učitavanja.

    >[AZURE.NOTE] **Testiranje iteracija** je mogućnost dostupna je samo kada pokrenete učitavanja test lokalno pomoću Visual Studio.

5. U odjeljku **mjesto** **Pokrenuti postavke**odredite mjesto gdje se generiraju zahtjeva za testiranje Učitaj. Čarobnjak može od vas zatražiti da se prijavite na račun servisa tima. Prijavite se, a zatim odaberite zemljopisnog položaja. Kada završite, odaberite gumb **Završi** .

6. Nakon stvaranja test učitavanja otvorite projekt .loadtest, a zatim je odaberite trenutni pokrenuti postavku, kao što su **Postavke za pokretanje** > **pokrenite Settings1 [aktivni]**. Time se otvara izvođenja postavke u prozoru **Svojstva** .

7. U odjeljku **Rezultati** prozor **Pokreni postavke** svojstva postavku **Tempiranja pojedinosti prostora za pohranu** moraju imati **nijedan** kao zadanu vrijednost. Promijenite ovu vrijednost **Svih pojedinačne detalje** da biste dobili dodatne informacije o opterećenje rezultate testiranja. Potražite u članku [Učitavanje testiranje](https://www.visualstudio.com/load-testing.aspx) dodatne informacije o tome kako povezuju sa servisima tima za Visual Studio, a zatim pokrenite test učitavanja.

### <a name="run-the-load-test-by-using-visual-studio-team-services"></a>Pokrenite test učitavanje pomoću Visual Studio Team Services

Odaberite naredbu **Pokreni učitavanja testiranje** da biste pokrenuli pokrenite test.

![Snimka zaslona s naredbom za pokrenite Test učitavanja][8]

## <a name="view-and-analyze-the-load-test-results"></a>Prikaz i analiza rezultata testa učitavanja

Kao opterećenje Testiraj napreduje, informacije o performansama biti prikazan na grafikonu. Vidjet ćete nešto slično sljedeće grafikonu.

![Snimka zaslona s grafikon performansi za rezultate testiranja učitavanja][9]

1. Odaberite vezu za **Preuzimanje izvješća** pri vrhu stranice. Kada preuzmete izvješće, odaberite gumb za **Prikaz izvješća** .

    Na kartici **grafikon** možete vidjeti grafikona za različite mjerača performansi. Na kartici **Sažetak** prikazuju se ukupne rezultate testiranja. Kartica **tablice** prikazuje ukupni broj testova proslijeđeni i neuspješnih Učitaj.

2. Odaberite broj veza na **Test** > **nije uspjelo** i **pogreške** > **broj** stupaca da biste vidjeli detalje o pogrešci.

    Na kartici **pojedinosti** prikazuje virtualne korisnika i testiranje scenarij podataka nije uspjelo zahtjeva za. Te podatke može biti korisno ako testiranje učitavanja sadrži više scenarija.

Dodatne informacije o prikazu rezultata testa učitavanja potražite u članku [Analiza učitavanje testiranje rezultate u prikazu grafikona za učitavanje testiranje analizatora](https://www.visualstudio.com/load-testing.aspx) .

## <a name="automate-your-load-test"></a>Automatiziranje testiranju učitavanja

Visual Studio tim servisima učitavanje Test nudi API-ji koje omogućuju upravljanje testira opterećenja i analiza rezultata u računa za servise u tim. Dodatne informacije potražite u članku [Oblaka učitavanja testiranje Rest API -ji](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) .

## <a name="next-steps"></a>Daljnji koraci
- [Nadzor i dijagnosticiranje services u postavi za razvoj lokalnog računala](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
