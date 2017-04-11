<properties
    pageTitle="Preduvjeti za upravljanje sadržajem za određivanje Azure Active Directory hibridnog identiteta zahtjevi dizajna - | Microsoft Azure"
    description="Daje uvid u kako odrediti preduvjeti upravljanje sadržajem za svoju tvrtku. Obično kada korisnik ima svoju vlastitu uređaj on može imati i više vjerodajnice će Izmjenični prema aplikaciji koja on koristi. Važno je da razlikovati koji sadržaj stvoren pomoću osobne vjerodajnice i one stvorene pomoću vjerodajnica za tvrtke. Identitet rješenje trebali biste moći raditi s oblaka services radi olakšavanja objedinjenog doživljaj krajnjeg korisnika prilikom provjerite je li svoj izjave o zaštiti privatnosti i povećati zaštitu od oštećenja podataka."
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

# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>Određivanje preduvjeti za upravljanje sadržajem za rješenje hibridnog identiteta

Objašnjenje preduvjeti za upravljanje sadržajem za svoju tvrtku možda izravno na odluku utjecati na koji hibridnog identiteta rješenje. Uz proliferation više uređaja i mogućnostima korisnika da bi se prikazala vlastite uređaje ([BYOD](http://aka.ms/byodcg)), tvrtke moraju zaštititi zasebnom podatke, ali je morate zadržati privatnosti korisnika ostaje netaknuta. Obično kada korisnik ima svoju vlastitu uređaj on može imati i više vjerodajnice će Izmjenični prema aplikaciji koja on koristi. Važno je da razlikovati koji sadržaj stvoren pomoću osobne vjerodajnice i one stvorene pomoću vjerodajnica za tvrtke. Identitet rješenje trebali biste moći raditi s oblaka services radi olakšavanja objedinjenog doživljaj krajnjeg korisnika prilikom provjerite je li svoj izjave o zaštiti privatnosti i povećati zaštitu od oštećenja podataka. 

Rješenje identiteta će leveraged različite tehničke kontrole radi upravljanja sadržajem, kao što je prikazano na slici u nastavku:
 
![](./media/hybrid-id-design-considerations/securitycontrols.png)

**Sigurnosne kontrole koje će se korištenje sustav upravljanja identiteta**

Općenito govoreći, preduvjeti za upravljanje sadržajem će pod utjecajem sustav upravljanja identiteta u sljedećih područja:

- Zaštita privatnosti: Označavanje korisnika koji je vlasnik resursa i Primjena odgovarajuće kontrole za održavanje cjelovitosti.
- Klasifikacija podataka: odredite korisnik ili grupa i razinu pristupa objekta prema njegov klasifikacija. 
- Zaštita od oštećenja podataka: sigurnosne kontrole odgovoran za zaštitu podataka da biste izbjegli oštećenja morat ćete sustavu identiteta za provjeru valjanosti identitet korisnika. To je važno za nadzor trag svrhu.

>[AZURE.NOTE]
Čitanje [podataka klasifikacija za pripremu oblaka](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) dodatne informacije o najbolje prakse i smjernice za klasifikaciju podataka.

Prilikom planiranja rješenje identiteta hibridnog bili sigurni da se na sljedeća pitanja odgovoriti prema preduvjeti za vaše tvrtke ili ustanove:

- Ima li vaša tvrtka sigurnosne kontrole na mjestu da biste nametnuli podataka o zaštiti privatnosti?
 - Ako da, te kontrole sigurnost moći za integraciju s rješenje identiteta hibridnog koje namjeravate prihvaćaju?
- Vaša tvrtka koristi klasifikaciju podataka?
 - Ako je da, trenutno rješenje je mogućnost integracije s rješenjem hibridnog identitet koji ćete prihvaćaju?
- Ne li vaša tvrtka trenutno imate rješenjem za oštećenja podataka? 
 - Ako je da, trenutno rješenje je mogućnost integracije s rješenjem hibridnog identitet koji ćete prihvaćaju?
- Vaša tvrtka mora nadzor pristupa resursima?
 - Ako je da, vrsti resursa?
 - Ako je da, razinu podataka nije potrebno?
 - Ako je da, gdje zapisnik nadzora mora nalaziti? Lokalni u oblaku?
- Vaša tvrtka mora da biste šifrirali sve poruke e-pošte koji sadrže osjetljive podatke (SSNs, brojeve kreditnih kartica i itd.)?
- Vaša tvrtka mora Šifriraj sve dokumente/sadržaj zajedničko korištenje s vanjskim poslovnim partnerima?
- Vaša tvrtka mora provođenje korporativnih pravila na određene vrste poruke e-pošte (ne bez Odgovori svima, ne prosljeđuj)?
 
>[AZURE.NOTE]
Provjerite je li bilješke svaki odgovor i razumijevanje rationale iza odgovor. [Definiranje Strategije za zaštitu podataka](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md) će ići preko mogućnosti dostupne i prednosti i nedostatke svake mogućnosti.  Tako da imate odgovoriti na ta pitanja ćete odabrati koju mogućnost najbolje odgovara vašem poslovanju mora.


## <a name="next-steps"></a>Daljnji koraci
[Određivanje preduvjeti za kontrolu pristupa](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>Vidi također
[Pregled pitanja vezana uz dizajn](active-directory-hybrid-identity-design-considerations-overview.md)
