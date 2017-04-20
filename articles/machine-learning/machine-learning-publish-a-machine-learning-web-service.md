<properties
    pageTitle="Uvođenje web-servis za učenje stroj | Microsoft Azure"
    description="Kako pretvoriti osposobljavanje eksperimentiraj predvidljivih eksperimentiraj, pripremiti za uvođenje, a zatim uvesti kao Azure stroj učenje web-usluge."
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="garye"/>

# <a name="deploy-an-azure-machine-learning-web-service"></a>Uvođenje Azure stroj učenje web-usluge

Azure učenje stroj omogućuje sastavljanje, testiranje i uvođenje predvidljivih analitičke rješenja.

Iz visoke razine točke-od-prikaza, to možete učiniti u tri koraka:

- **[Stvaranje eksperimentiraj osposobljavanje]** - Azure stroj učenje Studio je suradnju vizualni razvojno okruženje koje koristite za uvježbavanje i testiranje predvidljivih analytics modela pomoću podataka osposobljavanje koje dobavite.
- **[Ga predvidljivih eksperimentiraj pretvoriti]** - jednom vaš model ima je put uvježbavan s postojeće podatke i spremni ste koristiti osvojiti nove podatke, pripremu i pojednostaviti eksperimentiraj za predictions.
- **Implementirati kao web-servis** - uvođenje predvidljivih eksperimentiraj kao [Novi] ili [Klasični] Azure web-usluge. Korisnici mogu slati podatke vašem modela i primiti vaš model predictions.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="create-a-training-experiment"></a>Stvaranje eksperimentiraj osposobljavanje

Za uvježbavanje model predvidljivih analytics koristite Azure stroj učenje Studio za stvaranje osposobljavanje eksperimentiraj gdje obuhvaćaju različite moduli za učitavanje podataka osposobljavanje Priprema podataka prema potrebi, primijeniti stroj učenje algoritmi i procjenu rezultate. Možete iterirati pokusa i pokušajte različite strojne grupe učenje Algoritmi za uspoređivanje i procjenu rezultate.

Postupak stvaranja i upravljanje eksperimenata Osposobljavanje je pokriveni detaljniju negdje drugdje. Dodatne informacije potražite u člancima:

