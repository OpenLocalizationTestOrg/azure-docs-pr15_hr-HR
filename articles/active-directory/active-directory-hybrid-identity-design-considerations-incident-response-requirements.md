
<properties
    pageTitle="Azure Active Directory hibridnog identiteta zahtjevi dizajna - procjeni uvjeta incidenta rResponse | Preduvjeti za Microsoft Azure "
    description="Određivanje mogućnosti nadzora i izvješćivanja za rješenje identiteta hibridnog koje možete leveraged tako da IT za izvođenje akcija za prepoznavanje i prevladavanje potencijalnih prijetnji"
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>Određivanje incidenta odgovor preduvjeti za hibridno rješenje identiteta

Velike i srednje tvrtke ili ustanove vjerojatno će imati [Sigurnost incidenta odgovor](https://technet.microsoft.com/library/cc700825.aspx) na mjestu da biste lakše IT poduzeti sukladno tome razinu incident. Sustav upravljanja identiteta je važno komponenta u postupku incidenta odgovora jer se može koristiti za označavanje koji je obavio određenu akciju u odnosu na cilj. Rješenje identiteta hibridnog moraju imati mogućnost za nadzor i izvješćivanja mogućnosti koje možete leveraged po IT za izvođenje akcija za prepoznavanje i prevladavanje potencijalne prijetnju. U standardne incidenta odgovor tarifi će imati sljedeće faze kao dio plan:

1.  Početna rizika.
2.  Incidenta komunikacije.
3.  Kontrola oštećenja i smanjenje rizika.
4.  Identifikacijski koje je narušena pouzdanost i težinu.
5.  Dokaz čuvanju.
6.  Obavijest o odgovarajuće stranama.
7.  Oporavak sustava.
8.  Dokumentacija.
9.  Procjena oštećenja i cijene.
10. Postupak i planiranje revizije.

Tijekom identifikacijski koje it je narušena pouzdanost i težinu faza, bit će potrebno da biste odredili sustavima koji imaju omogućeno ugrožena, datoteke koje ste pristupili i odrediti osjetljivost te datoteke. Identitet sustava hibridnog trebali biste moći ispunjavanje preduvjeta kako bi vam prepoznavanje korisnika koji ste napravili te promjene. 

## <a name="monitoring-and-reporting"></a>Nadzor i izvješćivanje o pogreškama
Više puta identitet sustava mogu pomoći u fazi početne procjenu uglavnom ako sustav sadrži ugrađena nadziranje i izvješćivanje o mogućnostima. Tijekom početne procjenu administrator moraju imati mogućnost da biste odredili sumnjivoj aktivnosti ili sustav trebali biste moći okidača automatski utemeljenog na unaprijed konfigurirana zadatka. Mnoge aktivnosti ukazuje mogućih napada, no u drugim slučajevima ozbiljno konfiguriranog sustava mogu dovesti do broj false positives u sustavu otkrivanje podataka. 

Sustav upravljanja identiteta trebali biste pomoći IT administratorima za prepoznavanje i stvaranje izvješća te sumnjive aktivnosti. Obično preduvjeta tehničke možete ispuniti nadzor svi sustavi i pritom izvješćivanja mogućnost koje možete istaknuti potencijalnih prijetnji. Koristite pitanja u nastavku da biste lakše dizajniranje rješenje identiteta hibridnog tijekom snimanja u obzir incidenta odgovor preduvjeti:

- Sadrži li vaša tvrtka ima sigurnost incidenta odgovor na mjestu?
 - Ako je da, trenutni sustav upravljanja identitet koristi se kao dio postupka?
- Vaša tvrtka mora prepoznavanje sumnjiva prijave pokušaja od korisnika preko različitih uređaja?
- Vaša tvrtka mora da biste otkrili potencijalne ugrožena korisnikove vjerodajnice?
- Vaša tvrtka mora nadzor pristupa i akcije korisnika?
- Ne tvrtke morate znati kada korisnik Vrati svoju lozinku?

## <a name="policy-enforcement"></a>Uvođenje pravila

Tijekom oštećenja kontrola i smanjenja faza rizika, važno je da brzo smanjiti efekti stvarni i potencijalne napada. Koje će poduzeti akciju sada možete napraviti razlika između programa pomoćne i glavne jedan. Točan odgovor će ovise o vašoj tvrtki ili ustanovi i prirode napada koji će vam biti namijenjeno. Ako početne procjenu završena je li ugrožena računa, morat ćete Nametni pravila da biste blokirali ovog računa. To je samo jedan primjer gdje će leveraged sustav upravljanja identitet. Koristite pitanja u nastavku da biste lakše dizajniranje rješenje identiteta hibridnog tijekom uzimajući u obzir kako pravila provest će se da biste brzo incident tijeku:

- Vaša tvrtka mora li pravila na mjestu korisnicima onemogućite iz programa access s mrežom Ako je potrebno?
 - Ako je da, ne trenutnog rješenja integrirati s sustav upravljanja hibridnog identitet koji ćete prihvaćaju?
- Vaša tvrtka mora da biste nametnuli uvjetnog pristupa za korisnike koji su u karantene? 
 
>[AZURE.NOTE]
Provjerite je li bilješke svaki odgovor i razumijevanje rationale iza odgovor. [Zaštita Strategije za definiranje podataka](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) će ići preko mogućnosti dostupne i prednosti i nedostatke svake mogućnosti.  Tako da imate odgovoriti na ta pitanja ćete odabrati koju mogućnost najbolje odgovara vašem poslovanju mora.

## <a name="next-steps"></a>Daljnji koraci
[Definiranje Strategije za zaštitu podataka](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)
