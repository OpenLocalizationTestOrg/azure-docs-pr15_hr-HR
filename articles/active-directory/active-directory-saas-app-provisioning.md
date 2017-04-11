<properties
    pageTitle="Automatski SaaS aplikacije korisnik Dodjela resursa u Azure AD | Microsoft Azure"
    description="Uvod u korištenju Azure AD automatski Dodjela Poništi dodjelu resursa, a stalno ažurirati korisničke račune preko više aplikacija SaaS drugih proizvođača."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>Automatiziranje korisničkog dodjele resursa i Deprovisioning SaaS aplikacije s Azure Active Directory

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>Što je automatski korisnika dodjele resursa za SaaS aplikacije?

Azure Active Directory (Azure AD) omogućuje vam da biste automatizirali stvaranje, održavanje i uklanjanje identitetima korisnika u oblaku ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) aplikacijama kao što su zajedničke mrežne mape, Salesforce, ServiceNow i drugo.

**U nastavku su neki primjeri što ova značajka omogućuje vam:**

- Automatski stvoriti novi računi u desnom SaaS aplikacije za nove korisnike kada se pridružite vaš tim.
- Automatski deaktivirati računa iz aplikacija SaaS kada korisnici inevitably ostavite tima.
- Provjerite je li da identiteta u aplikacijama za SaaS su ažurni na temelju promjena u direktoriju.
- Dodjela resursa za objekte koji nisu dodijeljeni korisnicima, kao što su grupe, SaaS aplikacije s podrškom za njih.

**Dodjeljivanje automatizirana korisnika sadrži i sljedeće funkcije:**

- Mogućnost tako da odgovara postojeće identiteta između Azure AD i SaaS aplikacije.
- Prilagodba mogućnosti za pomoć Azure AD stane trenutne konfiguracije SaaS aplikacije u vašoj tvrtki ili ustanovi trenutno koristite.
- Neobavezni e-pošte upozorenja za pogreške u dodjeljivanju.
- Izvješćivanje o pogreškama i aktivnost zapisnicima će vam pomoći u nadzor i otklanjanje poteškoća.

##<a name="why-use-automated-provisioning"></a>Zašto koristiti automatiziranog dodjele resursa?

Neke uobičajene motivations za korištenje ove značajke obuhvaćaju sljedeće:

- Da biste izbjegli troškove, neučinkovitosti i Ljudski pogreške pridružene ručno Dodjeljivanje procesa.
- Radi zaštite vaše tvrtke ili ustanove uklanjanjem trenutačno identiteti korisnika iz ključa SaaS aplikacija kada napuste tvrtku ili ustanovu.
- Da biste u određenu aplikaciju SaaS jednostavno uvezli skupno broj korisnika.
- Uživati praktičnost pamtiti dodjele resursa rješenje s istom pravilnike aplikacije programa access koji ste definirali za Azure AD jedinstvenu prijavu Pokreni.

##<a name="frequently-asked-questions"></a>Najčešća pitanja

**Koliko se često Azure AD pisanje direktorija promjene aplikaciju SaaS?**

Azure AD provjerava ima li promjena svakih pet do deset minuta. Ako aplikaciju SaaS je povratka nekoliko pogrešaka (primjerice kao u slučaju koji nisu valjani administratorske vjerodajnice), Azure AD će se postupno usporiti njegov frekvencija najviše jedanput dnevno dok rješava pogreške.

**Koliko će trajati Dodjela korisnike?**

Rastuća promjena dogoditi gotovo odmah, ali ako pokušavate Dodjela većinu direktorija, pa ga ovisi o broju korisnika i grupa koje imate. Small direktorija potrajati nekoliko minuta, srednjim direktorija može potrajati nekoliko minuta i vrlo velike direktorija može potrajati nekoliko sati.

**Kako možete pratiti napredak trenutne dodjele resursa posla?**

Možete pregledati račun dodjeljivanje izvješća u odjeljku Reports u direktoriju. Druga mogućnost je posjetiti kartica nadzorna ploča za SaaS aplikacije koje su dodjele resursa za pa potražite u odjeljku "Stanje integracije" pri dnu stranice.

**Kako će znati je li korisnicima se neće ispravno dodjeli?**

Na kraju dodjele resursa konfiguraciju čarobnjak još je mogućnost Pretplaćivanje na obavijesti e-pošte za dodjelu resursa pogreške. Možete provjeriti i pogreške izvješće dodjele resursa da biste vidjeli korisnike koji nije uspjelo biti dodjeli i zašto.

**Možete Azure AD odgovorili promjene u aplikaciji SaaS imeniku?**

