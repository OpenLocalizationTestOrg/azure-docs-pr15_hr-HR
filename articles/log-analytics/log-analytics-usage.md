<properties
    pageTitle="Analize korištenja podataka u prijava analitiku | Microsoft Azure"
    description="Korištenje stranice u prijava analitiku možete koristiti da biste pogledali koliko je podataka koje se šalju OMS usluga."
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
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="analyze-data-usage-in-log-analytics"></a>Analize korištenja podataka u zapisniku analize

Zapisnik analize u na operacije upravljanja paket (OMS) prikuplja podatke i redovito ga šalje OMS usluga.  **Korištenje** stranica možete koristiti da biste pogledali koliko je podataka koje se šalju OMS usluga. **Korištenje** stranica i pokazuje koliko je podataka koje se šalju svakodnevno po rješenja i koliko često vaši poslužitelji šalju podataka.

>[AZURE.NOTE] Ako imate besplatnu račun stvoren pomoću sustava [OMS web-mjesta](http://www.microsoft.com/oms), ste ograničeni svakodnevno slanju 500 MB podataka OMS usluga. Ako ne dođete do dnevno ograničenje, analiza podataka će prestati i nastaviti na početku sljedeći dan. Ćete morati ponovno sve podatke koje nije prihvaćen ili obradili OMS.

Upotreba možete pogledati pomoću pločicu za **Korištenje** na nadzornoj ploči za **Pregled** u OMS.

![Korištenje pločica](./media/log-analytics-usage/usage-tile.png)

Ako koju ste premašili dnevnih korištenje ili ako ste uz ograničenje, po želji možete ukloniti rješenje da biste smanjili količinu podataka koje šaljete OMS usluga. Dodatne informacije o uklanjanju rješenja potražite u članku [Dodavanje analize zapisnika rješenja iz galerije rješenja](log-analytics-add-solutions.md).

![Korištenje nadzorne ploče](./media/log-analytics-usage/usage-dashboard.png)

Na stranici **Korištenje** prikazuju se sljedeće informacije:

- Prosječna korištenje po danu
- Podatkovni promet za svakog rješenja tijekom zadnjih 30 dana
- Koliko podataka poslužitelji u svom okruženju šaljete OMS usluga tijekom zadnjih 30 dana
- Plan podataka cijene sloju i Procijenjena trošak
- Informacije o vašem razine ugovor o usluzi (SLA), uključujući koliko dugo traje OMS za obradu podataka

## <a name="to-work-with-usage-data"></a>Rad s podacima za korištenje

1. Na stranici **Pregled** kliknite pločicu **Korištenje** .
2. Na stranici **Korištenje** prikaz kategorija korištenje koji pokazuju područja ste zabrinuti.
3. Ako imate rješenje koje koristi previše dnevnih kvota za prijenos, razmislite o uklanjanju tog rješenja.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>Da biste pogledali podatke o naplati i Procijenjena trošak
1. Na stranici **Pregled** kliknite pločicu **Korištenje** .
2. Na stranici za **Korištenje** u odjeljku **Korištenje**kliknite ševron (**>**) pokraj **Procijenjena trošak**.
3. U na proširenom **podatkovne tarife** detalje, možete vidjeti vaše Procijenjena mjesečno cijena.  
    ![Podatkovne tarife](./media/log-analytics-usage/usage-data-plan.png)
4. Ako želite da biste pogledali podatke o naplati, kliknite **Prikaz Moje fakture** za prikaz informacija o pretplati.
    - Na stranici pretplate kliknite pretplatu da biste pogledali detalje i popis stavke retka korištenje.  
        ![pretplate](./media/log-analytics-usage/usage-sub01.png)
    - Na stranici sažetka za vašu pretplatu možete izvršiti različitih zadataka za prikaz više pojedinosti o pretplati i upravljanje.  
        ![Detalji o pretplati](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>Da biste pogledali serije podataka za vaše SLA
1. Na stranici **Pregled** kliknite pločicu **Korištenje** .
2. U odjeljku **Ugovor o razini usluge**kliknite **Preuzimanje SLA pojedinosti**.
3. Datoteke programa Excel XLSX da pregledate je preuzeti.  
    ![Detalji o SLA](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Daljnji koraci

- Potražite u [zapisniku pretraživanja u zapisnik analize](log-analytics-log-searches.md) da biste pogledali detaljne podatke prikupljene putem rješenja.
