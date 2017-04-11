<properties
    pageTitle="Neprekinuti isporuke s Visual Studio Team Services u Azure | Microsoft Azure"
    description="Saznajte kako konfigurirati projekte tima Visual Studio Team Services da biste automatski stvorite i implementirajte značajke web-aplikacije u aplikacije servisa za Azure ili oblaka usluga."
    services="cloud-services"
    documentationCenter=".net"
    authors="mlearned"
    manager="douge"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="07/06/2016"
    ms.author="mlearned"/>

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Neprekinuti isporuka Azure pomoću Visual Studio Team Services

Možete konfigurirati projekte tima Visual Studio Team Services da biste automatski Sastavljanje i implementirati na Azure web-aplikacije ili servisa u oblaku.  (Informacije o tome da biste postavili neprekinuti Sastavi i implementacija sustava pomoću programa *lokalnog* poslužitelja Foundation tima, pogledajte [Neprekinuti isporuke za servise u Oblaku u Azure](cloud-services-dotnet-continuous-delivery.md)).

Pomoću ovog praktičnog vodiča podrazumijeva da imate Visual Studio 2013 i SDK Azure instaliran. Ako još nemate Visual Studio 2013, preuzmite ga tako da odaberete vezu **besplatno prvi koraci** pri [www.visualstudio.com](http://www.visualstudio.com). Instalirajte Azure SDK [ovdje](http://go.microsoft.com/fwlink/?LinkId=239540).

> [AZURE.NOTE]Potreban vam je račun za Visual Studio Team Services da biste dovršili ovaj Praktični vodič: možete [besplatno otvorili račun za Visual Studio Team Services](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Da biste postavili servise u oblaku omogućuje automatsko stvaranje i implementacija Azure pomoću Visual Studio Team Services, slijedite ove korake.

## <a name="1-create-a-team-project"></a>1: Stvaranje projekta tima

Slijedite upute [u nastavku](http://go.microsoft.com/fwlink/?LinkId=512980) da biste stvorili timskim projektima i veza na Visual Studio. Ovaj vodič pretpostavlja da koristite tima Foundation verzije kontrole (TFVC) kao rješenje izvor kontrole. Ako želite koristiti brojka za kontrolu verzije potražite u članku [brojka verziju ovaj vodič](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: prijava projekta s kontrolom izvorišnog

1. U Visual Studio, otvorite rješenje koje želite uvesti ili stvorite novi.
Možete implementirati web-aplikacijama ili neki servis u oblaku (aplikacija za Azure) tako da slijedite korake u ovom vodiču.
Ako želite da biste stvorili novo rješenje, stvorite novi projekt Azure u Oblaku ili novi projekt ASP.NET MVC. Provjerite je li projekt pronalaze .NET Framework 4 ili 4,5, a ako stvarate projekta servisa oblaka, dodajte ulogu ASP.NET MVC web i ulogu suradnika, a zatim odaberite internetska aplikacija za ulogu web. Kada se to od vas zatraži, odaberite **Internetska aplikacija**.
Ako želite stvoriti web-aplikacijama, odaberite predložak projekta ASP.NET web-aplikacije, a zatim MVC. Potražite u članku [Stvaranje ASP.NET web app u aplikacije servisa za Azure](../app-service-web/web-sites-dotnet-get-started.md).

    > [AZURE.NOTE] Visual Studio Team Services podržavaju samo CI implementacijama web-aplikacija za Visual Studio trenutno. Projekti web-mjesta su iz opseg.

1. Otvaranje kontekstnog izbornika za rješenje pa odaberite **Dodaj rješenje izvor kontrole**.

    ![][5]

1. Prihvatite ili promijenite zadane postavke i odaberite gumb **u redu** . Nakon dovršetka postupka izvor kontrole ikona u **Pregledniku rješenja**.

    ![][6]

1. Otvaranje izbornika prečaca za rješenja, a zatim odaberite **Prijava**.

    ![][7]

1. U području **Promjene na čekanju** **Tima Explorer**unesite komentar za prijavljivanju i odaberite gumb **Prijavi** .

    ![][8]

    Imajte na umu mogućnosti za uključivanje ili isključivanje određene promjene kada se prijavite. Ako nije Izuzeto željene promjene, odaberite **Uključi sve** veze.

    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: povezali projekt Azure

1. Sad kad ste projekta tima Team Services za dodavanje veze VANJSKIH izvora kod koji je u njemu, spremni ste za povezivanje tima projekta s Azure.  [Azure klasični portala](http://go.microsoft.com/fwlink/?LinkID=213885)odaberite oblak servisa ili web-aplikacije ili stvorite novi tako da odaberete u **+** ikonu u donjem lijevom kutu, a zatim odaberete **Servis u Oblaku** ili **Web-aplikacije** , a zatim **Brzo stvaranje**. Odaberite vezu za **Postavljanje za objavljivanje s Visual Studio tim servisima** .

    ![][10]

1. U čarobnjaku unesite naziv računa za Visual Studio Team Services u tekstni okvir, a zatim kliknite vezu **Autorizirali sada** . Možda će se zatražiti da biste se prijavili.

    ![][11]

1. U **Zahtjev za povezivanje** skočni dijaloškom okviru odaberite gumb **Prihvati** da biste autorizirali Azure konfiguriranje tima projekta u Dodavanje veze za VANJSKIH Team Services.

    ![][12]

1. Kada autorizacije uspije, vidjet ćete na padajućem popisu koji sadrži popis projekte tima Visual Studio Team Services. Odaberite naziv tima projekta koji ste stvorili u prethodnim koracima, a zatim odaberite gumb kvačicu u čarobnjaka.

    ![][13]

1. Kada je povezana projekta, vidjet ćete neke upute za provjeru promjene u Visual Studio Team Services tima projekt.  Na vaš sljedeći prijavi, Visual Studio Team Services će stvorite i implementirajte projekta za Azure.  Isprobajte ovo sada po klikom na vezu **Prijava s Visual Studio** i veza za **Pokretanje Visual Studio** (ili gumb ekvivalentan **Visual Studio** pri dnu zaslona portala).

    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: pokretanje programa izvođenje i ponovno implementirate projekta

1. U Visual Studio **Explorer tima**, odaberite vezu **Explorer izvor kontrole** .

    ![][15]

1. Dođite do datoteke rješenja i otvorite ga.

    ![][16]

1. U **Pregledniku rješenja**otvorite datoteku, a zatim ga promijenite. Na primjer, promijenite datoteku `_Layout.cshtml` u odjeljku Prikazi\\zajedničke mape u ulogu web MVC.

    ![][17]

1. Uređivanje logotipa web-mjesta, a zatim odaberite **Ctrl + S** da biste spremili datoteku.

    ![][18]

1. U programu **Explorer tima**, odaberite vezu na **Čekanju** .

    ![][19]

1. Unesite komentar, a zatim odaberite gumb **Prijavi** .

    ![][20]

1. Odaberite gumb **Polazno** da biste se vratili na početnu stranicu **Timskog Explorer** .

    ![][21]

1. Odaberite vezu **sastavlja** da biste pogledali na izgradi u tijeku.

    ![][22]

    **Tim Explorer** prikazuje da koja je aktivirala za vaše prijavi.

    ![][23]

1. Dvokliknite naziv sastavljanje u tijeku da biste pogledali detaljne zapisnik tijekom rada na Sastavi.

    ![][24]

1. Dok je sastavljanje u tijeku, pogledajte definiciju Sastavi koja je stvorena kada su povezane TFS Azure pomoću čarobnjaka.  Otvaranje izbornika prečaca za definiciju Sastavi, a zatim odaberite **Uređivanje definicije Sastavi**.

    ![][25]

    Na kartici **okidača** vidjet ćete da definiciju Sastavi postavljen tako da na svaku prijavi prema zadanim postavkama.

    ![][26]

    Na kartici **postupak** možete vidjeti okruženje za implementaciju postavljena na naziv oblaka servisa ili web-aplikacije. Ako radite s web-aplikacijama, svojstva vidite bit će razlikuju od onih što je prikazano ovdje.

    ![][27]

1. Ako želite da se različite vrijednosti od zadane vrijednosti, navedite vrijednosti za svojstva. Svojstva za Azure objavljivanje su u odjeljku **implementacije** .

    Sljedeća tablica prikazuje dostupne svojstva u odjeljku **implementacije** :

  	|Svojstvo|Zadana vrijednost|
  	|---|---|
  	|Dopusti nepouzdanih certifikata|Ako je false, SSL certifikata moraju biti prijavljeni korijenski za izdavanje certifikata.|
  	|Dopusti nadogradnju|Omogućuje implementaciju da biste ažurirali postojeće implementacije umjesto stvaranja novog. Zadržava IP adresa.|
  	|Brisanje|Ako je true, Prebriši postojeće nepovezanih implementacije (Nadogradnja je dopušteno).|
  	|Put do postavki implementacije|Put do datoteke .pubxml za web-aplikacije u korijenskoj mapi na repo. Zanemaruje za servise u oblaku.|
  	|Okruženje za implementaciju sustava SharePoint|Isto kao naziv usluge.|
  	|Okruženje za Azure implementacije|Web app ili oblačić naziv usluge.|

1. Ako koristite više konfiguracija servisa (.cscfg datoteke), možete odrediti konfiguracija željeni servisa u postavci **Sastavljanje Napredno, a zatim MSBuild argumenata** . Ako, na primjer, da biste koristili ServiceConfiguration.Test.cscfg, postavite MSBuild argumente mogućnost crte `/p:TargetProfile=Test`.

    ![][38]

    Prema vremenu, vaš Sastavi mora biti uspješno dovršena.

    ![][28]

1. Ako dvokliknite naziv Sastavi, Visual Studio prikazuje **Sastavljanje sažetak**, uključujući sve rezultate testiranja iz povezane jedinica test projekata.

    ![][29]

1. [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885)možete pogledati povezane implementacije na kartici **implementacijama** kada je odabrana pripremna okruženju.

    ![][30]

1.  Dođite do URL-a web-mjesta. Za web-aplikacije, samo kliknite gumb **Pregledaj** na naredbenoj traci. Za servis u oblaku, odaberite URL-a u odjeljku **Brzi Glance** stranice **nadzorne ploče** koja prikazuje pripremna okruženja za servise u oblaku. Implementacijama s neprekinutim Integracija za servise u oblaku se objavljuju u okruženju pripremna prema zadanim postavkama. To možete promijeniti tako da postavite svojstvo **Zamjenski okruženje oblaka servisa** na **radni**. Snimka zaslona prikazuje gdje je URL web-mjesta na stranici servisom cloud nadzorne ploče.

    ![][31]

    Na novoj kartici preglednika otvorit će se da bi se pojavila izvodi web-mjesta.

    ![][32]

    Za servise u oblaku, ako ste unijeli druge promjene u projekt, koje okidača više stvara i imate više implementacije. Najnovije jedan je označena kao aktivna.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: ponovno implementirate starijim međuverzije

Ovaj korak odnosi na servise u oblaku i nije obavezno. Azure klasični portalu odaberite starije implementacije, a zatim gumb **implementirati** za vraćanje web-mjesta u starijim prijavi.  Imajte na umu da to će pokrenuti nova Sastavi u TFS i stvaranje novog unosa u povijesti implementacije.

![][34]

## <a name="6-change-the-production-deployment"></a>6: promjena implementacije radnog

Ovaj korak odnosi samo na servise u oblaku, ne web-aplikacije. Kada ste spremni, Digni razinu pripremna okruženje za radnog okruženja odabirom gumba **Zamjena** Azure klasični portalu. Upravo distribuiranih pripremna okruženje je ulogu radnog i prethodni radnog okruženja, ako postoje, postaje okruženju pripremna. Aktivna implementacija mogu se razlikovati za proizvodnju i pripremnih okruženja, ali povijest implementacije nedavno korištenih izdanja je na isti način bez obzira na to okruženje.

![][35]

## <a name="7-run-unit-tests"></a>7: pokretanje jedinici testira

Ovaj korak odnosi se samo na web-aplikacije, ne servise u oblaku. Da biste umetnuli kvalitete prolaz u implementaciji, možete pokrenuti testove jedinica i ako oni neće funkcionirati, možete isključiti uvođenje.

1.  U Visual Studio, dodajte projekta test jedinicu.

    ![][39]

1.  Dodajte reference projekta za projekt koje želite testirati.

    ![][40]

1.  Dodavanje neke testovi jedinicu. Da bismo započeli, pokušajte sustava testu koji uvijek proći kroz.

        ```
        using System;
        using Microsoft.VisualStudio.TestTools.UnitTesting;

        namespace UnitTestProject1
        {
            [TestClass]
            public class UnitTest1
            {
                [TestMethod]
                [ExpectedException(typeof(NotImplementedException))]
                public void TestMethod1()
                {
                    throw new NotImplementedException();
                }
            }
        }
        ```

1.  Uređivanje definicije Sastavi, odaberite karticu **proces** , a Proširite čvor **Test** .

1.  **Nije uspjelo stvaranje pogreške test** postavljen na True. To znači da implementacijskih neće pojaviti osim ako testova proslijediti.

    ![][41]

1.  Red čekanja novi Sastavi.

    ![][42]

    ![][43]

1. Tijekom nastavka je sastavljanje, provjerite u tijeku.

    ![][44]

    ![][45]

1. Kada se dovrši sastavljanje, provjerite rezultate testiranja.

    ![][46]

    ![][47]

1.  Pokušajte stvoriti testu koji se neće uspjeti. Dodavanje nove test kopiranjem prvoga, preimenovati i komentar izvan redak kod koji navodi NotImplementedException je iznimka očekivani.

        ```
        [TestMethod]
        //[ExpectedException(typeof(NotImplementedException))]
        public void TestMethod2()
        {
            throw new NotImplementedException();
        }
        ```

1. Prijava promjena red novi Sastavi.

    ![][48]

1. Prikaz rezultata provjere da biste vidjeli detalje o pogrešci.

    ![][49]

    ![][50]

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o jedinici testiranju u Visual Studio Team Services potražite u članku [pokrenuti testove jedinice u vašem Sastavi](http://go.microsoft.com/fwlink/p/?LinkId=510474). Ako koristite brojka, potražite u članku [zajedničko korištenje kod u brojka](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) i [Neprekinuti implementacije aplikacije servisa za Azure](../app-service-web/app-service-continuous-deployment.md).  Dodatne informacije o servisima tima za Visual Studio potražite u članku [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
