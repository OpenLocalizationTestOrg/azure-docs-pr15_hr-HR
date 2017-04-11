<properties
   pageTitle="Testiranje Azure web app performanse | Microsoft Azure"
   description="Pokrenite Azure web app performanse testira da biste provjerili je način postupanja s aplikacijom učitavanja korisnika. Izmjerite reakcija, a zatim pronađite pogreške koji mogu upućivati problema."
   services="app-service\web"
   documentationCenter=""
   authors="ecfan"
   manager="douge"
   editor="jimbe"/>

<tags
   ms.service="app-service-web"
   ms.workload="web"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="05/25/2016"
   ms.author="estfan; manasma; ahomer"/>

# <a name="performance-test-your-azure-web-app-under-load"></a>Performanse testiranje Azure web aplikacije opterećenju

Prije no što ga pokrenuti ili implementacija ažuriranja radnog, provjerite performanse web app. Na taj način možete bolje otkriti bez obzira jeste li spremni za izdanje je aplikacija. Dojam sigurni aplikacije možete rukovati promet tijekom korištenja Vršna ili na sljedeći marketinške automatsko prosljeđivanje.

Tijekom javno pretpregleda performanse test možete aplikacije besplatno na portalu za Azure.
Ove testovi tijekom određenog vremenskog razdoblja kao zamjenu za korisnika opterećenje aplikacije i mjerenje odgovor pokrenite aplikaciju. Ako, na primjer, rezultate testiranja Prikaži brzinu aplikacije odgovaranja na određeni broj korisnika. Također prikazuju koliko zahtjeva nije uspjelo, koji mogu upućivati problema s aplikacijom.      

