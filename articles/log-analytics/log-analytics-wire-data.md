<properties
    pageTitle="Slika rješenje podatke u prijava analitiku | Microsoft Azure"
    description="Podaci za žičani konsolidirane podatke mrežom i performansama iz računalima s OMS agenata, uključujući Operations Manager i Windows povezani agenata. Mrežnih podataka u kombinaciji s podatke iz zapisnika za povezivanje podataka."
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
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="wire-data-solution-in-log-analytics"></a>Slika podataka rješenja u zapisnik Analytics

Podaci za žičani konsolidirane podatke mrežom i performansama iz računalima s agente OMS, uključujući Operations Manager i agente za Windows povezani. Mrežnih podataka u kombinaciji s podatke iz zapisnika za povezivanje podataka. OMS agenata na računalima u vaš IT infrastrukturu monitor mreže podataka koji se šalju i s tih računala za mreže razine 2-3 u [modelu upravljački](https://en.wikipedia.org/wiki/OSI_model) uključujući različite protokoli i priključke koristi.

>[AZURE.NOTE] Rješenje žičani podataka nije dostupan potrebno dodati u radni prostor. Korisnike koji već imaju rješenje žičani podataka omogućeno možete nastaviti koristiti rješenja žičani podataka.

Prema zadanim postavkama OMS prikuplja prijavljenih podataka za procesora, memorije, na disku i mrežnih performansi podataka iz mjerača ugrađen u sustavu Windows. Mreža i druge zbirke podataka se mijenja u stvarnom vremenu za svaki pojedini agent, uključujući podmreže i protokoli razini aplikacije koristi računalo. Na stranici Postavke na kartici zapisnici možete dodati druge mjerača performansi.

Ako ste iskoristili [sFlow](http://www.sflow.org/) ili neki drugi softver s [protokolom NetFlow Cisco korisnika](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html), zatim Statistika i podataka prikazat će se iz podataka žičani bit će vam poznata.

Neke vrste ugrađenog upita za pretraživanje zapisnika obuhvaćaju sljedeće:

- Agenata koje daju podatke žičani
- IP adresa agenata pružati žičani podatke
- Izlaznu komunikaciju prema IP adresama
- Broja bajtova poslao protokoli aplikacije
- Broja bajtova poslao servis za aplikaciju
- Bajtova primio različite protokola
- Ukupno bajtova šalje i prima po IP
- IP adrese kojima ste komunicirali s agente na podmreži 10.0.0.0/8
- Prosječne latencije za veze koje su mjeri pouzdano
- Računalo postupaka koji se pokreće ili Primljeno mrežni promet
- Iznos mrežni promet za proces

Kada pretražujete pomoću žičani podataka, filtriranje i grupiranje podataka za prikaz informacija o gornji agente i gornje protokola. Ili možete pogleda kada određene računala (IP adresa/MAC adrese) prenosi međusobno koliko dugo i kojom količinom podataka poslana – zapravo, prikazujete metapodatke o mrežnog prometa koji je temeljenih na pretraživanju.

No Budući da prikazujete metapodataka, nije nužno korisne su za detaljnije otklanjanje poteškoća. Žičani podataka u OMS nije cijeli snimanje mrežnih podataka. Tako da ga nije namijenjen duboke paketa razinom za otklanjanje poteškoća.
Prednost korištenja agent, u odnosu na druge načine zbirke je da ne morate instalirati aparata, konfigurirajte parametri na mreži ili preform složene konfiguracije. Žičani podaci jednostavno agent sustavom – agenta instalirati na računalo i on će nadzirati vlastitu mrežni promet. Druga prednost je kada želite nadzirati radnih opterećenja izvodi u oblak davatelji ili hostinga davatelj usluga ili Microsoft Azure koju korisnik ne vlasnik tkanina sloja.

Nasuprot tome, ne morate dovršeno vidljivost što se pojavljuje na mreži ako ne instalirate agenata na svim računalima mrežne infrastrukture.

## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja
Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

- Rješenje žičani podataka nabaviti podataka s računala sa sustavom Windows Server 2012 R2, Windows 8.1 i noviji operacijski sustavi.
- Na računalima na mjesto na koje želite dobiti žičani podatke iz potreban je Microsoft .NET Framework 4.0 ili noviji.
- Dodavanje rješenja žičani podataka u radni prostor OMS koristeći postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).  Postoji bez daljnje konfiguracije obavezno.
- Ako želite da biste vidjeli podatke žičani određene rješenja, morat ćete imati rješenje već dodali OMS radnog prostora.

## <a name="wire-data-data-collection-details"></a>Slika Detalji zbirke podataka podataka

Žičani podataka prikuplja metapodatke o mrežni promet pomoću agenata koji ste omogućili.

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za žičani podatke.


| platforme | Izravni Agent | Agent za SCOM | Prostor za pohranu za Azure | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Windows (2012 R2 / 8.1 ili noviji)|![Da](./media/log-analytics-wire-data/oms-bullet-green.png)|![Da](./media/log-analytics-wire-data/oms-bullet-green.png)|![ne](./media/log-analytics-wire-data/oms-bullet-red.png)|            ![ne](./media/log-analytics-wire-data/oms-bullet-red.png)|![ne](./media/log-analytics-wire-data/oms-bullet-red.png)| Svaki 1 min|


## <a name="combining-wire-data-with-other-solution-data"></a>Kombiniranje podataka žičani drugim podacima rješenja

Podatke dobivene ugrađene upitima gore navedenoj sintaksi može biti zanimljive samostalno. Međutim, upotrebljivost žičani podataka je Rač prilikom kombiniranja s podacima iz druge OMS rješenja. Ako, na primjer, možete koristiti sigurnosne događaj podatke prikupljene putem rješenja za sigurnost i nadzora i kombiniranje s podacima žičani da biste potražili neobično mreže pokušaja prijave za imenovani procesa.  U ovom se primjeru koristila u i DISTINCT operatori za pridruživanje točaka podataka u upit za pretraživanje.

Preduvjeti: Da biste koristili u sljedećem primjeru, morat ćete imati sigurnost i nadzora rješenje instaliran. Međutim, možete koristiti podatke iz druge rješenja za kombiniranje s žičani podataka da biste postigli slične rezultate.

### <a name="to-combine-wire-data-with-security-events"></a>Kombiniranje podataka žičani s sigurnost događajima

1. Na stranici pregled kliknite pločicu **WireData** .
2. Na popisu **Zajednički upiti WireData**kliknite **Iznos od mrežni promet (u bajtova) proces** da biste vidjeli popis vraćeni procesa.
    ![wiredata upita](./media/log-analytics-wire-data/oms-wiredata-01.png)
3. Ako je predugačak jednostavno prikazati popis procesa, možete izmijeniti upit za pretraživanje u oblik sličan:

    ```
    Type WireData | measure count() by ProcessName | where AggregatedValue <40
    ```
    Prikazano u primjeru u nastavku procesa pod nazivom DancingPigs.exe, možda će se pojaviti sumnjive.
    ![Rezultati pretraživanja wiredata](./media/log-analytics-wire-data/oms-wiredata-02.png)

4. Vraća pomoću podataka na popisu, kliknite imenovani procesa. U ovom primjeru DancingPigs.exe je kliknut. Rezultati prikazano u nastavku opisuju vrstu mrežnog prometa kao što su izlazne komunikacije putem protokola za različite.
    ![wiredata rezultata prikazuje imenovani procesa](./media/log-analytics-wire-data/oms-wiredata-03.png)

5. Jer je instalirana rješenja za sigurnost i nadzora možete isprobati u sigurnosne događaje koje imaju iste vrijednosti polja ProcessName izmjenom pomoću operatora upita pretraživanja u i DISTINCT upit za pretraživanje. To možete učiniti pa kad žičani podataka i drugih rješenja zapisnika vrijednosti u istom obliku. Izmjena upit za pretraživanje u oblik sličan:

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName}
    ```    

    ![wiredata rezultata prikazuje kombinirane podatke](./media/log-analytics-wire-data/oms-wiredata-04.png)
6. U rezultatima iznad, vidjet ćete da se prikazuju informacije o računu. Sada možete prilagoditi upit za pretraživanje da biste saznali koliko često je račun, prikazuje sigurnost i nadzora podataka koristio postupak s nalik upita:        

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName} | measure count() by Account
    ```

    ![wiredata rezultata prikazuje podataka računa](./media/log-analytics-wire-data/oms-wiredata-05.png)



## <a name="next-steps"></a>Daljnji koraci

- [Pretraživanje zapisnika](log-analytics-log-searches.md) radi prikaza žičani detaljne podatke pretraživanje zapisa.
- Informacije o tome Dan, [objavite pomoću žičani podataka u blog operacije upravljanja paket zapisnika pretraživanja](http://blogs.msdn.com/b/dmuscett/archive/2015/09/09/using-wire-data-in-operations-management-suite.aspx) ima dodatne informacije o učestalosti prikupljanja podataka i kako možete promijeniti svojstva zbirke za komponentu Operations Manager agenata.
