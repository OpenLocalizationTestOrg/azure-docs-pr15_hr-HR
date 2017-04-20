<properties
    pageTitle="Korak 5: Uvođenje stroj učenje Web usluge | Microsoft Azure"
    description="Korak 5 razvoju vodič predvidljivih rješenja: uvođenje predvidljivih eksperimentiraj u stroj učenje Studio kao web-usluge."
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
    ms.date="10/05/2016"
    ms.author="garye"/>


# <a name="walkthrough-step-5-deploy-the-azure-machine-learning-web-service"></a>Prođite korak 5: Uvođenje Azure stroj učenje web-usluge

Ovo je peti korak vodič [razvoju predvidljivih analytics rješenja u Azure stroj učenje](machine-learning-walkthrough-develop-predictive-solution.md)


1.  [Stvaranje radnog prostora učenje strojne grupe](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Prijenos postojećih podataka](machine-learning-walkthrough-2-upload-data.md)
3.  [Stvaranje nove eksperimentiraj](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Uvježbavanje i procjenu na modeli](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  **Uvođenje web-usluge**
6.  [Pristup web-usluge](machine-learning-walkthrough-6-access-web-service.md)

----------

Dati drugima šansa da koristi predvidljivih model razvili smo u ovom prikazu, možemo implementirati kao web-usluge na Azure.

Do tog trenutka smo ste je experimenting s osposobljavanje naše modela. Ali distribuiranih usluga više ne namjeravate učiniti osposobljavanje - generira predictions po bodovanja unos korisnika na temelju naše model. Tako smo namjeravate učiniti neke pripreme za pretvaranje ovog eksperimentiraj iz eksperimentiraj ***Osposobljavanje*** za na ***predvidljivih*** eksperimentirati. 

Ovo je postupak u dva koraka:  

1. Pretvoriti u *Osposobljavanje eksperimentirati* ste stvorili smo *predvidljivih eksperimentiraj*
2. Uvođenje eksperimentiraj predvidljivih kao web-usluge

Ali prvo, moramo malo obrezivanja ovaj eksperimentiraj dolje. Trenutno imamo dvije različite modele u pokusa, ali želimo samo jedan modela kada to smo uvođenje kao web-usluge.  

Recimo da ćemo ste odlučili je li boosted Stablo modela bolje model za korištenje. Tako je prva stvar učiniti uklonite [Dvije klase podršku vektor stroj] [ two-class-support-vector-machine] modul i modulima koji su korišteni za osposobljavanje ga. Možda želite da prvo napravite kopiju pokusa klikom na **Spremi kao** na dnu eksperimentiraj područje crtanja.

Morati ćemo izbrisati sljedeće module:  

- [Podršku za dva klase vektor stroj][two-class-support-vector-machine]
- [Uvježbavanje Model] [ train-model] i [Model rezultat] [ score-model] module koji su povezani s njim
- [Normalizaciju podataka] [ normalize-data] (oba)
- [Procijeniti modela][evaluate-model]

Odaberite modul i pritisnite tipku Delete ili desnom tipkom miša pritisnite modul i odaberite **Izbriši**.

Sada ćemo spremni za uvođenje ovaj model pomoću [Stabla odluka Boosted dvije klase][two-class-boosted-decision-tree].

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Pretvaranje eksperimentiraj osposobljavanje eksperimentiraj predvidljivih

Pretvorba u predvidljivih eksperimentiraj uključuje tri koraka:

1. Spremanje modela smo ste put uvježbavan i zamijeni naše moduli za osposobljavanje
2. Obreži eksperimentiraj ukloniti module samo potrebne za osposobljavanje
3. Definirati gdje će web-usluga prihvaća unos i gdje generira Izlaz

Srećom, sve tri koraka možete obaviti tako da kliknete **Postavi Web usluga** na dnu područja crtanja eksperimentiraj (Odabir mogućnost **predvidljivih web-usluge** ).

Kada pritisnite **skup Web usluga**dogoditi nekoliko stvari:

- Obučeni model spremiti kao jedan **Obučeni Model** modul u paleti modul lijevo od područja crtanja eksperimentiraj (to možete pronaći pod **Obučeni modeli**).
- Moduli koji su korišteni za osposobljavanje uklanjaju. Posebno:
  - [Stabla odluka dva klase Boosted][two-class-boosted-decision-tree]
  - [Podučavanje modela][train-model]
  - [Podijeli podatke][split]
  - drugi [Izvršavanje skripti R] [ execute-r-script] modul koji je korišten za testiranje podataka
- Spremljeni model obučeni dodaje se u pokusa.
- **Web usluge ulaz** i **Izlaz Web usluge** module dodaju.

> [AZURE.NOTE] Pokusa spremljena je u dva dijela pod karticama koje su dodane na vrhu područja crtanja eksperimenta: izvorni eksperimentiraj Osposobljavanje je pod karticom **Osposobljavanje eksperimentirati**i novostvoreni predvidljivih eksperimentiraj je pod **Predictive eksperimentirati**.

Morati ćemo poduzeti dodatni korak s ovom određenom eksperimentiraj.
Smo dodali dva [Izvršavanje skripti R] [ execute-r-script] moduli za pružanje funkcija razine podataka za osposobljavanje i testiranja. Ne možemo trebate koji učinite u konačni modela.

Stroj učenje Studio uklonjena jedna [Izvršavanje skripti R] [ execute-r-script] modul kada ukloniti [Podijeli] [ split] modula. Sada možemo sada ukloniti druge i povezati [Metapodataka uređivač] [ metadata-editor] izravno na [Rezultat Model][score-model].    

Naše eksperimentiraj sada trebao bi izgledati ovako:  

![Obučeni model bodovanja][4]  

> [AZURE.NOTE] Koje možda se pitate iz Zašto smo lijevo dataset UCI njemački kreditne kartice podataka u predvidljivih eksperimentiraj. Servis namjeravate koristiti korisničke podatke, nije izvorne dataset, pa Zašto ostavite izvorni dataset u modelu?
>
>Je istinito servis ne trebate izvornih podataka kreditne kartice. Ali potrebno sheme za podatke, koje informacije obuhvaćaju koliko stupaca postoje i stupce koji su numerički. Informacije o shemi je potrebno protumačiti korisničke podatke. Možemo ostavite tih komponenti povezani tako da bodovanja modul ima dataset sheme kada servis pokrenut. Ne koristi podatke, samo sheme.  

Pokrenite eksperimentiraj jedan posljednji put (kliknite **Pokreni**.) Ako želite provjeriti još uvijek radi modela kliknite izlazni [Rezultat Model] [ score-model] modul i odaberite **Prikaz rezultata**. Vidjet ćete da izvorni podaci se prikazuju, kao vrijednost rizik odobrenja ("osvojila natpisi") i bodovanja vrijednost vjerojatnosti ("osvojila vjerojatnosti".) 

## <a name="deploy-the-web-service"></a>Uvođenje web-usluge

Možete uvesti eksperimentiraj kao klasični web-usluge ili nove web-usluge koji se temelji na Azure upravitelja resursa.

### <a name="deploy-as-a-classic-web-service"></a>Uvođenje kao klasični web-usluge ###

Uvođenje klasični web-usluga izvedene iz naše eksperimentiraj kliknite **Uvođenje web-usluga** ispod područje crtanja i odaberite **Uvođenje web-servisa [Classic]**. Stroj učenje Studio uvodi eksperimentiraj kao web-usluge i odvodi vas nadzorne ploče za web-usluzi. Ovdje možete povrata eksperimentiraj (**Prikaz snimka** ili **Prikaz najnovijih**) i pokrenuti jednostavnog testa web-usluge (pogledajte **Test web-usluga** ispod). Također je ovdje informacije za stvaranje aplikacija možete pristupiti web-usluge (više na koji u sljedećem koraku ovaj vodič).

![Web usluge nadzorne ploče][6]

Konfiguriranje usluge pritiskom na karticu **KONFIGURACIJE** . Ovdje možete izmijeniti naziv usluge (ga je dao naziv eksperimentiraj po zadanom) i dati opis. Možete dati i više neslužbeni natpise za stupce ulazni i izlazni.  

![Konfiguriranje web-usluge][5]  

### <a name="deploy-as-a-new-web-service"></a>Uvođenje kao nove web-usluge

Uvođenje nove web-usluge izvedene iz naše eksperimentiraj pritisnite **Uvođenje web-usluga** ispod područja crtanja i **Uvođenje web-servisa [novi]**. Stroj učenje Studio prenosi stranici uvođenje eksperimentiraj Azure stroj učenje Web services.

Unesite naziv za web-usluge i odaberite plan određivanja cijena. Ako imate postojećeg plana određivanja cijena možete je odabrati, u suprotnom morate stvoriti novi plan cijena za servis. 

1.  U padajućem **Cijena Plan** odaberite postojeći plan ili odaberite mogućnost **Odaberite novi plan** .
2.  **Naziv plana**, upišite naziv koji identificira plan na računu.
3.  Odaberite **Razine mjesečni Plan**. Zadane razine plan za planove za zadanu regiju i web-usluge je uveden to područje.

Kliknite **uvođenja** i **brzo pokretanje** stranice za svoje otvara Web usluge.

Konfiguriranje usluge klikom na mogućnost izbornika **Konfiguriraj** . Ovdje možete izmijeniti naziv usluge (ga je dao naziv eksperimentiraj po zadanom) i dati opis. 

Za testiranje Web usluga odaberite pritisnite mogućnost izbornika **testiranje** (pogledajte **web-servis za testiranje** ispod). Informacije o stvaranju aplikacije koje možete pristupiti web-usluzi, kliknite mogućnost izbornika **utrošak** (više na koji u sljedećem koraku ovaj vodič).

> [AZURE.TIP] Nakon što ste je uveden možete ažurirati web-usluge. Ako, na primjer, ako želite promijeniti vaš model uređivanje eksperimentiraj osposobljavanje, tweak model parametre i pritisnite **Uvođenje web-usluga**. Zatim odaberite **Uvođenje web-servisa [Classic]** ili **Uvođenje web-servisa [novi]**. Kada ponovno implementirati pokusa zamjenjuje web-usluge, sada koristeći ažurirani modela.  

## <a name="test-the-web-service"></a>Testiranje web-usluge

### <a name="test-a-classic-web-service"></a>Testiranje klasični web-usluge

Servis možete testirati u stroj učenje Studio ili portal Azure stroj učenje web-usluge. Testiranje portal Azure stroj učenje web-usluga ima prednost dopuštajući omogućiti 

**Testiranje stroj učenje Studio**

Na stranici **nadzorne PLOČE** , kliknite gumb **Test** pod **Zadana krajnja točka**. Dijalog će pop i zatražiti unos podataka za servis. Te su iste stupce koje su se dogodile u izvornom dataset rizik njemački odobrenja.  

Unesite skup podataka i kliknite **u redu**. 

**Testiranje portal Azure stroj učenje web-usluge**

Na stranici **nadzorne PLOČE** kliknite **testiranje** veze pretpregled pod **Zadana krajnja točka**. Probna stranica u portal Azure stroj učenje web-usluge za krajnje točke usluge Web otvara i pita za unos podataka za servis. Te su iste stupce koje su se dogodile u izvornom dataset rizik njemački odobrenja.

Kliknite **Testiranje - odgovor na zahtjev**. 

U web-uslugu podatke unosi kroz modul **Web usluge unos** putem [Uređivača metapodataka] [ metadata-editor] modula, i [Model rezultat] [ score-model] modul gdje je osvojila. Rezultati zatim izlaza iz web-servisa putem **Web usluga izlaza**.

**Testirajte nove web-usluge**

Portal Azure stroj učenje Web services kliknite **Test** na vrhu stranice. **Probna** stranica otvara i za unos podataka za servis. Unos prikazana polja odgovaraju stupcima koje su se dogodile u izvornom dataset rizik njemački odobrenja. 

Unesite skup podataka, a zatim kliknite **Test-odgovor na zahtjev**.

Na desnoj strani stranice u izlaznom stupcu će prikazati rezultate testa. 

Prilikom testiranja portal Azure stroj učenje web-usluge, možete omogućiti uzorak podataka koje možete koristiti za testiranje servisa odgovor na zahtjev. Ako ste stvorili web-usluge u stroj učenje Studio uzorak podataka se uzima iz podataka vaše korištene za uvježbavanje modelu.

> [AZURE.TIP] Imamo predvidljivih eksperimentiraj konfiguriran, način cijele rezultata iz [Modela rezultat] [ score-model] vratio modul. To uključuje ulaznih podataka plus rizik vrijednost odobrenja i bodovanja vjerojatnosti. Ako ste željeli da biste se vratili nešto drugo - ako, na primjer, samo potraživanje rizika vrijednost - a zatim može umetnuti [Stupce projekta] [ project-columns] modul između [Rezultat Model] [ score-model] i **izlaza Web usluga** da biste eliminirali stupce koje ne želite vratiti web-servisa. 

## <a name="manage-the-web-service"></a>Upravljanje web-usluge

**Upravljanje klasični web-usluge**

Nakon što ste uveden klasični web-uslugu, možete upravljati iz [Azure klasični portal](https://manage.windowsazure.com).

1. Prijavite se u [Azure klasični portal](https://manage.windowsazure.com).
2. Na ploči usluge Microsoft Azure kliknite **UČENJE strojne GRUPE**.
3. Kliknite na radni prostor.
4. Kliknite karticu **web-usluge** .
5. Pritisnite web-uslugu smo stvorili.
6. Pritisnite krajnje točke "Zadano".

Ovdje možete učiniti stvari poput nadzirati kako način web-usluge i provjerite tweaks performanse promjenom koliko Istodobni poziva servisa može rukovati.
Čak možete objaviti web-usluga na tržištu Azure.

Za više detalja pogledajte:

- [Stvaranje krajnje točke](machine-learning-create-endpoint.md)
- [Skaliranje web-usluge](machine-learning-scaling-webservice.md)
- [Objavi Azure stroj učenje Web servisa Azure Marketplace](machine-learning-publish-web-service-to-azure-marketplace.md)

**Upravljanje web-usluge u portal Azure stroj učenje web-usluge**

Nakon što ste implementirati web-usluge, klasični ili novo, možete upravljati iz [Azure stroj učenje Web services portal](https://servics.azureml.net).

Za nadgledanje performansi web-usluge:

1. Prijavite se u [Azure stroj učenje Web services portal](https://servics.azureml.net).
2. Kliknite **web-usluge**.
3. Kliknite web-usluge.
4. Kliknite **nadzorne ploče**.

----------

**Sljedeće: [Access web-usluge](machine-learning-walkthrough-6-access-web-service.md)**

[1]: ./media/machine-learning-walkthrough-5-publish-web-service/publish1.png
[2]: ./media/machine-learning-walkthrough-5-publish-web-service/publish2.png
[3]: ./media/machine-learning-walkthrough-5-publish-web-service/publish3.png
[4]: ./media/machine-learning-walkthrough-5-publish-web-service/publish4.png
[5]: ./media/machine-learning-walkthrough-5-publish-web-service/publish5.png
[6]: ./media/machine-learning-walkthrough-5-publish-web-service/publish6.png


<!-- Module References -->
[evaluate-model]: https://msdn.microsoft.com/library/azure/927d65ac-3b50-4694-9903-20f6c1672089/
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[metadata-editor]: https://msdn.microsoft.com/library/azure/370b6676-c11c-486f-bf73-35349f842a66/
[normalize-data]: https://msdn.microsoft.com/library/azure/986df333-6748-4b85-923d-871df70d6aaf/
[score-model]: https://msdn.microsoft.com/library/azure/401b4f92-e724-4d5a-be81-d5b0ff9bdb33/
[split]: https://msdn.microsoft.com/library/azure/70530644-c97a-4ab6-85f7-88bf30a8be5f/
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
[two-class-boosted-decision-tree]: https://msdn.microsoft.com/library/azure/e3c522f8-53d9-4829-8ea4-5c6a6b75330c/
[two-class-support-vector-machine]: https://msdn.microsoft.com/library/azure/12d8479b-74b4-4e67-b8de-d32867380e20/
[project-columns]: https://msdn.microsoft.com/en-us/library/azure/1ec722fa-b623-4e26-a44e-a50c6d726223/