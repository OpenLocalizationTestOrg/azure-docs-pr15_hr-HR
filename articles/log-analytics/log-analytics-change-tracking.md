<properties
    pageTitle="Rješenja u prijava analitiku evidentiranja promjena | Microsoft Azure"
    description="Evidentiranje promjena u konfiguraciji rješenje možete koristiti u prijava analitiku da biste lakše prepoznali jednostavno softvera i servisa Windows promjene koji se pojavljuju u okruženju sustava – prepoznavanje te promjene konfiguracije omogućuju pinpoint radu problema."
    services="operations-management-suite"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="operations-management-suite"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="change-tracking-solution-in-log-analytics"></a>Promjena praćenja rješenja u zapisnik Analytics


Evidentiranje promjena u konfiguraciji rješenje možete koristiti u prijava analitiku da biste lakše prepoznali jednostavno softvera i servisa Windows promjene i promjene daemon Linux koji se pojavljuju u okruženju sustava – prepoznavanje te promjene konfiguracije omogućuju pinpoint radu problema.

Instalacija rješenja da biste ažurirali vrstu koji ste instalirali. Promjene da biste instalirali softver i Windows services na nadziranim poslužiteljima na kojima su za čitanje, a zatim podaci se šalju servisu prijava analitiku u oblaku za obradu. Logika primjenjuje se na primljene podataka i servisa u oblaku zapisa podataka. Kada se pronađu promjene, poslužitelje s promjenama prikazane su na nadzornoj ploči za evidentiranje promjena. Pomoću informacija na nadzornoj ploči za evidentiranje promjena, jednostavno možete vidjeti promjene izvršene u infrastruktura za server.

## <a name="installing-and-configuring-the-solution"></a>Instaliranje i konfiguriranje rješenja
Poslužite se sljedećim informacijama za instalaciju i konfiguriranje rješenja.

- Operations Manager potreban je za rješenja za evidentiranje promjena.
- Agent za Windows ili Operations Manager morate imati na svim računalima na mjesto na koje želite nadzirati promjene.
- Dodajte rješenje evidentiranje promjena u radni prostor OMS koristeći postupak opisan u [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).  Postoji bez daljnje konfiguracije obavezno.


## <a name="change-tracking-data-collection-details"></a>Promjena pojedinosti zbirke podataka za praćenje

Evidentiranje promjena prikuplja zaliha softvera i servisa Windows metapodataka pomoću agenata koji ste omogućili.

Sljedeća tablica prikazuje metode zbirke podataka i druge detalje o načinu prikupljanja podataka za evidentiranje promjena.

| platforme | Izravni Agent | Agent za SCOM | Prostor za pohranu za Azure | SCOM potrebne? | SCOM agent podataka šalju putem upravljanja grupe | Učestalost zbirke |
|---|---|---|---|---|---|---|
|Windows|![Da](./media/log-analytics-change-tracking/oms-bullet-green.png)|![Da](./media/log-analytics-change-tracking/oms-bullet-green.png)|![ne](./media/log-analytics-change-tracking/oms-bullet-red.png)|            ![ne](./media/log-analytics-change-tracking/oms-bullet-red.png)|![Da](./media/log-analytics-change-tracking/oms-bullet-green.png)| hourly|

## <a name="use-change-tracking"></a>Koristite evidentiranje promjena

Nakon instalacije rješenja za nadziranim poslužiteljima pomoću pločica **Evidentiranje promjena** na stranici **Pregled** u OMS možete pogledati sažetak promjene.

![Slika pločica evidentiranje promjena](./media/log-analytics-change-tracking/oms-changetracking-tile.png)

Možete pregledati promjene Infrastruktura i zatim dubinske analize u pojedinosti sljedećih kategorija:

- Promjene konfiguracije upišite za softver i usluge za Windows
- Softver se mijenja u aplikacije i ažuriranja za pojedinačne poslužitelje
- Ukupan broj promjena softver za svaku aplikaciju
- Promjene za pojedinačne poslužitelje servisa za Windows

![Slika zaslona evidentiranje promjena nadzorne ploče](./media/log-analytics-change-tracking/oms-changetracking01.png)

![Slika zaslona evidentiranje promjena nadzorne ploče](./media/log-analytics-change-tracking/oms-changetracking02.png)

### <a name="to-view-changes-for-any-change-type"></a>Da biste vidjeli promjene za bilo koju Promijeni vrstu

1. Na stranici **Pregled** kliknite pločicu **Evidentiranje promjena** .
2. Na **Promjena praćenja** nadzornu ploču, pregledali sažetak informacija u jednom od blades vrste promjena, a zatim jednu da biste vidjeli detaljne informacije o tome na stranici **zapisnika pretraživanja** .
3. Na bilo kojoj od stranica za pretraživanje zapisnika, možete pogledati rezultate prema vremenu, detaljne rezultate i povijesti zapisnika pretraživanja. Možete filtrirati i pozornici da biste suzili rezultate.

## <a name="next-steps"></a>Daljnji koraci

- Da biste pogledali detaljne promjene podataka za praćenje pomoću [zapisnika pretraživanja u zapisnik analize](log-analytics-log-searches.md) .
