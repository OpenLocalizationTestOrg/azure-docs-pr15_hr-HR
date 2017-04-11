<properties
    pageTitle="Dodavanje kašnjenje u aplikacijama logike | Microsoft Azure"
    description="Pregled odgode i odgode-do akcije i kako ih koristiti aplikaciju Azure logike."
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/18/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Početak rada s odgode i odgode-do akcije

Pomoću odgode i "odgode-dok" Akcije, dovršite scenarija tijeka rada.

Na primjer, možete:

- Pričekajte dok se ne weekday poslati ažuriranja statusa putem e-pošte.
- Odgađanje tijek rada dok poziv HTTP je vrijeme da biste dovršili prije nastavljanje i dohvaćanje rezultata.

Da biste počeli koristiti odgode akcija u aplikaciji logike, potražite u članku [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>Korištenje akcija odgode

Akciju je postupak koji se provodi tako da se tijek rada koji je definiran u aplikaciji logike. [Dodatne informacije o akcije](connectors-overview.md).

Evo primjera slijeda kako koristiti odgode korak u aplikaciji logika:

1. Kad dodate okidač, kliknite **Novi korak** da biste dodali akciju.
2. Traženje **odgode** da bi se prikazala odgode akcije. U ovom primjeru smo ćete odabrati **odgode**.

    ![Odgoda akcije](./media/connectors-native-delay/using-action-1.png)

3. Ispunite bilo koje od svojstava akcije da biste konfigurirali kašnjenja.

    ![Odgoda config](./media/connectors-native-delay/using-action-2.png)

4. Kliknite **Spremi** da biste objavili i aktivirati aplikaciju logike.


## <a name="action-details"></a>Detalji o akcija

Ponavljanje okidača ima sljedeća svojstva koja se može konfigurirati.

### <a name="delay-action"></a>Odgoda akcija

Ova akcija odgađa Pokreni za određeni vremenski interval.
A * znači da je obavezno polje.

|Zaslonsko ime|Naziv svojstva|Opis|
|---|---|---|
|Count *|Count|Broj vremenskih jedinica da biste odgodili početak|
|Jedinica *|jedinica|Jedinica vremena: `Second`, `Minute`, `Hour`, ili`Day`|
<br>

### <a name="delay-until-action"></a>Odgoda-do akcija

Ova akcija odgađa Pokreni do određenog datuma/vremena.
A * znači da je obavezno polje.

|Zaslonsko ime|Naziv svojstva|Opis|
|---|---|---|
|Godine *|Vremenska oznaka|Godina da biste odgodili do (GMT)|
|Mjesec *|Vremenska oznaka|Mjesec da biste odgodili do (GMT)|
|Dan *|Vremenska oznaka|Dan da biste odgodili do (GMT)|
<br>


## <a name="next-steps"></a>Daljnji koraci

Sada, isprobajte platforme i [Stvaranje logike aplikacije](../app-service-logic/app-service-logic-create-a-logic-app.md). Druge dostupne poveznika u aplikacijama logike možete istražiti tako da pogledate našem [popisu API-ji](apis-list.md).
