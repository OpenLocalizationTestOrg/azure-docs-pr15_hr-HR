<properties
   pageTitle="Operacije upravljanja paketa sigurnosti i zaštite podataka rješenja nadzora | Microsoft Azure"
   description="Ovaj dokument objašnjava upravlja i safeguarded u operacije upravljanja paket sigurnost i rješenje nadzora podataka."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="yurid"/>

# <a name="operations-management-suite-security-and-audit-solution-data-security"></a>Postupci upravljanja paket sigurnost i nadzora sigurnost podataka rješenja

Da biste lakše kupci spriječiti, otkrivanje i odgovaranje na prijetnji, [Postupci upravljanja paket (OMS) sigurnost nadzora rješenje](operations-management-suite-overview.md) prikuplja i obrađuje podatke o resursima, što obuhvaća:

- Zapisnik događaja sigurnosti
- Događaji događaj praćenje za sustav Windows (ETW)
- AppLocker nadzora događaja
- Zapisnik Vatrozid za Windows
- Napredne analize prijetnju događaja
- Rezultati osnovne procjenu
- Rezultati antimalware procjenu
- Rezultati procjenu/zakrpu ažuriranja programa
- Syslogs tokova koji je izričito omogućeno na agenta

Dajemo istaknuti p za zaštitu privatnosti i sigurnosti te podatke. Microsoft se pridržava izričite smjernice za usklađenost i sigurnost – iz kodiranje za operacijski servisa.
U ovom se članku objašnjava upravlja i safeguarded u OMS sigurnost i rješenje nadzora podataka.

## <a name="data-sources"></a>Izvori podataka

Sigurnost OMS i rješenje nadzora analize podataka s virtualnim strojevima i fizičke računala na kojem je instaliran OMS Agent. OMS sigurnost i nadzor rješenja možete prikupiti podatke o konfiguraciji o događajima sigurnost, kao što su događaja sustava Windows, zapisnike nadzora, IIS zapisnika i syslog poruke. Primjeri tih podataka: vrsta operacijski sustav i verzija, pokretanje procesa, naziv računala, IP adresa prijavljeni korisnik i ID klijenta.  

## <a name="data-protection"></a>Zaštita podataka

**Segregation podataka**: podaci se drži logično zasebnom na svaku komponentu cijeloj servis. Svi podaci označen po tvrtke ili ustanove. Ovaj označavanje nastavi cijeloj životni ciklus podataka, a Poboljšana je svaki sloju servisa. 

**Pristup podacima**: za preporuke o sigurnosti i istražiti potencijalnih sigurnosnih rizika, Microsoftovo osoblje može pristupiti podacima koji se prikupljaju ili analizirati Services. Ne možemo drži za [Microsoft Online Services uvjete](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) i [Izjavu o zaštiti privatnosti](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), koji stanje da se Microsoft ne koristi podatke o klijentima ili proizlaziti informacije iz nje advertising ili slične komercijalne svrhe. Pružanje preporuke o sigurnosti i istražiti potencijalnih sigurnosnih rizika, Microsoft osoblja mogu pristupati podaci prikupljeni ili analizirati Services. Samo koristimo korisničkih podataka po potrebi da vam pruži Azure servisa, uključujući svrhe kompatibilan s pružanje tih servisa. Zadržava sve prava na vlastitim podacima.

**Korištenje podataka**: Microsoft koristi obrazaca i obavještavanje prijetnju vidjeti preko više klijenata za poboljšanje naših sprječavanje i otkrivanje mogućnosti; ne možemo učiniti skladu prakse za zaštitu privatnosti na što je opisano u našem [Izjava o zaštiti privatnosti](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

> [AZURE.NOTE] Mjesto podataka je konfiguriran na razini OMS radnog prostora, prilikom stvaranja radnog prostora koji je početna procesa konfiguracije OMS sigurnost i nadzora.

## <a name="see-also"></a>Vidi također

U ovom dokumentu naučili kako upravljati i safeguarded u OMS podataka. Da biste saznali više o sigurnosti OMS i nadzora rješenja, pogledajte:

- [Pregled operacije upravljanja paketa (OMS)](operations-management-suite-overview.md)
- [Praćenje i odgovarati na njih sigurnosnih upozorenja u operacije upravljanja paketa sigurnost i rješenje nadzora](oms-security-responding-alerts.md)
- [Nadzor resursa u operacije upravljanja paketa sigurnost i rješenje nadzora](oms-security-monitoring-resources.md)

