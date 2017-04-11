<properties
    pageTitle="Active Directory replikacije Status rješenja u prijava analitiku | Microsoft Azure"
    description="Paket rješenja Active Directory replikacije Status redovito nadzire okruženja servisa Active Directory za sve pogreške replikacije izvješća i rezultata na nadzornoj ploči za OMS."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="active-directory-replication-status-solution-in-log-analytics"></a>Active Directory replikacije Status rješenja u zapisnika Analytics

Active Directory je ključa komponenta Enterprise IT okruženju. Da biste bili sigurni visoke dostupnosti i visoke performanse, svaki kontrolera domene ima vlastitu kopiju baze podataka servisa Active Directory. Kontrolera domena replicirati međusobno da bi se Proširi promjene na razini tvrtke. Problemi s tom se postupku replikacije mogu prouzročiti raznih problema na razini tvrtke.

Paket rješenja AD replikacije Status redovito nadzire okruženja servisa Active Directory za sve pogreške replikacije izvješća i rezultata na nadzornoj ploči za OMS.

## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja
Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

- Agenata mora biti instaliran na kontrolera domena koje su članovi domene na vrednovanje ili na poslužiteljima člana konfiguriran za slanje podataka replikacije AD OMS. Da biste razumjeli kako povezati računala sa sustavom Windows OMS, potražite u članku [Povezivanje Windows računalima zapisnika analize](log-analytics-windows-agents.md). Ako je kontroler domene već dio sustava centar za komponentu Operations Manager okruženje koje želite povezati s OMS, potražite u članku [Povezivanje prijava analitiku u komponente Operations Manager](log-analytics-om-agents.md).
- Dodavanje rješenja Active Directory replikacije Status radnog prostora OMS koristeći postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).  Postoji bez daljnje konfiguracije obavezno.


## <a name="ad-replication-status-data-collection-details"></a>Detalji o zbirke podataka AD Status replikacije

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za AD replikacije Status.

