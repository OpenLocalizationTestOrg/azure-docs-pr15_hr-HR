<properties
   pageTitle="Istječe podataka u DocumentDB s vremenom Live | Microsoft Azure"
   description="S TTL, Microsoft Azure DocumentDB omogućuje da bi se dokumenti automatski očišćene iz sustava nakon određenog vremena."
   services="documentdb"
   documentationCenter=""
   keywords="vrijeme Live"
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="04/28/2016"
   ms.author="kipandya"/>

# <a name="expire-data-in-documentdb-collections-automatically-with-time-to-live"></a>Istječe podataka u DocumentDB zbirke automatski s vremenom Live

Aplikacije možete proizvesti i pohraniti veliku količine podataka. Neke od tih podataka, kao što su strojno generirani događaj podataka, zapisnika i korisnik sesije informacije koristan je samo za konačne vremensko razdoblje. Kada se podaci postaju viška potrebama aplikacija je sigurno Pročistite ove podatke i smanjiti količinu prostora za pohranu potrebama aplikacije.

"Vrijeme Live" ili TTL, Microsoft Azure DocumentDB omogućuje da bi se dokumenti automatski očišćene iz baze podataka nakon određenog vremena. Zadano vrijeme Live možete postaviti na razini zbirke i nadjačati na temelju po dokumentu. Nakon TTL postavljanja kao zadanu zbirke ili na razini dokumenta, DocumentDB automatski ukloniti dokumente koje postoje nakon tog vremena u sekundama nakon zadnje izmjene.

Vrijeme Live u DocumentDB koristi se pomak od kada zadnji mijenjao dokument. Da biste to učinili koristi polje _ts koji postoji na svaki dokument. Polje _ts je vremenskog pečata epoch unix stil koji predstavlja datum i vrijeme. Polje _ts ažurira se svaki put kada dokument se mijenja. 

## <a name="ttl-behavior"></a>TTL ponašanje

Značajka TTL upravlja TTL svojstva na dvije razine - razini zbirke i razinu dokumenta. Vrijednosti se postavljaju u sekundama i tretira se kao na delta iz _ts koji je vrijeme zadnje izmjene dokumenta.

 1.  DefaultTTL zbirke
  * Ako nema (ili postavljena na null) dokumenti se automatski brišu.
  
  * Ako je izlaganje i vrijednost "-1" = beskonačno – dokumenti ne isteknu prema zadanim postavkama
  
  * Ako prezentacija i vrijednost je neki broj ("n") – dokumenata istječe "n" s nakon zadnje izmjene

 2.  TTL za dokumente: 
  * Svojstvo se može primijeniti samo ako se DefaultTTL nadređenog zbirke.
  
  * Zanemaruje vrijednosti DefaultTTL nadređenoj zbirci.

Čim dokument istekla (ttl + _ts > = trenutno vrijeme server), dokument označen kao "istekle". Nijedna operacija dozvoljen na te dokumente nakon ovaj put, a zatim će se izuzeti iz rezultata eventualne upite koji se izvode. Dokumenti fizički izbrisan u sustavu i brišu se u pozadini opportunistically kasnije. To zauzimaju sve [Zahtjev jedinice (RUs)](documentdb-request-units.md) iz zbirke proračuna.

Iznad logike moguće je prikazati u matrici sljedeće:

|       | DefaultTTL nedostaje ili nije postavljena na zbirci | DefaultTTL = -1 na zbirke | DefaultTTL = "n" u zbirci|
| ------------- |:-------------|:-------------|:-------------|
| TTL nedostaje na dokumentu| Ništa da biste nadjačali na razini dokumenta jer dokument i zbirka ste bez pojam TTL. | Nema dokumenata u ovoj zbirci će isteći. | Kada je interval n minutama, dokumenata u ovoj zbirci će isteći. |
| TTL = -1 na dokumentu | Ništa da biste nadjačali na razini dokumenta od zbirke ne definirati svojstvo DefaultTTL koje možete nadjačati dokumenta. TTL na dokumentu je nepotvrđen interpreted sustav. | Nema dokumenata u ovoj zbirci će isteći. | Dokument s TTL =-1 u ovoj zbirci nikada ne istječe. Svi ostali dokumenti će isteći nakon interval "n". |
|  TTL = n na dokumentu | Ništa da biste nadjačali na razini dokumenta. TTL na dokumentu u nepotvrđen interpreted sustav. | Dokument s TTL = n će isteći nakon interval n, u sekundama. Drugi dokumenti će nasljeđuju interval -1 i nikada ne istječe. | Dokument s TTL = n će isteći nakon interval n, u sekundama. Drugi dokumenti će naslijediti interval "n" iz zbirke. |