![Pronalaženje probleme s performansama u web-aplikaciji](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>Prije početka

* Morat ćete na [Azure pretplatu](https://account.windowsazure.com/subscriptions), ako još nemate jedno mjesto. Saznajte kako možete [otvoriti račun za Azure besplatno](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

* Potreban vam je račun za [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) da biste zadržali povijesti probno performansi. Odgovarajući račun stvorit će se automatski kada postavljate testiranju performansi. Ili možete stvoriti novi račun ili postojeći račun ako ste vlasnik računa. 

* Implementacija aplikacije za testiranje u okruženju koje nisu radnog. Imati aplikacije koristite tarifu aplikacije servisa za osim plan koji se koriste u radni. Na taj način ne utječe na postojeće korisnike ili usporavanje aplikacije u radnog. 

## <a name="set-up-and-run-your-performance-test"></a>Postavljanje i pokretanje testiranju performansi

0.  Prijava na [Portal za Azure](https://portal.azure.com). Da biste koristili račun za Visual Studio Team Services da ste vlasnik, prijavite se kao vlasnik računa.

0.  Idite na web-aplikaciju.

    ![Idite na Pregledaj sve, web-aplikacijama web-aplikaciju programa](./media/app-service-web-app-performance-test/azure-np-web-apps.png)

0.  Idite na **performanse Test**.

    ![Idite na Alati za testiranje performansi](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
 
0. Sada ćete povezati račun za [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) da biste zadržali povijesti probno performansi.

    Ako imate račun Team Services da biste koristili, odaberite taj račun. Ako ne, stvorite novi račun.

    ![Odaberite postojeći račun servisa tima ili stvorite novi račun](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)

0.  Stvaranje testiranju performansi. Postavite detalje, a zatim pokrenite test. 

Možete pogledati rezultate u stvarnom vremenu tijekom testiranja.

Na primjer, pretpostavimo imamo aplikacije koji je dao izgleda kupona pri Prodaja praznik prošle godine. Ovaj događaj lasted 15 minuta s opterećenjem Vršna 100 Istodobni klijenata. Želimo dvostruki broj kupaca ove godine. Želimo poboljšanja zadovoljstva smanjivanjem vremena učitavanja stranice od 5 sekundi dvije sekunde. Tako, ne možemo testirajte performanse naš ažurirane aplikacije s 250 korisnika za 15 minuta.

Ne možemo ćete kao zamjenu za učitavanje na naša aplikacija tako da generira virtualne korisnika (Kupci) koji je posjetite naš web-mjesto u isto vrijeme. Time ćete prikazati nam zahtjeva za koliko se ne uspijeva reagira ili sporo.

  ![Stvaranje, postavljanje i pokretanje testiranju performansi](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

   *  Zadani URL za web app automatski se dodaju. 
   Možete promijeniti URL-a da biste testirali druge stranice (HTTP GET zahtjeve samo).

   *  Da biste kao zamjenu za lokalni uvjete i smanjivanje Latencija, odaberite mjesto koje se nalazi najbliže korisnicima za generiranje Učitaj.

  Evo test u tijeku. Tijekom prvog minute našu stranicu sporije od želimo učitava.

  ![Testiranje performanse u tijeku s podatke u stvarnom vremenu](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)

  Po završetku test smo Saznajte mnogo brže učitava stranice nakon prve minute. Tako da odredite gdje ćemo možda želite pokrenuti otklanjanje problema.

  ![Testiranje dovršene performanse prikazuje rezultate, uključujući neuspjelih zahtjeva](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>Testirajte sljedeće URL-više

Možete i pokrenuti performanse testira uključite više URL-ovi koji predstavljaju scenarij u kraj krajnjeg korisnika po prijenos datoteke programa Visual Studio Test za Web. Neki od načina možete stvoriti u Visual Studio Web Test su datoteke:

* [Snimite promet pomoću Fiddler i izvezite u obliku datoteke za Visual Studio Test za Web](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [Stvaranje datoteke za testiranje Učitaj u Visual Studio](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

Da biste prenijeli i pokrenite datoteku za Visual Studio Web Test:
 
0. Slijedite korake da biste otvorili plohu **testirajte novu performansi** .
   U ovom plohu odaberite mogućnost CONFIGFURE TESTIRANJE POMOĆU da biste otvorili plohu **Konfiguriraj Probno korištenje** .  

    ![Otvaranje učitavanja Konfiguriraj testiranje plohu](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)

0. Provjerite je li vrsta PROBNO postavljeno za **Visual Studio Web Test** , a zatim odaberite HTTP arhivsku datoteku.
    Pomoću ikona "mape" da biste otvorili dijaloški okvir za odabir datoteke.

    ![Prijenos više datoteka Web Test za Visual Studio URL-a](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)

    Nakon prijenosa datoteka prikazat će se popis URL-ovi testira u odjeljku pojedinosti o URL-a.
 
0. Odredite opterećenje korisnika i testiranje trajanje, a zatim **pokrenite test**.

    ![Odabir korisnika opterećenja i trajanje](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)

    Kada testiranje završi, vidjet ćete rezultate u dva okna. U lijevom oknu prikazuje performnace podatke kao niz grafikona.

    ![Okno s rezultatima performansi](./media/app-service-web-app-performance-test/multiple-01a-results.png)

    U desnom oknu prikazuje popis nije uspjelo zahtjeva za vrstu pogreške i koliko je puta je došlo je do.

    ![U oknu zahtjev neuspjeha](./media/app-service-web-app-performance-test/multiple-01b-results.png)

0. Ponovno pokrenite test tako da odaberete ikonu **ponovno pokrenite** pri vrhu u desnom oknu.

    ![Rerunning test](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

##  <a name="q--a"></a>Značajka pitanja i odgovora

#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>P: postoji li ograničenje na koliko se pokrenuti testiranje? 

**A**: da, možete pokrenuti testiranju jedan sat na portalu za Azure.

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>P: koliko je vremena mogu doći do pokrenuti testove performansi? 

**A**: nakon javno pretpregled dobijete 20 000 virtualne korisnika minuta (VUMs) besplatna svakog mjeseca s računom za Visual Studio Team Services. U VUM je broj korisnika virtualne množen broj minuta u testiranju. Ako vašim potrebama prekorače Slobodno ograničenje, možete kupiti još jedanput i platiti samo koje koristite.

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>P: gdje provjeriti koliko VUMs koje ste dosad koristili?

**A**: možete provjeriti ovoliko na portalu za Azure.

![Idite na račun servisa tima](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![Provjera VUMs koristi](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>P: što je zadana mogućnost i su utjecati na moj postojeće testira?

**A**: zadana mogućnost za performansi učitavanja testira je ručno test - isto kao prije više URL testirajte mogućnost dodan na portal.
Postojeće testiranja nastaviti koristiti konfigurirani URL i funkcionirat će kao prije.

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>P: koje značajke nisu podržane u Visual Studio Web Test datoteku?

**A**: trenutno ta značajka ne podržava dodatke, izvora podataka i pravila izdvajanjem probno Web. Potrebno je urediti probno Web datoteku da biste ih uklonili. Nadamo da biste dodali podrške za te značajke u budućnosti ažuriranja.

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>P: ga podržava drugi oblici datoteka probno Web?
  
**A**: pri izlaganje samo Visual Studio Web Test podržani su datoteke u obliku.
Ne možemo bi postane da biste čuli ako vam je potrebna podrška za drugi oblici datoteka. Slanje e-poštom nam se na [vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com).

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>P: što još možete učiniti s računom za Visual Studio Team Services?

**A**: da biste pronašli na novi račun, idite na ```https://{accountname}.visualstudio.com```. Zajedničko korištenje kodu, sastavljanje, testiranje, praćenje rada i isporuke softver – sve u oblak pomoću alata ni jezik. Saznajte više o kako [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) značajke i servise pomoći timom mnogo je jednostavnija Suradnja i implementacija kontinuirano.

## <a name="see-also"></a>Vidi također

* [Pokrenuti testove performanse jednostavne oblaka](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [Pokrenuti testove Apache Jmeter performansi](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [Snimanje i ponavljanje testira oblaku opterećenja](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [Performanse testiranje aplikacije u oblaku](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)
