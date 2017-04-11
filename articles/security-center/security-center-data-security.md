<properties
   pageTitle="Centar za sigurnost Azure sigurnost podataka | Microsoft Azure"
   description="Ovaj dokument objašnjava kako upravljati i safeguarded u centru za sigurnost Azure podataka."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-data-security"></a>Centar za sigurnost Azure sigurnost podataka
Da biste lakše kupci spriječiti, otkrivanje i odgovoriti prijetnji, centar za sigurnost Azure prikuplja i obrađuje podatke o Azure resursa, uključujući informacije o konfiguraciji, metapodataka i zapisnike događaja neočekivano zatvoriti ispis datoteka i drugo. Dajemo istaknuti p za zaštitu privatnosti i sigurnosti te podatke. Microsoft se pridržava izričite smjernice za usklađenost i sigurnost – iz kodiranje za operacijski servisa. 

U ovom se članku objašnjava kako upravljati i safeguarded u centru za sigurnost Azure podataka.

## <a name="data-sources"></a>Izvori podataka

Centar za sigurnost Azure analiziraju podaci iz sljedećih izvora:

- Servisi za Azure: Čita informacije o konfiguraciji servisa Azure uveli po komunikaciju s davatelja resursa za tu uslugu.
- Mrežni promet: Čitanja uzorkovanja mrežni promet metapodataka iz tvrtke Microsoft infrastrukture, kao što je izvor odredište IP/priključak veličinu paketa i mrežnih protokola.
- Rješenja partnera: Prikuplja sigurnosnih upozorenja iz integrirani partnerskih rješenja, kao što su vatrozida i antimalware rješenja. Ti podaci se pohranjuju u Centar za sigurnost Azure prostor za pohranu, koja se trenutno nalazi u Sjedinjenim Američkim Državama.
- Virtualnim strojevima: Centar za sigurnost Azure možete prikupiti podatke o konfiguraciji i informacije o događajima sigurnost, kao što su događaja sustava Windows i nadzor zapisnike, IIS zapisnike, syslog poruke i pad ispis datoteka s virtualnim računalima sustava pomoću agenata zbirke podataka. U odjeljku "Upravljanje prikupljanje podataka" ispod dodatne detalje.  

Uz to, informacije o sigurnosnih upozorenja, preporuke i sigurnosnog stanja stanja pohranjuju se u Centar za sigurnost Azure prostor za pohranu, koja se trenutno nalazi u Sjedinjenim Američkim Državama. Ove informacije može obuhvaćati podatke o konfiguraciji povezani i sigurnosne događaje prikupljene od virtualnim strojevima prema potrebi da vam pruži sigurnosno upozorenje, preporuke ili sigurnosnog stanja stanja.

## <a name="data-protection"></a>Zaštita podataka

**Segregation podataka**: podaci se drži logično zasebnom na svaku komponentu cijeloj servis. Svi podaci označen po tvrtke ili ustanove. Ovaj označavanje nastavi cijeloj životni ciklus podataka, a Poboljšana je svaki sloju servisa. Osim toga, podaci prikupljeni s virtualnim strojevima pohranjuju se u svoje račune za pohranu.

**Pristup podacima**: da biste mogli unijeti preporuke o sigurnosti i istražiti potencijalnih sigurnosnih rizika, Microsoftovo osoblje može pristupiti podacima koji se prikupljaju ili analizirati Azure servisima, uključujući datoteke ispisa pad sustava. Datoteke ispisa pad sustava i procesa stvaranja događaja slučajno može obuhvaćati podatke o klijentu i osobnih podataka iz virtualnih računala. Ne možemo drži za [Microsoft Online Services uvjete](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) i [Izjavu o zaštiti privatnosti](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx), koji stanje da se Microsoft ne koristi podatke o klijentima ili proizlaziti informacije iz nje advertising ili slične komercijalne svrhe. Samo koristimo korisničkih podataka po potrebi da vam pruži Azure servisa, uključujući svrhe kompatibilan s pružanje tih servisa. Zadržava sve prava za podatke o klijentu.

