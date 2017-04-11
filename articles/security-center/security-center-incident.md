<properties
   pageTitle="Rukovanje incident vezan uz sigurnost u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pomaže vam da biste koristili mogućnosti centar za sigurnost Azure upravljanja sigurnošću."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="handling-security-incident-in-azure-security-center"></a>Rukovanje incident vezan uz sigurnost u centru za sigurnost Azure 
Triaging i istražuje sigurnosnih upozorenja može dosta dugo trajati za čak i u većinu stručne sigurnost analitičar analitičar podataka, a za mnoge teško je čak i Saznajte gdje da biste započeli. Pomoću [analize](security-center-detection-capabilities.md) povezivanje informacija između različitih [sigurnosnih upozorenja](security-center-managing-and-responding-alerts.md), centar za sigurnost može vam pružiti jednom prikazu programa kampanje napadi i svih povezanih upozorenja – možete brzo razumijevanje akcije koje traje napadača i resursima su negativno utjecati na značajku.

Ovaj dokument opisuje kako koristiti mogućnošću pretraživanja kroz razine sigurnosnih upozorenja u centru za sigurnost da bi vam rukovanje sigurnošću.


## <a name="what-is-a-security-incident"></a>Što je incident vezan uz sigurnost?

U centru za sigurnost incident vezan uz sigurnost je prikupljene sva upozorenja za resurs poravnavanje uzorcima [Ukloni lanac](https://blogs.technet.microsoft.com/office365security/addressing-your-cxos-top-five-cloud-security-concerns/) . Kupljenih se prikazivati na pločici [Sigurnosnih upozorenja](security-center-managing-and-responding-alerts.md) i plohu. Incident prikazuje popis povezanih upozorenja, koji omogućuje da biste dobili dodatne informacije o svako pojavljivanje.

## <a name="managing-security-incidents"></a>Upravljanje sigurnošću

Trenutni sigurnošću možete pregledati tako da pogledate pločicu sigurnosnih upozorenja. Pristup portalu Azure i slijedite korake u nastavku da biste vidjeli dodatne detalje o svakom incident vezan uz sigurnost:

1. Na nadzornoj ploči za centar za sigurnost, vidjet ćete pločicu **sigurnosnih upozorenja** .

    ![Pločica sigurnosnih upozorenja u centru za sigurnost](./media/security-center-incident/security-center-incident-fig1.png)

2.  Kliknite na toj pločici da biste proširili je te je li incident vezan uz sigurnost je otkriven, pojavit će se u odjeljku sigurnosnih upozorenja grafikona kao što je prikazano u nastavku:

    ![Incident vezan uz sigurnost](./media/security-center-incident/security-center-incident-fig2.png)

3.  Obratite pozornost na to da opis incidenta sigurnost ima različite ikonu u usporedbi s druga upozorenja. Kliknite da biste vidjeli dodatne detalje o ovom incident vezan uz.

    ![Incident vezan uz sigurnost](./media/security-center-incident/security-center-incident-fig3.png)

4.  Na **incident** plohu vidjet ćete više Detalji o ovom incident sigurnosti koji uključuje njegov puni opis, njegov težinu (što u tom slučaju je visoke), njegovu trenutnom stanju (u tom slučaju je i dalje *aktivna*, koji podrazumijeva da korisnik nije snimanja akcije da biste *otkazali* ga – to možete učiniti tako da desnom tipkom miša kliknete incident u plohu **sigurnosnih upozorenja** ) , attacked resursa (u ovom slučaju *VM1*) na olakšava koraka za incident, a u donjem oknu imate upozorenja uvrštene u ovom incident. Ako želite da biste dobili dodatne informacije o svakom upozorenju, samo pritisnite i drugi plohu otvorit će se, kao što je prikazano u nastavku:

    ![Incident vezan uz sigurnost](./media/security-center-incident/security-center-incident-fig4.png)

Informacije o ovom plohu razlikuju se prema upozorenje. Dodatne informacije o upravljanju te upozorenja pročitajte [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) . Neke važne stavke vezane uz tu mogućnost:

- Novi filtar omogućuje vam da biste prilagodili prikaz samo incident vezan uz, samo upozorenja ili i jedno i drugo. 
- Isti upozorenje može postojati u sklopu Incident (Ako je primjenjivo), kao i da budu vidljive kao samostalni upozorenja. 
- Zatvaranjem incident će odbacili povezane upozorenja.

## <a name="see-also"></a>Vidi također

U ovom dokumentu naučili kako koristiti mogućnost incidenta sigurnost u centru za sigurnost. Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md)
- [Centar za sigurnost Azure otkrivanje mogućnosti](security-center-detection-capabilities.md)
- [Centar za sigurnost Azure planiranja i vodič](security-center-planning-and-operations-guide.md)
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md)
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md)– traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/)– blog traženje članaka o Azure sigurnost i usklađenost