- [Stvaranje jednostavnog eksperimentiraj Azure stroj učenje Studio](machine-learning-create-experiment.md)
- [Razvoj predvidljivih rješenje s Azure stroj učenje](machine-learning-walkthrough-develop-predictive-solution.md)
- [Uvoz podataka osposobljavanje u Azure stroj učenje Studio](machine-learning-data-science-import-data.md)
- [Upravljanje eksperimentiraj iteracija u Azure stroj učenje Studio](machine-learning-manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Pretvaranje eksperimentiraj osposobljavanje eksperimentiraj predvidljivih

Kada ste put uvježbavan vaš model, spremni ste osposobljavanje eksperimentiraj pretvoriti predvidljivih eksperimentiraj osvojiti nove podatke.

Pretvaranjem predvidljivih eksperimentiraj dobivate obučeni model spreman uveden kao bodovanja web-usluge. Korisnici web-usluge mogu slati ulaznih podataka vaš model i modelu će poslati natrag predviđanje rezultata. Kao pretvorite predvidljivih eksperimentiraj, imajte na umu kako očekujete model koriste drugima.

Da biste pretvorili osposobljavanje eksperimentiraj predvidljivih eksperimentiraj, kliknite **Pokreni** na dnu područja crtanja eksperimentiraj, pritisnite **Skup web-usluga**, a zatim odaberite **Predvidljivih web-usluge**.

![Pretvori u bodovanja eksperimentiraj](./media/machine-learning-publish-a-machine-learning-web-service/figure-1.png)

Dodatne informacije o kako izvesti ovaj pretvorbe potražite u [pretvoriti eksperimentiraj osposobljavanje učenje stroj za predvidljivih eksperimentiraj](machine-learning-convert-training-experiment-to-scoring-experiment.md).

Sljedeći koraci opisuju implementaciji predvidljivih eksperimentiraj kao nove web-usluge. Možete uvesti i pokusa kao Classic web-usluge.

## <a name="deploy-the-predictive-experiment-as-a-new-web-service"></a>Uvođenje predvidljivih eksperimentiraj kao nove web-usluge

Sada da predvidljivih eksperimentiraj pripremljen, možete je uvesti kao Azure web-usluge. Pomoću web-usluga, korisnici mogu slati podatke vašem modela i model će vratiti njegove predictions.

Uvođenje predvidljivih eksperimentiraj kliknite **Pokreni** na dnu eksperimentiraj područje crtanja. Nakon pokusa završi s izvođenjem pritisnite **Uvođenje web-usluga** i odaberite **uvođenje web-usluga [Novo]**.  Otvara se stranica uvođenja portal stroj učenje web-usluge.

### <a name="machine-learning-web-service-portal-deploy-experiment-page"></a>Uvođenje eksperimentiraj stranici portala stroj učenje web-usluge

Na stranici uvođenje eksperimentiraj unesite naziv za web-uslugu.
Odaberite plan određivanja cijena. Ako imate postojećeg plana određivanja cijena možete je odabrati, u suprotnom morate stvoriti novi plan cijena za servis.

1.  **Cijena Plan** padajući popis, odaberite postojeći plan ili odaberite mogućnost **Odaberite novi plan** .
2.  **Naziv plana**, upišite naziv koji će prepoznati plan na računu.
3.  Odaberite **Razine mjesečni Plan**. Zadane razine plan za planove za zadanu regiju i web-usluge je uveden to područje.

Kliknite **uvođenja** i **brzo pokretanje** stranice za svoje otvara web usluge.

Web-servis brzo pokretanje stranice daje vam pristup i upute na većinu uobičajenih zadataka će izvođenje nakon stvaranja web-usluge. Ovdje možete jednostavno pristupiti probnu stranicu i stranicu potroši.

<!-- ![Deploy the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-web-service"></a>Testiranje web-usluge

Da biste testirali nove web-usluge, pritisnite **web-servis za testiranje** pod uobičajene zadatke. Na stranici testiranje možete testirati web-usluga kao u odgovor na zahtjev usluge (RRS) ili obradu izvršavanja servisa (pored stavke).

RRS probna stranica prikazuje ulaza, izlaza i globalne parametara koje ste definirali za pokusa. Da biste testirali web-uslugu, možete ručno unijeti odgovarajuće vrijednosti za ulaza ili opskrbu zarezom zarezima (CSV) oblikovani datoteka koja sadrži vrijednosti test.

Za testiranje pomoću RRS, iz načina prikaza popisa, unesite odgovarajuće vrijednosti za ulaza i kliknite **Testiranje-odgovor na zahtjev**. Predviđanje rezultate prikazati u izlazni stupac lijevo.

![Uvođenje web-usluge](./media/machine-learning-publish-a-machine-learning-web-service/figure-5-test-request-response.png)

Za testiranje vaše pokraj stavke kliknite **serije**. Na serije probne stranice pod unos pritisnite Pregledaj i odaberite CSV datoteku koja sadrži odgovarajuće ogledne vrijednosti. Ako nemate CSV datoteku, a stvorili vaše predvidljivih eksperimentiraj pomoću Studio učenje strojne grupe, možete preuzeti skup podataka za predvidljivih eksperimentiraj i koristiti ga.

Da biste preuzeli skup podataka, otvorite učenje Studio strojne grupe. Otvorite predvidljivih eksperimentiraj i desnom tipkom miša kliknite unos za svoj eksperimentiraj. Kontekstnom izborniku odaberite **dataset** , a zatim odaberite **Preuzimanje**.

![Uvođenje web-usluge](./media/machine-learning-publish-a-machine-learning-web-service/figure-7-mls-download.png)

Kliknite **testiranje**. Stanje izvršavanja obrada posla prikazuje desno pod **Testiranja obrade**.

![Uvođenje web-usluge](./media/machine-learning-publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)-->

Na stranici **KONFIGURACIJA** možete promijeniti opis, naslov, ažuriranje ključa za pohranu računa i omogućiti uzorak podataka za web-uslugu.

![Konfiguriranje web-usluge](./media/machine-learning-publish-a-machine-learning-web-service/figure-8-arm-configure.png)

Nakon što ste implementirati web-uslugu, možete:

- **Access** ga kroz API web usluge.
- **Upravljanje** ga kroz Azure stroj učenje web-portal services ili Azure klasični portal.
- **Ažuriranje** ga ako vaš model mijenja.

### <a name="access-the-web-service"></a>Pristup web-usluge

Nakon uvođenja web-usluge iz Studio učenje strojne grupe možete slanje podataka usluge i programski primati odgovore.

Stranica **potroši** pruža sve informacije potrebne za pristup web-usluge. Na primjer, API ključ je ugrađena Dopusti ovlašteni pristup servisu.

Dodatne informacije o pristupanju učenje stroj web-uslugu pogledajte [kako zauzeti web-usluga za učenje stroj Azure uvodi](machine-learning-consume-web-services.md).

### <a name="manage-your-new-web-service"></a>Upravljanje nove web-usluge

Možete upravljati klasični web-portala za usluge stroj učenje web-usluge. Iz [glavnog stranici portala](https://services.azureml-test.net/)pritisnite **Web-usluge**. Iz web-usluge stranice možete izbrisati ili kopirati usluge. Da biste pratili određenu uslugu, kliknite servis, a zatim **nadzorne ploče**. Nadgledanje obrade pridružene web-usluge, kliknite **Zapisnik zahtjev za obradu**.

## <a name="deploy-the-predictive-experiment-as-a-classic-web-service"></a>Uvođenje predvidljivih eksperimentiraj kao Classic web-usluge

Sada kad predvidljivih eksperimentiraj dovoljno je pripremljen, možete je uvesti kao Azure web-usluge. Pomoću web-usluga, korisnici mogu slati podatke vašem modela i model će vratiti njegove predictions.

Uvođenje predvidljivih eksperimentiraj **pokrenuti** na dnu eksperimentiraj područje crtanja, a zatim **Uvođenje web-usluge**. Web-servis postaviti i su smjestiti u nadzornu ploču web usluge.

![Uvođenje web-usluge](./media/machine-learning-publish-a-machine-learning-web-service/figure-2.png)

Testirajte web-usluge u portal stroj učenje web-usluge ili Studio učenje strojne grupe.

Da biste testirali odgovor na zahtjev web-usluge, kliknite gumb **testiranje** nadzornoj ploči web usluge. Dijalog iskače zatražiti unos podataka za servis. To su stupci očekivani bodovanja eksperimentiraj. Unesite skup podataka i kliknite **u redu**. Rezultati generira web-usluga prikazuju se na dnu nadzorne ploče.

Možete kliknite Pretpregled vezu **testiranje** da biste testirali usluge u portal Azure stroj učenje web-usluge kao što je prethodno prikazano u novu sekciju usluge web.

Da biste testirali izvršavanja servisa obradu, kliknite **testiranje** pretpregled vezu. Na serije probne stranice pod unos pritisnite Pregledaj i odaberite CSV datoteku koja sadrži odgovarajuće ogledne vrijednosti. Ako nemate CSV datoteku, a stvorili vaše predvidljivih eksperimentiraj pomoću Studio učenje strojne grupe, možete preuzeti skup podataka za predvidljivih eksperimentiraj i koristiti ga.

![Testiranje web-usluge](./media/machine-learning-publish-a-machine-learning-web-service/figure-3.png)

Na stranici **KONFIGURACIJA** možete promijeniti naziv prikaza servisa i dati opis. Naziv i opis je prikazan u [Azure klasični portal](http://manage.windowsazure.com/) gdje možete upravljati web-usluge.

Možete unijeti opis za unos podataka, izlazne podatke i parametre web servisa unosom niz za svaki stupac pod **Unos SHEMU**, **SHEMA IZLAZ**i **PARAMETAR web-usluge**. Ovi opisi se koriste u uzorak koda dokumentaciju za web-uslugu.

Možete omogućiti zapisivanje dijagnosticirati pogreške koje vidite kada pristupiti web-usluzi. Za dodatne informacije pogledajte [Omogući bilježenje za učenje stroj web-usluge](machine-learning-web-services-logging.md).

![Konfiguriranje web-usluge](./media/machine-learning-publish-a-machine-learning-web-service/figure-4.png)

Krajnje točke za web-uslugu možete konfigurirati i slične postupak prethodno prikazano u novu sekciju usluge web portalu Azure stroj učenje web-usluge. Mogućnosti su drugačije, možete dodati ili promijeniti opis servisa, omogućiti zapisivanje i Omogući uzorak podataka za testiranje.

### <a name="access-the-web-service"></a>Pristup web-usluge

Nakon uvođenja web-usluge iz Studio učenje strojne grupe možete slanje podataka usluge i programski primati odgovore.

Nadzorne ploče daje sve informacije potrebne za pristup web-usluge. Ako, na primjer, ključ API navedeni dopustiti ovlašteni pristup servisu i stranice API Pomoć pruža pomoć početak pisanja koda.

Dodatne informacije o pristupanju učenje stroj web-uslugu pogledajte [kako zauzeti web-usluga za učenje stroj Azure uvodi](machine-learning-consume-web-services.md).

### <a name="manage-the-web-service"></a>Upravljanje web-usluge

Postoje različite akcije koje možete izvesti za nadgledanje web-usluge. Ažurirati i izbrišite ga. Klasični web-usluge uz zadana krajnja točka koja je stvorena prilikom uvođenja ga možete dodati dodatne krajnje točke.

Za dodatne informacije pogledajte [Upravljanje Azure stroj učenje radnog prostora](machine-learning-manage-workspace.md) i [Upravljanje web-usluge korištenjem portal Azure stroj učenje web-usluge](machine-learning-manage-new-webservice.md).

<!-- When this article gets published, fix the link and uncomment
For more information on how to manage Azure Machine Learning web service endpoints using the REST API, see **Azure machine learning web service endpoints**.
-->

## <a name="update-the-web-service"></a>Ažuriranje web-usluge

Možete promjene web-usluge, kao što su ažuriranje model s Dodatna obuka podataka i ga ponovno implementirati prepisivanja izvornog web-usluge.

Da biste ažurirali web-usluge, otvorite izvornu eksperimentiraj predvidljivih koristi za uvođenje web-usluge i provjerite može uređivati kopiju klikom na **SPREMI kao**. Izvršite promjene, a zatim **Uvođenje web-usluge**.

Jer ste uveden eksperimentiraj prije, upitani ste želite prebrisati (klasični web-usluge) ili ažurirati postojeći servis (nove web-usluge). Klikom na **da** ili **Ažuriranje** zaustavlja postojeće web-usluge i uvodi novi predvidljivih eksperimentiraj uveden na njegovu mjestu.

> [AZURE.NOTE] Ako ste napravili promjene konfiguracije u izvornom web-usluge, na primjer, unosom novog zaslonsko ime ili opis, morat ćete ponovno unijeti te vrijednosti.

Je programski retrain model jednu mogućnost za ažuriranje web-usluge. Dodatne informacije potražite u [Retrain stroj učenje programski modelima](machine-learning-retrain-models-programmatically.md).


<!-- internal links -->
[Stvaranje eksperimentiraj osposobljavanje]: #create-a-training-experiment
[Pretvori u predvidljivih eksperimentiraj]: #convert-the-training-experiment-to-a-predictive-experiment
[novi]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Klasični]: #deploy-the-predictive-experiment-as-a-new-Web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
