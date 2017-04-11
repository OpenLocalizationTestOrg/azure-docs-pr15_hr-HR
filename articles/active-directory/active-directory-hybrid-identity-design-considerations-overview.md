<properties
    pageTitle="Azure Active Directory hibridnog identiteta zahtjevi dizajna – pregled | Microsoft Azure"
    description="Pregled i sadržaja mape hibridnog identiteta dizajn pitanja vezana uz vodiča"
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

# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Zahtjevi za dizajna identiteta hibridnog Azure Active Directory

Utemeljenu na potrošača uređaji su proliferating tvrtke svijeta pa su jednostavni prihvaćaju (SaaS) aplikacije utemeljene na oblaku softver kao-na-usluga. Zbog toga održavanje kontrole nad pristupom korisnika aplikacije preko Interna podatkovni centri i oblak platforme je izazov.  Rješenja za identitet tvrtke Microsoft obuhvaćati lokalnog i oblaku mogućnostima, stvaranje jednog korisničkog identiteta za provjeru autentičnosti i ovlaštenja na sve resurse, bez obzira na to mjesto. Ne možemo nazovite hibridnog identitet. Postoje drugi dizajn i mogućnosti konfiguracije hibridnog identiteta pomoću rješenja za Microsoft, a u nekim slučaj može biti teško utvrditi koja kombinacija najbolje odgovaraju potrebama vaše tvrtke ili ustanove. Ovaj vodič hibridnog identiteta dizajn pitanja vezana uz će vam pomoći da biste razumjeli kako dizajnirati rješenje hibridnog identiteta koji najbolje odgovara tvrtke i tehnologije potrebne za tvrtku ili ustanovu.  Ovaj vodič ćete detaljno niz koraka i zadatke koji možete pratiti da biste lakše dizajniranje rješenja za hibridno identitet koji ispunjava specifičnim potrebama vaše tvrtke ili ustanove. Kroz korake i zadatke, vodič prikazati odgovarajuće tehnologije i mogućnosti značajki tvrtkama ili ustanovama natjecanjima funkcionirati i kvalitete usluge (primjerice dostupnost, skalabilnost, performanse, mogućnost upravljanja i sigurnost) razine preduvjeti. Konkretno, hibridnog identiteta projektne pitanja vezana uz vodič ciljeve ćete odgovaraju na sljedeća pitanja: 

- Što pitanja moram postavljanje i odgovarati na pogon hibridnog specifične za identitet dizajna za tehnologiju ili problem domenu ispunjava li najbolje Moje preduvjeti?
- Koje slijeda aktivnosti dovršavanje za dizajniranje rješenja hibridnog identiteta za domenu tehnologiju ili problem? 
- Koje hibridnog identiteta tehnologije i konfiguraciji su mogućnosti dostupne za pomoć za zadovoljavaju Moje preduvjeti? Što su u gubitke između tih mogućnosti tako da možete odabrati najbolja mogućnost za Moj posao?


## <a name="who-is-this-guide-intended-for"></a>Tko je vodič namijenjen?
 CIO, CITO, projektantima Šef identiteta, Enterprise projektantima i IT projektantima odgovorni za dizajniranje rješenja hibridnog identiteta za srednje i velike tvrtke ili ustanove.

## <a name="how-can-this-guide-help-you"></a>Kako može ovaj vodič pomoći? 
Ovaj vodič možete koristiti da biste razumjeli kako dizajniranje rješenja za hibridno identitet koji se nalazi mogućnost integracije sustava za upravljanje identiteta u oblaku s trenutnog lokalnih identiteta rješenja. Sljedeća grafika prikazuje primjer rješenja za hibridno identitet koji omogućuje IT administratori za upravljanje da biste integrirali njihove trenutnog rješenja Active Directory za Windows Server koja se nalazi lokalnog s Microsoft Azure Active Directory radi omogućivanja korisnicima da biste koristili jednu prijavu (SSO) putem aplikacije koja se nalazi u oblak i lokalne.

![](./media/hybrid-id-design-considerations/hybridID-example.png)


Primjer rješenja za hibridno identiteta koje je korištenje servisa u oblaku za integraciju s mogućnosti lokalnih omogućili jedan sučelje za postupak provjere autentičnosti za krajnjeg korisnika, a da biste olakšali je iznad slika IT upravljanje tih resursa. Iako to može biti vrlo uobičajeni scenarij, svaki tvrtke ili ustanove hibridnog identiteta dizajna je vjerojatno će biti razlikuje se od primjera podržali slika 1 zbog različitih preduvjeti. Ovaj vodič sadrži niz koraka i zadatke koji možete pratiti za dizajniranje rješenja za hibridno identitet koji ispunjava specifičnim potrebama vaše tvrtke ili ustanove. Kroz sljedeće korake i zadatke, vodič prikazuje relevantne tehnologije i mogućnosti značajki dostupnih da bi odgovarao funkcionalni i usluge kvalitete razine preduvjeti za tvrtku ili ustanovu.

**Pretpostavke**: imate iskustva s Windows Server, Active Directory Domain Services i Azure Active Directory. U ovom dokumentu, pretpostavimo da ste tražili kako ova rješenja možete svojim potrebama tvrtke vlastite ili mogućnosti integriranih rješenja.

## <a name="design-considerations-overview"></a>Pregled pitanja vezana uz dizajn
Ovaj dokument sadrži skup koraka i zadatke koji možete pratiti za dizajniranje rješenja za hibridno identitet koji najbolje odgovara potrebama. Koraci su usmjereni uređeni niz. Zahtjevi dizajna saznat ćete kasnije koraka možda potrebno da biste promijenili odlukama u starijim koraka, međutim, zbog sukobljenih mogućnosti dizajna. Svaki pokušaj upozorenje sukoba dizajn cijelom dokumentu. 

Stizati na dizajn najbolje odgovara potrebama tek nakon što iterating kroz korake neograničen broj puta koliko je potrebno da biste sve pitanja vezana uz unutar dokumenta. 

| Faza hibridnog identiteta                                             | Popis tema                                                                                                                                                                                       |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Određivanje preduvjeti za identitet                                   | [Određivanje poslovne potrebe](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [Određivanje preduvjeti za sinkronizaciju direktorija](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [Određivanje preduvjeti višestruka provjera autentičnosti](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [Definiranje strategije prihvaćanja hibridnog identiteta](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)                       |
| Plan za poboljšavanje sigurnost podataka putem istaknuti identiteta rješenja | [Određivanje potrebama za zaštitu podataka](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [Određivanje preduvjeti za upravljanje sadržajem](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [Određivanje preduvjeti za kontrolu pristupa](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [Određivanje incidenta odgovor preduvjeti](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [Definiranje Strategije za zaštitu podataka](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)  |
| Plan za životni ciklus hibridnog identiteta                                | [Određivanje zadatke upravljanja identiteta hibridnog](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [Upravljanje sinkronizacijom](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [Određivanje strategiju prihvaćanja hibridnog identiteta](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##<a name="download-this-guide"></a>Preuzmite ovaj vodič
Pdf verziju hibridnog identiteta dizajn pitanja vezana uz vodič možete preuzeti iz [galerije Technet](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288). 

                                                             