| platforme | Izravni Agent | Agent za SCOM | Prostor za pohranu za Azure | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Windows|![Da](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Da](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![ne](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![ne](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Da](./media/log-analytics-ad-replication-status/oms-bullet-green.png)| svakih 5 dana|


## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Po želji, omogućite kontroler koji nisu domene za slanje podataka AD OMS
Ako ne želite povezati bilo kontrolera domene izravno OMS, možete koristiti bilo koje druge OMS povezani računalo u domeni da biste prikupili podatke za paket rješenja AD replikacije Status, a da se šalje podatke.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Da biste omogućili kontroler koji nisu domene za slanje podataka AD OMS
1.  Provjerite je li računalo član domene koje želite nadzirati pomoću AD replikacije Status rješenja.
2.  [Povezivanje na računalu sa sustavom Windows da biste OMS](log-analytics-windows-agents.md) ili [Povezivanje s postojećeg Operations Manager okruženja za OMS](log-analytics-om-agents.md), ako već niste povezani.
3.  Na tom računalu postavite sljedećeg ključa registra:
    - Ključ: **grupe HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management\<ManagementGroupName > \Solutions\ADReplication**
    - Vrijednost: **IsTarge**
    - Podaci vrijednosti: **true**

    >[AZURE.NOTE]Te promjene ne primjenjuje se do vaše ponovno pokretanje servisa Microsoft Agent nadzor (HealthService.exe).

## <a name="understanding-replication-errors"></a>Objašnjenje pogreške replikacije
Nakon što dodate AD replikacije status podataka koji se šalju na OMS, vidjet ćete sličnu ovoj pločicu na nadzornoj ploči za OMS koji označava koliko replikacije pogreške koju trenutno imate.  
![Status replikacije AD pločica](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritične pogreške replikacije** su oni koje su na ili iznad 75% od [Vrijeme odgode do brisanja](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) za skup stabala sustava Active Directory.

Kada kliknete pločicu, vidjet ćete dodatne informacije o pogreškama.
![Nadzorna ploča za AD Status replikacije](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)


### <a name="destination-server-status-and-source-server-status"></a>Status poslužitelja odredište i Status poslužitelja izvora
Ove blades prikazati stanje odredište i izvorišnog poslužitelja koja se pojave pogreške prilikom replikacije. Broj nakon naziva domene kontroler pokazuje broj ponavljanja pogrešaka na taj kontroler.

Pogreške za odredište poslužitelja i poslužitelja izvora prikazane su jer su neke probleme lakše otklanjanje poteškoća s iz perspektive izvorišnog poslužitelja, a drugi iz perspektive poslužitelja odredište.

U ovom primjeru, vidjet ćete da mnoge poslužitelje odredište otprilike imati isti broj pogreške, ali postoji jedan izvor poslužitelj (ADDC35) koji sadrži više pogrešaka od svih ostalih. Vjerojatno da postoji neki problem na ADDC35 koji je uzrok da bi se neće poslati podatke njegovi partneri replikacije je. Rješavanje problema na ADDC35 će vjerojatno riješiti mnoge pogreške koje se pojavljuju u Elektronička ploča poslužitelja odredište.

### <a name="replication-error-types"></a>Vrste pogreške replikacije
U ovom plohu daje informacije o vrstama pogrešaka otkriven cijeloj tvrtki. Svaku pogrešku ima jedinstvene kod numeričke i poruku koja pojednostavljuju određivanje uzrok pogreške.

Prstenastih pri vrhu daje uvid pogrešaka koje se prikazuju informacije i rjeđe u svom okruženju.

To može vam prikazati kada više kontrolera domena sučelje ista replikacije pogreška. U ovom slučaju, možda ćete moći otkrijte identifikaciju rješenja na kontroleru jednu domenu, a zatim ponovite na druge kontrolera domena utječe ista pogreška.

### <a name="tombstone-lifetime"></a>Vrijeme odgode do brisanja
Vrijeme odgode do brisanja određuje koliko izbrisanog objekta, se nazivaju u odgode do brisanja, se zadržavaju u bazi podataka servisa Active Directory. Kada izbrisanog objekta prosljeđuje vrijeme odgode do brisanja, prikupljanja za smeća je automatski uklanjaju iz baze podataka servisa Active Directory.

Vrijeme odgode do brisanja zadani je 180 dana za najnovije verzije sustava Windows, ali je 60 dana u starijim verzijama, a ga može promijeniti izričito administrator servisa Active Directory.

Nije važno je znati ako imate problema ponavljanja pogreške koje su Približavanje ili kojima je istekao vrijeme odgode do brisanja. Ako dva kontrolera domena pojave pogreške u replikacije nastavi prošle vrijeme odgode do brisanja, replikacije će biti onemogućena između kontrolera tih dvaju domena, čak i ako se popravcima podlozi replikacije pogreške.

Vrijeme odgode do brisanja plohu olakšava prepoznali mjesta gdje je u opasnost događa. Svaku pogrešku u na **više od 100% TSL** kategorija predstavlja particija koja sadrži ne replicirati između njegove izvorišne i odredišne poslužitelja za barem vrijeme odgode do brisanja za u skup stabala.

U tom slučaju jednostavno ispravljanje pogreške replikacije neće biti dovoljno. Barem, morat ćete ručno istražiti za prepoznavanje i čišćenja lingering objekte prije nego što ponovno pokrenuti replikacije. Čak i morati isključiti kontroler domene.

Osim prepoznavanje pogreške replikacije koje imaju ista i prošle vrijeme odgode do brisanja ćete želite obratite pažnju na sve pogreške kvartalne u kategorije **50 75% TSL** ili **TSL 75 100%** .

To su pogreške koje su jasno lingering ne tranzitne tako da su vjerojatno morate vaše intervencije da biste riješili. Dobra je vijest da oni su još nisu vrijeme odgode do brisanja. Ako odmah riješiti te probleme i replikacije *prije* dođu vrijeme odgode do brisanja, možete započeti s minimalnim ručno intervencije.

Kao što je naznačeno ranije, pločice nadzorne ploče za rješenje AD replikacije Status prikazuje broj *ključnih* replikacije pogrešaka u okruženju sustava koja je definirana kao pogreške koje su veće od 75% vrijeme odgode do brisanja (uključujući pogreške koje su veće od 100% TSL). Pokušajte da bi taj broj 0.

>[AZURE.NOTE] Sve izračune postotak vijek odgode do brisanja temelje se na stvarni odgode do brisanja vijek za vaše skup stabala servisa Active Directory tako pouzdanost tih postotaka jesu, čak i ako imate vrijednost trajanja prilagođene odgode do brisanja, postavite.

### <a name="ad-replication-status-details"></a>AD replikacije status pojedinosti
Kada kliknete bilo koju stavku u jedan od popisa, vidjet ćete dodatne detalje o njoj pomoću zapisnika pretraživanja. Rezultati su filtrirane da bi se prikazala samo pogreške vezane uz tu stavku. Ako, na primjer, ako kliknete kontrolerom domene prvi navedeni u odjeljku **Status odredište poslužitelja (ADDC02)**, vidjet ćete filtrirane tako da se rezultati pretraživanja Prikaži pogreške s taj kontroler navedena kao odredišnom poslužitelju:

![AD replikacije stanja pogreške u rezultatima pretraživanja](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Na tom mjestu možete filtrirati Dodatno, Preinaka upita za pretraživanje i tako dalje. Dodatne informacije o korištenju zapisnika pretraživanja potražite u članku [zapisnika pretraživanja](log-analytics-log-searches.md).

**HelpLink** prikazuje URL stranice TechNet s Dodatne detalje o tom problemu. Možete kopirati i zalijepiti tu vezu u prozoru preglednika da biste vidjeli informacije o otklanjanju poteškoća i ispravljanje pogreške.

Možete kliknuti i **Izvoz** da biste izvezli rezultate u programu Excel. Omogućuje vizualni prikaz podataka o pogrešci replikacije u bilo koji način želite.

![izvezeni AD replikacije stanja pogreške u programu Excel](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>Status replikacije AD najčešća Pitanja
**P: koliko često je ažurirati AD replikacije status podataka?**
O: podaci se ažurira svakih 5 dana.

**P: postoji način da biste konfigurirali učestalost ažuriranja te podatke?**
O: Zasad ne.

**P: moram da biste dodali sve moje kontrolera domene Moje OMS radnog prostora da biste vidjeli status replikacije?**
A: ne, samo kontroler jedne domene mora biti dodan. Ako imate više kontrolera domena u radnom prostoru OMS, OMS slanje podataka iz svih od njih.

**P: ne želim dodati sve kontrolera domena Moje OMS radnog prostora. Mogu li i dalje koristiti rješenja AD replikacije stanje?**
O: da. Možete postaviti vrijednost ključa registra da biste omogućili tu. Potražite u članku [Omogućivanje kontroler koji nisu domene poslati podatke AD OMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**P: što je naziv procesa koji se ne prikupljanje podataka?**
A: AdvisorAssessment.exe

**P: koliko dugo traje podataka prikupljenih?**
A: vrijeme zbirke podataka ovisi o veličini okruženju Active Directory, ali obično traje manje od 15 minuta.

**P: što vrstu podataka koji se prikupljaju?**
O: putem LDAP prikupljaju se podaci replikacije.

**P: postoji način da biste konfigurirali kada prikupljanja podataka?**
O: Zasad ne.

**P: koje dozvole potrebne da biste prikupili podatke?**
A: normalni korisničke dozvole sa servisom Active Directory su obično.

## <a name="troubleshoot-data-collection-problems"></a>Otklanjanje poteškoća zbirke podataka
Da biste prikupili podatke, paket rješenja AD replikacije Status potreban je barem jedan kontroler domene biti povezani s OMS radnog prostora. Dok je to učinite, vidjet ćete poruku te **i dalje se prikupljanja podataka**.

Ako vam je potrebna pomoć povezujete nešto kontrolera domene, možete pogledati dokumentaciju [računalima povezivanje Windows zapisnika analize](log-analytics-windows-agents.md). Osim toga, ako kontroler domene već povezan s postojećeg okruženja sustava centar za komponentu Operations Manager, možete pogledati dokumentaciju [Povezivanje sustava centar za komponentu Operations Manager da biste zapisnika analize](log-analytics-om-agents.md).

Ako ne želite da biste se povezali kontrolera domene, izravno OMS ili SCOM, potražite u članku [Omogućivanje kontroler koji nisu domene poslati podatke AD OMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).


## <a name="next-steps"></a>Daljnji koraci

- Da biste pogledali detaljne podatke za Active Directory replikacije status pomoću [zapisnika pretraživanja u zapisnik analize](log-analytics-log-searches.md) .
