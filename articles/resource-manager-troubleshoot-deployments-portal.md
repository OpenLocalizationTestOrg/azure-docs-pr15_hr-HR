<properties
   pageTitle="Prikaz postupci implementacije s portala | Microsoft Azure"
   description="U članku se opisuje kako pomoću portala za Azure da biste otkrili pogreške s resursima implementacije."
   services="azure-resource-manager,virtual-machines"
   documentationCenter=""
   tags="top-support-issue"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="infrastructure"
   ms.date="06/15/2016"
   ms.author="tomfitz"/>

# <a name="view-deployment-operations-with-azure-portal"></a>Prikaz postupci implementacije pomoću portala za Azure

> [AZURE.SELECTOR]
- [Portal](resource-manager-troubleshoot-deployments-portal.md)
- [PowerShell](resource-manager-troubleshoot-deployments-powershell.md)
- [Azure EŽA](resource-manager-troubleshoot-deployments-cli.md)
- [REST API-JA](resource-manager-troubleshoot-deployments-rest.md)

Možete pogledati postupci za implementaciju putem portala za Azure. Možda ćete zainteresirani operacije možete pregledavati i dok ste primili pogreške tijekom implementacije tako da u ovom članku fokus je na prikaz postupaka koji nije uspjela. Na portalu nudi sučelje koje omogućuje vam olakšava pronalaženje pogrešaka i određivanje potencijalnih rješenja.

[AZURE.INCLUDE [resource-manager-troubleshoot-introduction](../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="use-deployment-operations-to-troubleshoot"></a>Rješavanje problema pomoću postupci implementacije

Da biste vidjeli postupci implementacije, poduzmite sljedeće korake:

1. Za grupu resursa vezanih implementacije, imajte na umu status zadnje implementacije. Možete odabrati taj status da biste vidjeli dodatne detalje.

    ![status uvođenja](./media/resource-manager-troubleshoot-deployments-portal/deployment-status.png)

2. Vidjet ćete Nedavna povijest implementacije. Odaberite implementacije koja nije uspjelo.

    ![status uvođenja](./media/resource-manager-troubleshoot-deployments-portal/select-deployment.png)

3. Odaberite **nije uspjelo. Kliknite ovdje da biste detalje** da biste vidjeli zašto nije uspjela implementacijskih opis. Slike u nastavku, DNS zapis nije jedinstven.  

    ![Prikaz nije uspjelo implementacije](./media/resource-manager-troubleshoot-deployments-portal/view-error.png)

    Poruka o pogrešci mora biti dovoljno umjesto vas da biste započeli otklanjanje poteškoća. Međutim, ako je potrebno više detalja o su Dovršeni zadaci, možete pogledati operacije kao što je prikazano u sljedećim koracima.

4. Možete pregledati sve operacije implementacije u plohu **implementacije** . Odaberite sve operacije da biste vidjeli dodatne detalje.

    ![Prikaz postupaka](./media/resource-manager-troubleshoot-deployments-portal/view-operations.png)

    U ovom slučaju, vidjet ćete da račun za pohranu, virtualne mreže i postavljanje dostupnosti uspješno stvorena. Nije uspjelo na javnu IP adresu, a ostali resursi su pokušaj.

5. Događaji za implementaciju možete pogledati tako da odaberete **događaja**.

    ![Prikaz događaja](./media/resource-manager-troubleshoot-deployments-portal/view-events.png)

6. Pregled svih događaja za implementaciju i odaberite nešto više pojedinosti.

    ![u odjeljku događaji](./media/resource-manager-troubleshoot-deployments-portal/see-all-events.png)

## <a name="use-audit-logs-to-troubleshoot"></a>Korištenje zapisnika nadzora za otklanjanje poteškoća

[AZURE.INCLUDE [resource-manager-audit-limitations](../includes/resource-manager-audit-limitations.md)]

Da biste vidjeli pogreške za implementaciju, poduzmite sljedeće korake:

1. Prikaz zapisnika nadzora za grupu resursa tako da odaberete **Zapisnika nadzora**.

    ![Odaberite nadzora zapisnika](./media/resource-manager-troubleshoot-deployments-portal/select-audit-logs.png)

2. U plohu **Zapisnika nadzora** možete vidjeti sažetak nedavne operacije svih grupa resursa u svoju pretplatu. Obuhvaća grafički prikaz vremena i status operacije, kao i sve operacije.

    ![Prikaži akcije](./media/resource-manager-troubleshoot-deployments-portal/audit-summary.png)

3. Možete filtrirati prikaz zapisnika nadzora radi fokusiranja na određene uvjete. Odaberite **filtra** pri vrhu plohu **zapisnika nadzora** .

    ![Filtar zapisnika](./media/resource-manager-troubleshoot-deployments-portal/filter-logs.png)

4. Plohu **Filtar** odaberite uvjete da biste ograničili prikaz zapisnika nadzora samo operacije koje želite vidjeti. Na primjer, možete filtrirati operacije da ostanu prikazani samo pogrešaka za grupu resursa.

    ![Postavljanje mogućnosti filtra](./media/resource-manager-troubleshoot-deployments-portal/set-filter.png)

5. Dodatno možete filtrirati operacije postavljanjem vremenskog razdoblja. Sljedeća slika filtrira prikaza za određeni vremenski raspon 20 minuta.

    ![Postavi vrijeme](./media/resource-manager-troubleshoot-deployments-portal/select-time.png)

6. Na popisu možete odabrati neku od operacije. Odaberite postupak koji sadrži pogrešku koju želite istražiti.

    ![Odaberite postupak](./media/resource-manager-troubleshoot-deployments-portal/select-operation.png)
  
7. Prikazivat će se svi događaji za taj postupak. Obratite pozornost na to **ID korelacije** u sažetku. Taj ID koristi se za praćenje povezane događaja. Može biti korisno kada radite s tehničku podršku da biste riješili problem. Možete odabrati bilo koji događaj da biste vidjeli detalje o događaju.

    ![Odaberite događaj](./media/resource-manager-troubleshoot-deployments-portal/select-event.png)

8. Prikazat će se pojedinosti o događaju. Posebno obratite pozornost na **Svojstva** informacije o pogrešci.

    ![Prikaži detalje zapisnika nadzora](./media/resource-manager-troubleshoot-deployments-portal/audit-details.png)

Filtar primijenjen na zapisnik nadzora zadržava se sljedeći put, prikaz tako da morate promijeniti te se vrijednosti da biste proširili prikaz operacija.

## <a name="next-steps"></a>Daljnji koraci

- Pomoć za ispravljanje pogrešaka određeni implementaciju, potražite u članku [Riješite uobičajene pogreške prilikom implementacije resurse za Azure s Azure Voditelj resursa](resource-manager-common-deployment-errors.md).
- Dodatne informacije o korištenju zapisnika nadzora praćenje drugih vrsta akcije, potražite u članku [nadzora operacije s Voditelj resursa](resource-group-audit.md).
- Da biste provjerili valjanost uvođenja prije no što je izvršavanja, potražite u članku [uvođenja grupu resursa pomoću predloška Azure Voditelj resursa](resource-group-template-deploy.md).