**Korištenje podataka**: Microsoft koristi obrazaca i obavještavanje prijetnju vidjeti preko više klijenata za poboljšanje naših sprječavanje i otkrivanje mogućnosti; ne možemo učiniti skladu prakse za zaštitu privatnosti na što je opisano u našem [Izjava o zaštiti privatnosti](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

**Mjesto podataka**: prostora za pohranu računa navedena za svako područje u kojem se pokreće virtualnim računalima sustava. Omogućuje spremanje podataka u istom području kao virtualnog računala s kojeg prikupljanja podataka. Ovaj podataka, uključujući datoteke ispisa rušenje, će se persistently pohraniti na vašem računu za pohranu. Servis pohranjuje i informacije o sigurnosnih upozorenja, uključujući upozorenja s Integriranom partnerskih rješenja, preporuke i sigurnosnog stanja stanja u centru za sigurnost Azure prostor za pohranu, koja se trenutno nalazi u Sjedinjenim Američkim Državama.

## <a name="managing-data-collection-from-virtual-machines"></a>Upravljanje prikupljanje podataka iz virtualnim strojevima

Kad odaberete da biste omogućili Azure centar za sigurnost, prikupljanje podataka uključeno za sve pretplate. Možete isključiti prikupljanje podataka u odjeljku "Sigurnosna pravila" nadzornu ploču centra sigurnosti Azure. Kada je uključena mogućnost prikupljanje podataka, centar za sigurnost Azure dodjeljuje resurse Azure nadzor agenta na sve postojeće podržane virtualnim strojevima i nove, koje su stvorene. Proširenje Azure sigurnost nadzor skenira za različite sigurnost povezane konfiguraciju i događaje u [Događaj praćenje za Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) (ETW) prati. Nadalje, operacijski sustav će podići događaje zapisnika događaja tijekom trajanja pokrenut na računalu. Primjeri tih podataka: vrsta operacijski sustav i verzija operacijskog sustava zapisnika (Windows zapisnika događaja,) pokretanje procesa, naziv računala, IP adresa prijavljeni korisnik i ID klijenta. Agent za nadzor Azure čita unose u zapisnik događaja i ETW prati i kopira ih na račun za pohranu za analizu. 

Prostor za pohranu računa navedena je za svaku regiju u kojoj ćete imati virtualnim strojevima pokrenut, gdje podataka koji se prikupljaju s virtualnim strojevima koji je pohranjen istom području. To olakšava zadržati podatke u istom zemljopisnom području izjave o zaštiti privatnosti i svrhe samostalnosti podataka. U odjeljku "Sigurnosna pravila" nadzornu ploču centra sigurnosti Azure možete konfigurirati račune za pohranu za svako područje.

Agent za nadzor Azure kopira i pad ispisa datoteke na račun servisa za pohranu.  Centar za sigurnost Azure prikuplja efemerni kopija rušenje ispis datoteka i analiziraju ih za dokaz pokušaja zlonamjerni program i uspješno kompromisa.  Centar za sigurnost Azure izvodi ovu analizu unutar iste regiji kao račun za pohranu, a briše efemerni kopija nakon dovršetka analize.

Prikupljanje podataka iz virtualnim strojevima u bilo kojem trenutku, koji će ukloniti sve nadzor agenata prethodno instalirao centar za sigurnost Azure možete onemogućiti.


## <a name="see-also"></a>Vidi također

U ovom dokumentu naučili kako upravljati i safeguarded u centru za sigurnost Azure podataka. Da biste saznali više o centru za sigurnost Azure, pogledajte:

- [Azure sigurnost centar za planiranje i vodič](security-center-planning-and-operations-guide.md) – upute za planiranje i razumijevanje zahtjevi dizajna prihvaćaju Azure centar za sigurnost.
- [Sigurnost stanja nadzor u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resursi
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost Azure](security-center-faq.md) – traženje najčešća pitanja o korištenju servisa
- [Azure sigurnost Blog](http://blogs.msdn.com/b/azuresecurity/) – pronaći bloga o Azure sigurnost i usklađenost