## <a name="configuring-ttl"></a>Konfiguriranje TTL

Prema zadanim postavkama, vrijeme Live onemogućeno je po zadanom u svim zbirkama DocumentDB i na svim dokumentima.

## <a name="enabling-ttl"></a>Omogućivanje TTL

Da biste omogućili TTL zbirka ili dokumenata u zbirci, morate postavite svojstvo DefaultTTL zbirke web -1 ili pozitivan broj od nule. Postavka u DefaultTTL za srednje vrijednosti-1 koji po zadanom svim dokumentima u zbirci će zauvijek live, ali u DocumentDB servisa treba pratiti tu zbirku u dokumentima koje ste nadjačati to je zadana vrijednost.

## <a name="configuring-default-ttl-on-a-collection"></a>Konfiguriranje zadanog TTL u zbirci web

Prikazuje vam se da biste konfigurirali zadano vrijeme Live na razini zbirke. 

Da biste postavili na TTL u zbirci web, morate unijeti od nule pozitivni broj koji označava razdoblje u sekundama, istek svim dokumentima u zbirci nakon zadnje izmjene vremenske oznake u dokumentu (_ts).

Ili možete postaviti zadano-1, podrazumijeva da sve dokumente koji se umeće u zbirci će live beskonačno prema zadanim postavkama.

## <a name="setting-ttl-on-a-document"></a>Postavka TTL na dokumentu

Uz postavku zadani TTL u zbirci web možete postaviti određenu TTL na razini dokumenta. Na taj način nadjačat će zadane zbirke.

Da biste postavili na TTL na dokumentu, morate unijeti od nule pozitivni broj koji označava razdoblje u sekundama, istek dokument nakon zadnje izmjene vremenske oznake u dokumentu (_ts).

Da biste postavili ovaj pomak isteka, polje TTL mora biti postavljeno na dokumentu.

Ako dokument ima polje ne TTL, primijenit će zadane zbirke.

Ako na razini zbirke je onemogućen TTL, polje TTL na dokumentu će zanemariti sve dok ponovno omogućiti TTL u zbirci.


## <a name="extending-ttl-on-an-existing-document"></a>Proširivanje TTL na postojećem dokumentu

TTL na dokumentu možete ponovno postaviti tako da učinite sve operacije pisanje na dokumentu. Time će se postaviti u _ts trenutno vrijeme, a odbrojavanjem do isteka dokumenta, kao što je postavljeno tako da ttl, ponovno će početi.

Ako želite promijeniti ttl dokumenta, kao što možete učiniti s bilo kojim drugim moguće postaviti poljem možete ažurirati polja.


## <a name="removing-ttl-from-a-document"></a>Uklanjanje TTL iz dokumenta

Ako je TTL postavljena na dokumentu, a više ne želite da taj dokument istječe, pa možete dohvatiti dokument, uklonite polje TTL i zamijeniti dokument na poslužitelju.

Kada TTL polje se uklanja iz dokumenta, primijenit će se zadana zbirke.

Da biste prekinuli dokumenta od istječe i ne nasljeđuju iz zbirke pa ćete morati postavite TTL vrijednost -1.


## <a name="disabling-ttl"></a>Onemogućivanje TTL

Da biste onemogućili TTL, u zbirci web i zaustavite proces pozadine iz tražite istekle dokumente svojstvo DefaultTTL u zbirci treba izbrisati.

