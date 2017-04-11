<properties
    pageTitle="Početak rada s konfiguriranog rješenja | Microsoft Azure"
    description="Slijedite ovaj Praktični vodič da biste saznali kako implementirate Azure IoT paket unaprijed konfigurirane."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/16/2016"
     ms.author="dobett"/>

# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Praktični vodič: Početak rada s konfiguriranog rješenja

## <a name="introduction"></a>Uvod

Azure IoT paket [konfiguriranog rješenja] [ lnk-preconfigured-solutions] kombiniranje većem broju servisa Azure IoT izlaganje završetka do kraja rješenja koja implementirati uobičajene poslovne scenarije IoT. Rješenje *udaljene nadzor* unaprijed konfigurirane povezuje i nadzire uređajima. Rješenje možete koristiti da biste analizirali strujanje podatke s uređaja i poboljšanje tvrtke ishoda uspostavljanjem procesa koji se automatski odgovor na tom strujanja podataka.

Pomoću ovog praktičnog vodiča objašnjava Dodjela udaljene nadzora konfiguriranog rješenja. Također vas vodi kroz osnovne se značajke udaljene nadzora rješenja. Mnoge od tih značajki možete pristupiti putem nadzorne ploče rješenja koja uvodi zajedno s konfiguriranog rješenja:

![Udaljena nadzor unaprijed konfigurirane rješenje nadzorne ploče][img-dashboard]

Da biste dovršili ovaj Praktični vodič, potrebno vam je aktivna pretplata na Azure.

> [AZURE.NOTE]  Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Besplatnu probnu verziju Azure][lnk_free_trial].

