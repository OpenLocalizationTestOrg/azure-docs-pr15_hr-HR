<properties
    pageTitle="Azure Active Directory hibridnog identiteta zahtjevi dizajna - procjeni uvjeta višestruka provjera autentičnosti"
    description="S kontrolom uvjetno pristup Azure Active Directory provjerava određenim uvjetima koje ste odabrali prilikom provjere autentičnosti korisnika i prije omogućivanja pristupa aplikaciji. Kada su ti uvjeti zadovoljeni, korisnik je autentičnost provjerena i dopuštenje za pristup aplikaciji."
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>Određivanje višestruka provjera autentičnosti preduvjeti za hibridno rješenje identiteta

U ovom svijetu mobilnost s korisnicima pristupa podacima i aplikacijama u oblak i s bilo kojeg uređaja zaštita taj podatak postala paramount.  Svakodnevno postoji novi naslov o sigurnosti kršenja.  Iako nema jamstva na temelju takve breaches, višestruke provjere autentičnosti pruža dodatne sloju sigurnost da biste spriječili te breaches.
Najprije procjene tvrtkama ili ustanovama preduvjeti za višestruke provjere autentičnosti. To jest, što tvrtke ili ustanove pokušava sigurne.  Ovaj procjenu je važno da biste definirali Tehnički preduvjeti za postavljanje i korisnicima tvrtke i ustanove za višestruke provjere autentičnosti.

>[AZURE.NOTE]
Ako niste upoznati s MFA i funkcija, preporučuje se pročitajte članak [što je Azure višestruka provjera autentičnosti?](../multi-factor-authentication/multi-factor-authentication.md) prethodnog da biste nastavili ovaj odjeljak za čitanje.

Provjerite je li na koja treba odgovoriti na sljedeći način:

- Vaša tvrtka pokušava sigurne aplikacije Microsoft? 
- Kako se objavljuju ove aplikacije?
- Ne tvrtke nude daljinski pristup omogućuje zaposlenika za pristup lokalnog aplikacije?

Ako je da, vrsti daljinski pristup? Morate procijeniti gdje će se nalaziti korisnike kojima pristupaju te aplikacije. U ovom procjenu je drugi važnih koraka da biste definirali strategije proper višestruke provjere autentičnosti. Provjerite odgovaraju na sljedeća pitanja:

- Gdje su korisnici će biti nalazi?
- Se može nalaziti bilo gdje?
- Ne tvrtki želite uspostaviti ograničenja prema korisnikovu lokaciju?

Kada saznate te preduvjete, važno je i procjenu korisničke preduvjete za višestruke provjere autentičnosti. U ovom procjenu važno je jer on će definirati zahtjeve za vodoravnim out višestruke provjere autentičnosti. Provjerite odgovaraju na sljedeća pitanja:

- Su vam već poznati višestruke provjere autentičnosti korisnika?
- Neke upotrebe bit će potrebno omogućuju dodatno unositi?  
 - Ako je da, cijelo vrijeme kada dolaze iz vanjskih mreža ili pristupanju određene programe ili druge uvjetima?
- Hoće li korisnici zahtijevati obuka za postavljanje i implementaciju višestruka provjera autentičnosti?
- Što su ključni scenariji da vaša tvrtka želi omogućiti višestruke provjere autentičnosti za svoje korisnike?

Nakon odgovaranje na pitanja prethodno, moći da biste shvatili ako postoje višestruke provjere autentičnosti koje su već implementirana lokalnog. Ovaj procjenu je važno da biste definirali Tehnički preduvjeti za postavljanje i korisnicima tvrtke i ustanove za višestruke provjere autentičnosti. Provjerite odgovaraju na sljedeća pitanja:

- Vaša tvrtka mora zaštita povlaštene računa s MFA?
- Vaša tvrtka mora omogućiti MFA za neke aplikacije za usklađenost razloga?
- Vaša tvrtka mora odobriti MFA za sve korisnike uvjete ove aplikacije ili samo administratori?
- Morate imate MFA uvijek omogućeno ili samo kada korisnici prijavljeni izvan s korporacijskom mrežom?


## <a name="next-steps"></a>Daljnji koraci
[Definiranje strategije prihvaćanja hibridnog identiteta](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)