Za većinu SaaS aplikacije, dodjeljivanje samo je za izlazni, što znači da korisnici zapisuju iz imenika aplikaciji, a promjene iz aplikacije ne može biti napisani ponovno direktoriju. [Workday](https://msdn.microsoft.com/library/azure/dn762434.aspx), međutim, dodjele resursa je dolazni samo, što znači da korisnici uvoze se u direktoriju iz Workday, a isto tako, promjene u imeniku ne pregledati pisane natrag u Workday.

**Kako poslati povratne informacije za tim?**

Obratite nam se kroz [Azure Active Directory forum za povratne informacije](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="how-does-automated-provisioning-work"></a>Kako automatiziranog dodjele resursa rada?

Azure AD dodjeljuje korisnicima aplikacije SaaS povezivanjem dodjeljivanje krajnje točke koje ste dobili od svakog dobavljača aplikacije. Te krajnje točke omogućuju Azure AD programski stvaranje, ažuriranje i uklanjanje korisnika. U nastavku je kratki pregled drugačije korake koje Azure AD potrebno da biste automatizirali dodjele resursa.

1. Kada omogućite dodjele resursa za aplikaciju za prvo izvršavaju se sljedeće radnje:
 - Azure AD će pokušati zadovoljavaju bilo koje postojeće korisnike u aplikaciji SaaS s odgovarajućih identiteti u direktoriju. Kada se uparuje korisnika, oni se *neće* se automatski omogućena za jedinstvenu prijavu. U redoslijedu korisnik ima pristup aplikaciji ih mora biti izričito dodijeljena aplikaciju u Azure AD izravno ili putem članstvo u grupi.
 - Ako ste već naveli koje korisnicima biti dodijeljene aplikacija, a ne uspijete Azure AD da biste pronašli postojeće račune za korisnike, Azure AD će Dodjela nove račune za njih u aplikaciji.
2. Kada je opisan je dovršeno početne sinkronizacije, Azure AD provjerit će svakih 10 minuta za sljedeće promjene:
 - Ako se novi korisnici su dodijeljene aplikacije (izravno ili putem članstva u grupi), tada će biti dodijeljena novog računa u aplikaciji SaaS.
 - Ako je uklonjena s korisničkog pristupa, a zatim putem računa u aplikaciji SaaS će biti označena kao onemogućena (korisnici potpuno nikad ne brišu se, koji štiti koju od gubitka podataka u slučaju u konfiguraciji).
 - Ako korisnik nedavno dodijeljen aplikacija, a oni već imate račun u aplikaciji SaaS, taj račun bit će označene kao omogućeni i određenog svojstva korisničkog možda se ažurirati ako su zastarjele u usporedbi s imenika.
 - Ako u imeniku je promijenjena korisničke podatke (kao što su telefonski broj, mjesto ureda itd), a zatim tih podataka će se ažurirati i u aplikaciji SaaS.

Dodatne informacije o načinu na koji se atributi mapiraju između Azure AD te SaaS aplikaciju, potražite u članku na [Prilagodba mapiranja atributa](active-directory-saas-customizing-attribute-mappings.md).

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>Popis aplikacija s podrškom za dodjelu resursa automatiziranog korisnika

Kliknite aplikaciju da biste vidjeli na vodič o tome kako konfigurirati automatiziranog dodjele resursa za njega:

- [Okvir](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [Concur](http://go.microsoft.com/fwlink/?LinkId=309575)
- [Docusign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [Dropbox za tvrtke](http://go.microsoft.com/fwlink/?LinkId=309581)
- [Servisa Google Apps](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [Salesforce](http://go.microsoft.com/fwlink/?LinkId=286017)
- [Izdvojeni Salesforce](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [WORKDAY](http://go.microsoft.com/fwlink/?LinkId=690250) (ulazni dodjele resursa)

Kako bi programa koji podržava dodjele resursa automatiziranog korisnika, ga morate unijeti potrebne krajnje točke koje omogućuju vanjske programe da biste automatizirali stvaranje, održavanje i uklanjanje korisnika. Stoga su sve aplikacije SaaS kompatibilan s tom značajkom. Za aplikacije koji podržavaju to tim Azure AD bit će moći sastavljanje poveznik za dodjelu resursa za tim aplikacijama, a to funkcioniralo je prioritet prema potrebama trenutne i potencijalni klijenti.

Da biste se obraćate službi tehničke Azure AD za upućivanje zahtjeva za dodjelu resursa podrške za dodatne aplikacije, pošaljite poruku kroz [Azure Active Directory forum za povratne informacije](https://feedback.azure.com/forums/169401-azure-active-directory/).

##<a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Prilagodba mapiranja atributa za dodjelu resursa korisnika](active-directory-saas-customizing-attribute-mappings.md)
- [Pisanje izraza za mapiranja atributa](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [Određivanje opsega filtre za dodjelu resursa korisnika](active-directory-saas-scoping-filters.md)
- [Koristite SCIM da biste omogućili automatsko dodjeljivanje korisnika i grupa iz servisa Azure Active Directory aplikacijama](active-directory-scim-provisioning.md)
- [Račun dodjeljivanje obavijesti](active-directory-saas-account-provisioning-notifications.md)
- [Popis vodiče za integraciju SaaS aplikacije](active-directory-saas-tutorial-list.md)