[AZURE.INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Prikaz nadzorne ploče rješenja

Na nadzornoj ploči rješenja omogućuje vam da biste upravljali distribuiranih rješenja. Na primjer, možete prikaz telemetrijskih, dodajte uređaje i konfigurirati pravila.

1.  Kada za dodjelu resursa dovršetka pločicu za rješenje konfiguriranog označava **spreman**, kliknite **Pokreni** da biste otvorili udaljene nadzora rješenje portal na novoj kartici.

    ![Pokretanje konfiguriranog rješenja][img-launch-solution]

2.  Prema zadanim postavkama portala za rješenja prikazuje *rješenje nadzorne ploče*. Možete odabrati druge prikaze pomoću izbornika slijeva.

    ![Udaljena nadzor unaprijed konfigurirane rješenje nadzorne ploče][img-dashboard]

Na nadzornoj ploči prikazuju se sljedeći podaci:

- Karte prikazuje mjesto svakog uređaja povezanih s rješenja. Kada prvi put pokrenete rješenje, postoje četiri Simulirani uređaja. Simulirani uređaje primjenjuju kao Azure WebJobs i rješenje koristi API za Bing karte za iscrtavanje podataka na karti.
- Ploča za **Telemetriju povijest** iscrtava humidity i temperatura telemetrijskih s odabranog uređaja u blizu stvarnom vremenu i prikazuje prikupljanje podataka kao što je humidity maksimalno, minimalne i average.
- Ploča za **Povijest alarma** sadrži nedavne događaje alarma kada je vrijednost za telemetriju premašila praga. Možete definirati vlastite alarmi uz primjere stvorio konfiguriranog rješenja.

## <a name="view-the-device-list"></a>Prikaz popisa uređaja

Na popisu uređaja prikazuju registrirani uređaja u rješenje. Koji prikaz i uređivanje metapodataka za uređaj, dodajte ili uklonite uređaje i slanje naredbi na uređaje.

1.  Kliknite **uređaji** na lijevom izborniku da biste prikazali *popis uređaja* za rješenja.

    ![Popis uređaja u nadzorne ploče][img-devicelist]

2.  Na popisu uređaja prikazuje da postoje četiri Simulirani uređaje stvorio postupka dodjele resursa.

3.  Kliknite uređaj na popisu uređaja da biste prikazali Detalji o uređaju.

    ![Detalji o uređaju nadzornoj ploči][img-devicedetails]

**Detalji o uređaju** ploča sadrži tri odjeljka:

- U odjeljku **Akcije** navedene akcije koje možete izvesti na uređaju. Ako onemogućite uređaj, više nije dopušteno telemetrijskih slati ni primati naredbe. Ako onemogućite uređaj, možete pa ga ponovno omogućiti. Možete dodati pravilo povezano s uređajem koji se pokreće alarma kada je vrijednost za telemetriju premašuje praga. Naredbe možete poslati i na uređaj. Kada se prvi put spaja uređaj, datotečni nastavak govori rješenje naredbe možete odgovoriti.
- Odjeljak **Svojstva uređaja** popise metapodataka za uređaj. Neke od ovaj metapodataka dolazi iz samom uređaju (kao što je proizvođač), a neke generira rješenje (kao što su stvoreni vrijeme). Možete urediti metapodataka za uređaj na tom mjestu.
- Odjeljak **Provjere autentičnosti tipke** popise tipki na uređaju možete koristiti za provjeru autentičnosti uz rješenje.

## <a name="send-a-command-to-a-device"></a>Slanje naredbe na uređaj

Okno s detaljima uređaj prikazuje sve naredbe s podrškom za određeni uređaj i omogućuje vam slanje naredbi na uređaj. Kada prvi put pokrene na uređaju, šalje informacije o naredbama podržava rješenje.

1.  Kliknite **naredbi** u oknu s detaljima uređaj za odabrani uređaj.

    ![Naredbe za uređaj u nadzorne ploče][img-devicecommands]

2.  Odaberite **PingDevice** popis naredbi.

3.  Kliknite **Pošalji naredbe**.

4.  Možete vidjeti status naredbe u povijest naredbi.

    ![Naredba stanje na nadzornoj ploči][img-pingcommand]

Rješenje prati status svaku naredbu šalje. Rezultat je prethodno **na čekanju**. Kada uređaj javlja da je izvršiti naredbu, rezultat je postavljeno na **uspjeh**.

## <a name="add-a-new-simulated-device"></a>Dodavanje novog Simulirani uređaja

Ako pokrenete konfiguriranog rješenja, automatski Dodjela četiri uzorka uređaje vidjet ćete na popisu uređaja. Ti uređaji su *Simulirani uređaja* s operacijskim sustavom u programa Azure WebJob. Simulirani uređaje olakšavaju Eksperimentirajte s konfiguriranog rješenja bez potrebe za implementaciju realni, fizičke uređaje. Ako se želite povezati realni uređaj rješenje, potražite u članku [Povezivanje uređaj tako da u alat za analizu daljinske nadzor konfiguriranog rješenje] [ lnk-connect-rm] vodič.

Sljedeći koraci pokazuju kako da biste dodali Simulirani uređaj rješenje:

1.  Otvorite popis uređaja.

2.  Kliknite **+ Dodaj A uređaj** u donjem lijevom kutu da biste dodali na uređaj.

    ![Dodavanje uređaja prethodno rješenje][img-adddevice]

3.  Kliknite **Dodaj novi** na pločici **Simulirani uređaja** .

    ![Postavljanje novim detaljima o uređaju u nadzorne ploče][img-addnew]
    
    Osim stvaranja novog Simulirani uređaj fizički uređaj možete dodati Ako odaberete da biste stvorili **Prilagođeni uređaja**. Dodatne informacije o povezivanju fizičkih uređaja rješenje potražite u članku [Povezivanje uređaja u paketu IoT udaljene konfiguriranog rješenja za nadzor][lnk-connect-rm].

4.  Odaberite **Omogući mi definiranje vlastitu ID uređaja**pa unesite naziv ID jedinstveni uređaja kao što su **mydevice_01**.

5.  Kliknite **Stvori**.

    ![Spremite novi uređaj][img-definedevice]

6. U koraku 3 **Dodavanje Simulirani uređaja**, kliknite **gotovo** da biste se vratili na popisu uređaja.

7. Na popisu uređaja možete pogledati uređaju **pokrenut** .

    ![Prikaz novi uređaj popisu uređaja][img-runningnew]

8. Možete pogledati i Simulirani telemetrijskih na novi uređaj na nadzornoj ploči:

    ![Prikaz telemetrijskih na novi uređaj][img-runningnew-2]

## <a name="edit-the-device-metadata"></a>Uređivanje metapodataka za uređaj

Kada uređaj najprije spaja rješenje, šalje njegove metapodatke rješenje. Prilikom uređivanja metapodataka za uređaj putem nadzorne ploče rješenja, šalje nove vrijednosti metapodataka na uređaju i sprema nove vrijednosti u bazu podataka za DocumentDB rješenja. Dodatne informacije potražite u članku [registra identiteta uređaja i DocumentDB][lnk-devicemetadata].

1.  Otvorite popis uređaja.

2.  Odaberite novi uređaj **Popisa uređaja**, a zatim kliknite **Uređivanje** da biste uredili **Svojstva uređaja**:

    ![Uređivanje metapodataka za uređaj][img-editdevice]

3. Pomaknite se dolje i unos promjena u vales zemljopisnu širinu i dužinu. Zatim kliknite **Spremi promjene u registar uređaja**.

    ![Uređivanje metapodataka za uređaj][img-editdevice2]

4. Vratite se na nadzornoj ploči, mjesto uređaja promijenio na karti:

    ![Uređivanje metapodataka za uređaj][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Dodavanje pravila za na novi uređaj

Nema pravila za na novi uređaj koji ste upravo dodali. U ovom ćete odjeljku dodajte pravilo koje pokreće alarma kada temperatura dojavi na novi uređaj premaši 47 stupnjeva. Prije nego što počnete, imajte na umu da povijest telemetrijskih na novi uređaj na nadzornoj ploči prikazuje temperatura uređaj nikad ne premašuje 45 stupnjeva.

1.  Otvorite popis uređaja.

2.  Odaberite novi uređaj s **Popisa uređaja**, a zatim **Dodaj pravilo** da biste dodali pravilo za uređaj.

3. Stvorite pravilo koje koristi **temperatura** kao podatkovno polje i koristi **AlarmTemp** kao izlaz kada temperatura premašuje 47 stupnjeva:

    ![Dodavanje pravila uređaja][img-adddevicerule]

4. Kliknite **Spremi i pravila prikaza** da biste spremili promjene.

5.  Kliknite **naredbi** u oknu s detaljima uređaj za na novi uređaj.

    ![Dodavanje pravila uređaja][img-adddevicerule2]

6.  Na popisu naredbi odaberite **ChangeSetPointTemp** i postavite **SetPointTemp** 45. Zatim **Naredba Pošalji**:

    ![Dodavanje pravila uređaja][img-adddevicerule3]

7.  Idite na nadzornoj ploči rješenja. Nakon kratko vrijeme, prikazat će se novi unos u oknu **Povijesti alarma** kada temperatura dojavi novi uređaj premašuje prag 47 stupnjeva:

    ![Dodavanje pravila uređaja][img-adddevicerule4]

8. Možete pregledati i urediti sva pravila na stranici **pravila** nadzorne ploče:

    ![Popis pravila uređaja][img-rules]

9. Možete pregledati i urediti sve akcije koje možete poduzeti u odgovoru pravilo na stranicu **Akcije** na nadzornoj ploči:

    ![Popis uređaja akcije][img-actions]

> [AZURE.NOTE] Da biste definirali akcije koje možete poslati poruku e-pošte ili SMS PORUKE u odgovoru pravila ili integrirati s sustavom redak specifični za poslovanje putem [Aplikacije logike]moguće je[lnk-logic-apps]. Dodatne informacije potražite u članku [Povezivanje logike aplikaciju za vaše Azure IoT paket udaljene nadzor unaprijed konfigurirane rješenje][lnk-logicapptutorial].

## <a name="other-features"></a>Ostale značajke

Pomoću portala za rješenja možete pretraživati za uređaje s određene značajke kao što su broj modela:

![Traženje uređaja][img-search]

Možete onemogućiti uređaj i nakon što je onemogućen možete ga ukloniti:

![Onemogućavanje i uklanjanje uređaja][img-disable]

## <a name="behind-the-scenes"></a>U pozadini

Ako pokrenete konfiguriranog rješenja, postupak implementacije stvara višestrukih resursa u Azure pretplatu koju ste odabrali. Pogledate ove resurse Azure [portal][lnk-portal]. Postupak implementacije stvara **grupu resursa** pod nazivom temelji se na nazivu odabira konfiguriranog rješenje:

![Unaprijed konfigurirani rješenje na portalu za Azure][img-portal]

Postavke svakog resursa možete pogledati tako da ga odaberete na popisu resursa u grupu resursa.

Možete pogledati i izvorni kod konfiguriranog rješenja. Na udaljenom nadzora konfiguriranog rješenje izvornog koda se [azure iot – alat za analizu daljinske-nadgledanje] [ lnk-rmgithub] GitHub spremište:

- Mapa **DeviceAdministration** sadrži izvornog koda za nadzornu ploču.
- Mapa **Simulator** sadrži izvornog koda za Simulirani uređaj.
- Mapa **EventProcessor** sadrži izvornog koda za pozadinsku proces kojim se obrađuje dolazne telemetrijskih.

Kada završite, možete izbrisati konfiguriranog rješenja iz pretplate Azure [azureiotsuite.com] [ lnk-azureiotsuite] web-mjesta. Ovo web-mjesto omogućuje jednostavno izbrisati sve resurse koji su tamo stvaranja konfiguriranog rješenja.

> [AZURE.NOTE] Da biste izbrisali sve vezano uz konfiguriranog rješenja, izbrišite na [azureiotsuite.com] [ lnk-azureiotsuite] web-mjesta, a ne jednostavno izbrisati grupu resursa na portalu.

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste implementiran rad unaprijed konfigurirane rješenje, možete nastaviti Uvod u paketu IoT tako da pročitate u sljedećim člancima:

- [Udaljena nadzor unaprijed konfigurirane vodič rješenja][lnk-rm-walkthrough]
- [Povezivanje uređaja udaljene nadzora konfiguriranog rješenje][lnk-connect-rm]
- [Dozvole za azureiotsuite.com web-mjesta][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
