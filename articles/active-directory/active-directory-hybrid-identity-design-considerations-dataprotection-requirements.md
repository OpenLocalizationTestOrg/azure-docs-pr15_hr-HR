<properties
    pageTitle="Azure Active Directory hibridnog identiteta zahtjevi dizajna - određivanje potrebama za zaštitu podataka | Microsoft Azure"
    description="Prilikom planiranja rješenje identiteta hibridnog odredite potrebama za zaštitu podataka za svoju tvrtku i koje su mogućnosti dostupne najbolje fulfil tim preduvjetima."
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

#<a name="plan-for-enhancing-data-security-through-strong-identity-solution"></a>Plan za poboljšavanje sigurnost podataka putem istaknuti identiteta rješenja

Prvi je korak u zaštiti podataka je prepoznati tko može pristupiti te podatke, a kao dio taj postupak, morate imati identitet rješenje koje možete integrira sustav omogućuje provjeru autentičnosti i ovlaštenja mogućnosti. Provjera autentičnosti i autorizacije često isto međusobno i misunderstood njihove uloge. U stvari se prilično razlikuju, kao što je prikazano na slici u nastavku:

![](./media/hybrid-id-design-considerations/mobile-devicemgt-lifecycle.png)
 
**Životni ciklus faze upravljanja za mobilni uređaj**

Prilikom planiranja rješenje identiteta hibridnog morate razumjeti potrebama za zaštitu podataka za svoju tvrtku i koje su mogućnosti dostupne najbolje fulfil tim preduvjetima.
 
>[AZURE.NOTE]
Kada završite s planiranje radi zaštite podataka, pregledajte [procjeni uvjeta višestruke provjere autentičnosti](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md) da biste bili sigurni da odabire vezane uz višestruka provjera autentičnosti preduvjeti su ne utječu odluke koje ste napravili u ovom odjeljku.

## <a name="determine-data-protection-requirements"></a>Određivanje potrebama za zaštitu podataka
U dobi mobilnost Većina proizvođača su uobičajeni cilj: omogućiti svojim korisnicima biti produktivni na mobilnim uređajima dok lokalnog ili daljinski s bilo kojeg mjesta da biste povećali produktivnost. Dok se to može biti zajedničkom cilju, tvrtkama koje imaju takve zahtjeva bit će složen vezanih uz količinu prijetnji koje morate mitigated da bi se zaštiti podataka tvrtke ili ustanove i održavanje izjave o zaštiti privatnosti korisnika. Svako poduzeće može različito tretiraju ako je regard; različite usklađenost pravila koja će se razlikovati prema koje industrijske tvrtke djeluje vodi do odluke različite dizajna. 

No postoje neke sigurnosne aspekti koje trebali biste istražili i potvrđuju, bez obzira na to industrijskih, koje su objašnjene u sljedećem odjeljku.

## <a name="data-protection-paths"></a>Putovi za zaštitu podataka

![](./media/hybrid-id-design-considerations/data-protection-paths.png)
 
**Putovi za zaštitu podataka**

U gornji dijagram komponentu identiteta bit će prvoga se provjeriti prije nego pristupiti podacima. Međutim, te podatke u može biti različiti statusi vrijeme joj je pristupiti. Svaki broj na ovaj dijagram predstavlja put kojim podataka može se nalaziti trenutku u vremenu. Ti brojevi je opisano u nastavku:

1. Zaštita podataka na razini uređaja.
2. Zaštita podataka na putu.
3. Zaštita podataka na ostale lokalnog.
4. Zaštita podataka na ostale u oblaku.

Iako u Tehnički kontrole taj će Omogući IT za zaštitu podataka sam na svaki od tih faze su izravno nudi identiteta rješenje hibridnog je potrebno da je rješenje identiteta hibridnog instaliranog korištenje lokalnom i u oblaku identiteta upravljanja resursima za identifikaciju korisnika prije dopustiti pristup podacima. Prilikom planiranja rješenje identiteta hibridnog bili sigurni da se na sljedeća pitanja odgovoriti prema preduvjeti za vaše tvrtke ili ustanove:

## <a name="data-protection-at-rest"></a>Zaštita podataka na ostale
Bez obzira na to gdje se podaci nalaze na ostale (uređaj, oblak ili lokalno), važno je da izvođenje procjenu u tom regard razumjeti potrebe tvrtke ili ustanove. Za područje, provjerite je li upitani na sljedeća pitanja:

- Vaša tvrtka mora zaštiti podataka na ostale?
 - Ako je da, je rješenje identiteta hibridnog mogućnost integracije s trenutnom infrastruktura za lokalni?
 - Da, ako je rješenje identiteta hibridnog mogućnost integracije s vašeg radnih opterećenja koja se nalazi u oblaku?
- Je li upravljanje identitetom oblaka moći zaštiti korisnikove vjerodajnice i druge podatke pohranjene u oblaku?

## <a name="data-protection-in-transit"></a>Zaštita podataka na putu
Mora biti zaštićene podataka na putu između uređaja i s podatkovnim centrom ili između uređaja i s oblakom. Međutim, koji se tranzitne ne mora značiti procesa komunikacije s komponentom izvan oblaku; Pomicanje interno, isto tako, primjerice između dvije virtualne mreže. Za područje, provjerite je li upitani na sljedeća pitanja:

- Vaša tvrtka mora zaštiti podataka na putu?
 - Da, ako je rješenje identiteta hibridnog mogućnost integracije s sigurne kontrola, kao što su SSL/TLS?
- Ne upravljanje identitetom oblaka zadržati promet unutar spremište direktorija (unutar i između podatkovnim centrima) i da biste prijavili?


## <a name="compliance"></a>Usklađenost
Propisa, zakonima i zahtjeve za usklađenosti s propisima će se razlikovati prema industrijskih koji pripada vaše tvrtke. Tvrtkama na visok regulated djelatnosti morate adresa upravljanje identitetom opasnosti vezane uz usklađenost problema. Propisa kao što su Sarbanes Oxley (SOX), prenosivost zdravstvenog osiguranja i zakon o odgovornost (HIPAA), Act Gramm Leach Bliley (GLBA) i plaćanja kartica industrijskih podataka sigurnost Standard (PCI DSS) su vrlo strogih vezanim uz identiteta i pristup. Rješenje identiteta hibridnog koje vaša tvrtka prihvaćaju mora imati core mogućnosti koje će ispunjavaju preduvjeti jedne ili više ovih propisa. Za područje, provjerite je li upitani na sljedeća pitanja:

- Identitet rješenje hibridnog je sukladan s Zakonski preduvjeti za svoju tvrtku?
- Ne identiteta rješenje hibridnog sadrži ugrađeni mogućnosti koje će se omogućiti tvrtke biti usklađen Zakonski preduvjeti? 
 
>[AZURE.NOTE]
Provjerite je li bilješke svaki odgovor i razumijevanje rationale iza odgovor. [Definiranje Strategije za zaštitu podataka](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) će ići preko mogućnosti dostupne i prednosti i nedostatke svake mogućnosti.  Tako da imate odgovoriti na ta pitanja ćete odabrati koju mogućnost najbolje odgovara vašem poslovanju mora.

## <a name="next-steps"></a>Daljnji koraci
 [Određivanje preduvjeti za upravljanje sadržajem](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)


## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)
