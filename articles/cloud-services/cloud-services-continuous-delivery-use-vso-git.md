<properties
    pageTitle="Neprekinuti isporuke s brojka i Visual Studio Team Services u Azure | Microsoft Azure"
    description="Saznajte kako konfigurirati projekte tima Visual Studio Team Services da biste koristili brojka omogućuje automatsko stvaranje i implementaciju značajke web-aplikacije u aplikacije servisa za Azure ili oblaka usluga."
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

# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Neprekinuti isporuka Azure pomoću Visual Studio Team Services i brojka

Projekte tima za Visual Studio Team Services možete koristiti za sadrže brojka spremište za izvornog koda i automatski Sastavljanje i implementaciju na Azure web-aplikacije ili servisa u oblaku kad god automatske na potvrdi u spremište.

Potreban vam je Visual Studio 2013 i SDK Azure instaliran. Ako još nemate Visual Studio 2013, preuzmite ga tako da odaberete vezu **besplatno prvi koraci** pri [www.visualstudio.com](http://www.visualstudio.com). Instalirajte Azure SDK [ovdje](http://go.microsoft.com/fwlink/?LinkId=239540).


> [AZURE.NOTE]Potreban vam je račun za Visual Studio Team Services da biste dovršili ovaj Praktični vodič: možete [besplatno otvorili račun za Visual Studio Team Services](http://go.microsoft.com/fwlink/p/?LinkId=512979).

Da biste postavili servise u oblaku omogućuje automatsko stvaranje i implementacija Azure pomoću Visual Studio Team Services, slijedite ove korake.

## <a name="1-create-a-git-repository"></a>1: Stvaranje brojka spremište

1. Ako već nemate račun za Visual Studio Team Services, možete ga preuzeti [ovdje](http://go.microsoft.com/fwlink/?LinkId=397665). Kada stvorite tima projekta, odaberite brojka kao izvorišnog sustava kontrolu. Slijedite upute za povezivanje Visual Studio u tim projekt.

2. U programu **Explorer tima**, odaberite vezu **Kloniraj ovo spremište** .

    ![][3]

3. Navedite mjesto na kojem se lokalnu kopiju, a zatim odaberite gumb **Kloniraj** .

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: Stvaranje projekta i primjenu spremište

1. U programu **Explorer tima**, u odjeljku **rješenja** odaberite **Nova** veza da biste stvorili novi projekt u lokalnom spremištu.

    ![][4]

2. Možete implementirati web-aplikacijama ili neki servis u oblaku (aplikacija za Azure) tako da slijedite korake u ovom vodiču. Stvorite novi projekt Azure u Oblaku ili novi projekt ASP.NET MVC. Provjerite jesu li projekt pronalaze je 4 za .NET Framework ili noviji. Ako stvarate projekta servisa oblaka, dodajte ulogu ASP.NET MVC web i ulogu suradnika.
Ako želite stvoriti web-aplikacijama, odaberite predložak projekta **ASP.NET web-aplikacije** , a zatim **MVC**. Dodatne informacije potražite u članku [Stvaranje ASP.NET web app u aplikacije servisa za Azure](../app-service-web/web-sites-dotnet-get-started.md) .

3. Otvaranje izbornika prečaca za rješenje pa odaberite **zapisivanje**.

    ![][7]

4. Ako je ovo prvi put brojka ste koristili u Visual Studio Team Services, morat ćete neke podatke predstavite se brojka. U području **Promjene na čekanju** **Tima Explorer**unesite svoju adresu korisničko ime i e-pošte. Unesite komentar za izvršavanje, a zatim odaberite gumb **Potvrdi** .

    ![][8]

5. Imajte na umu mogućnosti za uključivanje ili isključivanje određene promjene kada se prijavite. Ako nije Izuzeto željene promjene, odaberite **Uključi sve**.

6. Sada ste predavanja promjene u lokalnoj kopiji u spremištu. Nakon toga sinkronizirati s poslužiteljem tako da odaberete veze za **sinkronizaciju** .

## <a name="3-connect-the-project-to-azure"></a>3: povezali projekt Azure

1. Sad kad ste brojka spremište u Visual Studio Team Services s nekim izvornog koda u njoj, spremni ste za povezivanje vašeg spremište brojka Azure.  [Azure klasični portala](http://go.microsoft.com/fwlink/?LinkID=213885)odaberite oblak servisa ili web-aplikacije ili stvorite novi tako da odaberete na ikonu u donjem lijevom kutu, a zatim odaberete **Servis u Oblaku** ili **Web-aplikacije** , a zatim **Brzo stvaranje**+.

    ![][9]

3. Za servise u oblaku, odaberite vezu za **Postavljanje za objavljivanje sa servisima tima za Visual Studio** . Odaberite vezu **Postavljanje implementacije iz izvora kontrole** web-aplikacijama.

    ![][10]

2. U čarobnjaku unesite naziv računa za Visual Studio Team Services u tekstni okvir, a zatim odaberite vezu **Autorizirali sada** . Možda će se zatražiti da biste se prijavili.

    ![][11]

3. U **Zahtjev za povezivanje** skočni dijaloškom okviru odaberite **Prihvati** da biste autorizirali Azure konfiguriranje tima projekta u Visual Studio Team Services.

    ![][12]

4. Nakon uspješnog autorizacije vidite padajući popis koji sadrži projekte tima Visual Studio Team Services.  Odaberite naziv tima projekta koji ste stvorili u prethodnim koracima pa odaberite gumb kvačicu u čarobnjaka.

    ![][13]

    Kada sljedeći put automatske na potvrdi vaš spremište Visual Studio Team Services će stvorite i implementirajte projekta za Azure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: pokretanje programa izvođenje i ponovno implementirate projekta

1. U Visual Studio, otvorite datoteku, a zatim ga promijenite. Na primjer, promijenite datoteku `_Layout.cshtml` u odjeljku Prikazi\\zajedničke mape u ulogu web MVC.

    ![][17]

2. Uredite tekst podnožja web-mjesta, a zatim spremite datoteku.

    ![][18]

3. U **Pregledniku rješenja**, otvorite izbornički prečac za čvor rješenja, čvor projekta ili datoteke koje ste promijenili, a zatim odaberite **zapisivanje**.

4. Upišite komentar, a zatim odaberite **zapisivanje**.

    ![][20]

5. Odaberite vezu za **sinkronizaciju** .

    ![][38]

6. Odaberite vezu **automatske** da biste na potvrdi na spremište u Visual Studio Team Services. (I koristite gumb **Sinkroniziraj** da biste kopirali vaše pamti spremište. Razlika je da **sinkronizaciju** i povlači najnovije promjene u spremištu.)

    ![][39]

7. Odaberite gumb **Polazno** da biste se vratili na početnu stranicu **Timskog Explorer** .

    ![][21]

8. Odaberite **sastavlja** da biste pogledali na izgradi u tijeku.

    ![][22]

    **Tim Explorer** prikazuje da koja je aktivirala za vaše prijavi.

    ![][23]

9. Da biste pogledali detaljne zapisnik tijekom rada na Sastavi, dvokliknite naziv sastavljanje u tijeku.

10. Dok je sastavljanje u tijeku, pogledajte definiciju Sastavi koja je stvorena kada se koristi čarobnjaka za povezivanje s Azure.  Otvaranje izbornika prečaca za definiciju Sastavi, a zatim odaberite **Uređivanje definicije Sastavi**.

    ![][25]

11. Na kartici **okidača** vidjet ćete da definiciju Sastavi postavljen tako da na svaku prijavi, po zadanom. (Za servise u oblaku Visual Studio Team Services stvara i automatski uvodi osnovne granu pripremna okruženje. I dalje imate ručno koraku za implementaciju web-mjesta uživo. Za web-aplikacije koje ne postoji pripremna okruženja, uvodi osnovne granu izravno na web-mjesta uživo.

    ![][26]

1. Na kartici **postupak** možete vidjeti okruženje za implementaciju postavljena na naziv oblaka servisa ili web-aplikacije.

    ![][27]

1. Ako želite da se različite vrijednosti od zadane vrijednosti, navedite vrijednosti za svojstva. Svojstva za Azure objavljivanje su u odjeljku **implementaciju** , a i morate postaviti MSBuild parametre. Na primjer, u prikazom oblaka servisa project, da biste odredili konfiguracija servisa koji nije "Oblaka", postavite parametre MSbuild `/p:TargetProfile=[YourProfile]` gdje se datoteka konfiguracije servisa s nazivom kao što su ServiceConfiguration *[YourProfile]* podudara. *YourProfile*.cscfg.

    Sljedeća tablica prikazuje dostupne svojstva u odjeljku **implementacije** :

  	|Svojstvo|Zadana vrijednost|
  	|---|---|
  	|Dopusti nepouzdanih certifikata|Ako je false, SSL certifikata moraju biti prijavljeni korijenski za izdavanje certifikata.|
  	|Dopusti nadogradnju|Omogućuje implementaciju da biste ažurirali postojeće implementacije umjesto stvaranja novog. Zadržava IP adresa.|
  	|Brisanje|Ako je true, Prebriši postojeće nepovezanih implementacije (Nadogradnja je dopušteno).|
  	|Put do postavki implementacije|Put do datoteke .pubxml za web-aplikacije u korijenskoj mapi na repo. Zanemaruje za servise u oblaku.|
  	|Okruženje za implementaciju sustava SharePoint|Isto kao naziv usluge.|
  	|Okruženje za Azure implementacije|Web app ili oblačić naziv usluge.|

1. Prema vremenu, vaš Sastavi mora biti uspješno dovršena.

    ![][28]

1. Ako dvokliknite naziv Sastavi, Visual Studio prikazuje **Sastavljanje sažetak**, uključujući sve rezultate testiranja iz povezane jedinica test projekata.

    ![][29]

1. [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885)možete pogledati povezane implementacije na kartici **implementacijama** kada je odabrana pripremna okruženju.

    ![][30]

1.  Dođite do URL-a web-mjesta. Za web-aplikacije, samo odaberite gumb **Pregledaj** na portalu. Za servis u oblaku, odaberite URL-a u odjeljku **Brzi Glance** stranice **nadzorne ploče** koja prikazuje pripremna okruženju.

    Implementacijama s neprekinutim Integracija za servise u oblaku se objavljuju u okruženju pripremna prema zadanim postavkama. To možete promijeniti tako da postavite svojstvo **Zamjenski okruženje oblaka servisa** na **radni**. Evo gdje je URL web-mjesta na stranici servisom cloud nadzorne ploče.

    ![][31]

    Na novoj kartici preglednika otvorit će se da bi se pojavila izvodi web-mjesta.

    ![][32]

1.  Ako unijeli druge promjene u projekt, koje okidača više sastavlja i imate više implementacije. Najnovije jedan je označena kao aktivna.

    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: ponovno implementirate starijim međuverzije

Ovaj korak nije obavezan. Azure klasični portalu odaberite starije implementacije, a zatim **ponovno implementirate** da biste vratili web-mjesta u starijim prijavi na. Imajte na umu da to će pokrenuti nova Sastavi u TFS i stvaranje novog unosa u povijesti implementacije.

![][34]

## <a name="6-change-the-production-deployment"></a>6: promjena implementacije radnog

Kada ste spremni, Digni razinu pripremna okruženje za radnog okruženja tako da odaberete **Zamjena** Azure klasični portalu. Upravo distribuiranih pripremna okruženje je ulogu radnog i prethodne radnog okruženja, ako postoje, postaje okruženju pripremna. Aktivna implementacija mogu se razlikovati za proizvodnju i pripremnih okruženja, ali povijest implementacije nedavno korištenih izdanja je na isti način bez obzira na to okruženje.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: implementirati s rad grani.

Kada koristite brojka, koje obično unesite promjene u radu grani i integrirati u glavni granu kada vaš razvoj dosegne dovršeni stanje. Tijekom faze razvoja projekta, ćete htjeti Unaprjeđivanje i uvesti grana rad Azure.

1. U programu **Explorer tima**, odaberite gumb **Polazno** , a zatim gumb **grana** .

    ![][40]

2. Odaberite vezu **Nova grani** .

    ![][41]

3. Unesite naziv granu, kao što su "radimo," i odaberite **Stvaranje grani**. Time se stvara novu lokalnu grani.

    ![][42]

4. Objavljivanje grani. Odaberite naziv podružnice u **objavu grana**, a zatim odaberite **Objavi**.

    ![][44]

6. Prema zadanim postavkama, samo promjene matrice granu pokretanje neprekinuti Sastavi. Da biste postavili neprekinuti Sastavi za rad granu, odaberite stranicu **sastavlja** u **Programu Explorer tima**pa odaberite **Uređivanje definicije Sastavi**.

7. Otvorite karticu **Postavke izvora** . U odjeljku **nadziranih grana neprekinuti integracije i Sastavi**, odaberite **kliknite ovdje da biste dodali novi redak**.

    ![][47]

8. Navedite granu koju ste stvorili, kao što su naslovi/refs/rad.

    ![][48]

9. Promjene u kodu, otvorite izbornik prečaca za promijenjene datoteke, a zatim odaberite **zapisivanje**.

    ![][43]

10. Odaberite vezu **Nisu sinkronizirane Commits** pa odaberite gumb **Sinkroniziraj** ili **automatske** veze da biste kopirali promjene kopiju grani rad u Visual Studio Team Services.

    ![][45]

11. Navigacija do prikaza **sastavlja** i pronađite sastavljanje koji je samo imate aktivirao za podružnice rad.

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više savjeta za korištenje brojka sa servisima tima za Visual Studio, potražite u članku [razvoju i zajedničko korištenje kod u brojka pomoću Visual Studio](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) i informacije o korištenju spremišta brojka koje ne upravlja Visual Studio Team Services da biste objavili Azure potražite u članku [Neprekinuti implementacije aplikacije servisa za Azure](../app-service-web/app-service-continuous-deployment.md). Dodatne informacije o Visual Studio Team Services potražite u članku [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