Brisanje ovo svojstvo razlikuje se od postavljanje-1. Postavka za nove dokumente sredstvo-1 dodan u zbirku će live zauvijek, ali to možete nadjačati na određene dokumente u zbirci.

Uklanjanje to svojstvo, iz zbirke, to znači da nema dokumenata će isteći, čak i ako postoje dokumenti koje ste izričito nadjačati prethodne zadani.


## <a name="faq"></a>NAJČEŠĆA PITANJA

**Što TTL košta mene?**

Postoji bez dodatnih troškova postavci na TTL na dokumentu.

**Koliko će trajati da biste izbrisali dokument nakon što u TTL gore?**

Dokumenti su označene kao nedostupan čim dokument istekla (ttl + _ts > = trenutno vrijeme server). Nijedna operacija dozvoljen na te dokumente nakon ovaj put, a zatim će se izuzeti iz rezultata eventualne upite koji se izvode. Dokumenti fizički brišu se sustav u pozadini. To će zauzimaju sve RUs iz proračuna za zbirku.

**Ako je potrebno neko vrijeme da biste izbrisali dokumente, oni Brojat će prema Moje kvote (i računa) sve dok se ne mogu se izbrisati?**

Ne, kada dokument istekla vam neće naplaćivati za pohranu te dokumente i veličine dokumenata se ne odnosi prema kvotu za pohranu za zbirku.

**TTL na dokumentu imat će utjecaja na Pravi troškove?**

Ne, pojavit će se bez utjecaja na Pravi naknade za razne operacije obavljene na bilo kojem dokumentu unutar DocumentDB.

**Će brisanje dokumenata utjecaj na propusnost koje ste dodjeli Moje zbirci?**

Ne, posluživanje zahtjeve protiv zbirci web primit će prioritet putem izvođenje postupka pozadine da biste izbrisali dokumenata. Dodavanje TTL u bilo kojem dokumentu neće utjecati to.

**Kada dokumentu istekne, koliko će se ostati u Moje zbirke do brisanja?**

Čim dokument istekne neće više biti dostupne. Točno vrijeme dokumenta će ostati u sklopu zbirke prije nego što zapravo brisanja je koji nisu deterministic i će se temeljiti na kada se mogućnost da biste izbrisali dokument postupak pozadine.

**Istekle dokumente brišu preko sve čvorove ili je "naposljetku consistent?"**

Dokument više neće biti dostupna u isto vrijeme duž sve čvorove i u svim regijama.

**Je li trošak Pravi za TTL nadzor pozadinske zadatke?**

Ne postoji bez Pravi trošak za to.

**Koliko često se provjeravaju TTL expirations?**

Provjera TTL expirations se ne događa kao postupka u pozadini. Prilikom odgovaranja na zahtjev za uslugu pozadinskog će nemojte umetnute provjere i isključili svi dokumenti koja je istekla. Brisanje fizičke dokument je samo postupak koji je asinkrono pokrenut u pozadini. Učestalost postupak određen je dostupna RUs u zbirci.

**Ne značajku TTL odnose samo na cijelu dokumenata ili li istječe vrijednosti svojstava za dokumente?**

TTL primjenjuje se na cijeli dokument. Ako želite istječe samo dio dokumenta, zatim preporučuje se da izdvajanje dijela iz glavnog dokumenta u zasebnim dokumentom "povezane", a zatim koristite TTL na njemu izdvojene.

**Mora li značajku TTL sve indeksiranja preduvjete?**

Da. Zbirka mora imati [Indeksiranje pravila postavljanje](documentdb-indexing-policies.md) Lazy ili jednak. Pokušaja postavljanja DefaultTTL zbirci s indeksiranje skup ništa uzrokovat će pogrešku, kao i pokušate da biste isključili indeksiranje u zbirci web DefaultTTL već postavio.


## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o Azure DocumentDB, pročitajte [*dokumentaciju*](https://azure.microsoft.com/documentation/services/documentdb/) stranici servisa.




