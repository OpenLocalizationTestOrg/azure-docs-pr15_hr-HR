<properties
   pageTitle="Kako se integrirati s Azure Active Directory | Microsoft Azure"
   description="Vodič kroz prednosti i resursi za integraciju s Azure Active Directory."
   services="active-directory"
   documentationCenter="dev-center-name"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="integrating-with-azure-active-directory"></a>Integracija sa Azure Active Directory

[AZURE.INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

Azure Active Directory sadrži tvrtke i ustanove čije upravljanje identitetom poslovne klase za oblak aplikacije.  Integracija Azure AD omogućuje korisnicima na pojednostavnjeno sučelje za prijavu i pomaže aplikacija u skladu sa IT pravila.

## <a name="how-to-integrate"></a>Kako integrirati

Aplikacije za integraciju s Azure AD na nekoliko načina.  Iskoristite prednost kao mnoge ili mali scenarija kao što je odgovarajuće aplikacije.

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>Podržava Azure AD za prijavu u aplikaciju

**Smanjite friction za prijavu i smanjivanje podrške.** Pomoću Azure AD da biste se prijavili aplikaciju vaši korisnici neće imati jednog više naziva i ne zaboravite lozinku.  Kao programer, imat ćete jedna manje lozinka za pohranu i zaštititi.  Ne morate učiniti zaboravljene lozinke može biti vrlo računanje samostalno.  Potenciranje Azure AD za prijavu u za neke na svijetu najpopularnijih oblaka aplikacije, uključujući sustava Office 365 i Microsoft Azure.  S stotine milijuna korisnika iz milijune tvrtke ili ustanove, vjerojatno korisnički već prijavili na Azure AD.  Dodatne informacije o [dodavanju podrška za Azure AD za prijavu](../active-directory-authentication-scenarios.md).

**Pojednostavnite znak prema gore za svoju aplikaciju.**  Prilikom registracije za aplikaciju, Azure AD možete poslati ključne informacije o korisnika tako da mogu unaprijed ispune na registracije obrasca ili ga potpuno uklonili.  Korisnicima možete se prijaviti za svoju aplikaciju pomoću svoj račun za Azure AD putem sučelja poznatih pristanak slični onima u društvenih mreža i mobilne aplikacije.  Bilo koji korisnik možete prijaviti i prijavite se u aplikaciju koja je integriran s Azure AD bez pomoći IT involvement.  Saznajte više o [potpisivanja kopirane aplikacija za prijavu Azure AD računa](../../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>Traženje korisnika, upravljanje dodjeljivanje korisnika i kontrola pristupa u aplikaciji

**Traženje korisnika u imeniku.**  Korištenje API grafikonu da bi korisnicima i pretraživati drugim osobama u tvrtki ili ustanovi kada pozvati ostale ili omogućivanju pristupa, umjesto da ih upisati e-pošte potrebno adrese.  Korisnici mogu pregledavati koristeći poznate adresu adresara stil sučelje, uključujući prikaz detalja o organizacijskoj hijerarhiji.  Dodatne informacije o [API grafikonu](../active-directory-graph-api.md).

**Ponovno korištenje grupa servisa Active Directory i klijentu već Upravljanje popisima za raspodjelu.**  Azure AD sadrži grupe koji klijent je već koristite za raspodjelu e-pošte i upravljanje pristupom.  Korištenje Graph API, iskoristite te grupe umjesto potrebno klijenta za stvaranje i upravljanje zaseban skup grupe u aplikaciji.  Podaci grupe mogu se slati i u aplikaciji u tokeni za prijavu.  Dodatne informacije o [API grafikonu](../active-directory-graph-api.md).

**Koristite Azure AD da biste kontrolirali tko ima pristup u aplikaciji.**  Administratori i vlasnici aplikacije u Azure AD možete dodijeliti pristup aplikacijama na određene korisnike i grupe.  Pomoću API grafikonu, možete čitati ovaj popis i pomoću njega možete odrediti dodjele resursa i poništite dodjeljivanje resursa i pristupiti u aplikaciji.

**Korištenje Azure AD za uloge kontrola pristupa na temelju.**  Administratori i vlasnici aplikaciju možete dodijeliti korisnicima i grupama uloge koje sami definirate kada registrirate vaše aplikacije u Azure AD.  Informacije o ulozi se šalje u aplikaciji u tokeni za prijavu i također je moguće čitati u grafikonu API-JA.  Dodatne informacije o [korištenju Azure AD za autorizaciju](http://blogs.technet.com/b/ad/archive/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles.aspx).

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>Dobiti pristup korisničkog profila, kalendar, e-pošte, kontakata, datoteke i dodatne informacije

**Azure AD je provjeru autentičnosti poslužitelja za Office 365 i druge Microsoftove servise za tvrtke.**  Ako podržava Azure AD za prijavu u aplikaciju ili podršku povezivanje trenutni korisničke račune za Azure AD korisničke račune pomoću OAuth 2.0, možete zatražiti čitanja i pisanja korisničkog profila, kalendar, e-pošte, kontakata, datoteke i druge informacije.  Možete jednostavno zapisivanje događaja kalendara korisnika i čitanje ili pisanje datoteke na OneDrive.  Dodatne informacije o [pristupu API -ji sa sustavom Office 365](https://msdn.microsoft.com/office/office365/howto/platform-development-overview).

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>Promicanje aplikacije u Azure i Marketplaces za Office 365

**Promicanje aplikaciju milijune tvrtkama ili ustanovama koje već koriste Azure AD.**  Korisnike i pretraživati te marketplaces već koriste jedan ili više servise u oblaku, čega pogodni klijenti servisa oblaka.  Saznajte više o prosljeđivanja aplikacija [na](https://azure.microsoft.com/marketplace/partner-program/)tržištu Azure.

**Kada korisnici se registrirate za aplikaciju, pojavit će se u njihove ploče programa access za Azure AD i u pokretač aplikacija sustava Office 365.**  Korisnici će moći da biste se vratili u aplikaciju na noviju verziju, brzo i jednostavno poboljšanje radnje korisnika.  Dodatne informacije o [Azure AD pristupa ploči](../active-directory-saas-access-panel-introduction.md).

### <a name="secure-device-to-service-and-service-to-service-communication"></a>Komunikacija servisa uređaja i servis za servis za sigurnu

**Korištenje Azure AD za upravljanje identitetom servisa i uređaje smanjuje kod morate napisati i omogućuje IT za upravljanje pristupom.**  Usluge i uređaje možete tokeni zatražite od službe za Azure AD pomoću OAuth i koristiti te tokeni da biste pristupili web API-ji.  Korištenje Azure AD možete izbjeći pisanja koda složene provjere autentičnosti.  Budući da identiteta servisa i uređaje spremaju se u Azure AD IT možete upravljati ključeva i opoziv na jednom mjestu umjesto da da biste to učinili zasebno u aplikaciji.

## <a name="benefits-of-integration"></a>Prednosti integracije

Integracija s Azure AD isporučuje se s pogodnosti koje zahtijevaju dodatne kod.

### <a name="integration-with-enterprise-identity-management"></a>Integracija s upravljanje identitetom tvrtke

**Pomoć za aplikacije usklađivanja s pravilnicima o IT.**  Tvrtke i ustanove integriranje sustava za upravljanje identiteta svoje tvrtke s Azure AD tako da kada osoba napusti tvrtkom ili ustanovom, oni će automatski izgubiti pristup bez ikakvih IT koje je potrebno poduzeti dodatne korake.  IT možete upravljati tko može pristupiti aplikaciji i odrediti koje access pravilnike za potrebni su - primjer višestruka provjera autentičnosti - smanjivanju vaše potrebe za pisanje koda u skladu sa složenim korporativnih pravila.  Azure AD omogućuje administratorima zapisnik nadzora detaljne tko prijavili u aplikaciju pa IT možete pratiti korištenje.

**Azure AD proširuje servisa Active Directory u oblak tako da se aplikacija možete integrirati s AD.**  Mnoge organizacije diljem svijeta pomoću servisa Active Directory kao njihove glavni prijave i identiteta sustava za upravljanje, a zahtijevaju svoje aplikacije da biste radili s AD.  Integracija sa Azure AD integrira u aplikaciju servisa Active Directory.

### <a name="advanced-security-features"></a>Naprednih sigurnosnih značajki

**Višestruka provjera autentičnosti.**  Azure AD sadrži izvorni višestruke provjere autentičnosti.  Administratori mogu biti višestruke provjere autentičnosti da biste pristupili aplikacije, tako da ne morate kod tu podršku.  Saznajte više o [Višestruke provjere autentičnosti](https://azure.microsoft.com/documentation/services/multi-factor-authentication/).

**Anomalous prijava otkrivanje.**  Azure AD obrađuje više milijarde znak dodaci dnevno tijekom korištenja strojnog učenja algoritama za otkrivanje sumnjive aktivnosti i obavijesti IT administratorima moguće probleme.  Po podrške Azure AD prijavu, aplikacija dohvaća prednost te zaštite. Saznajte više o [Prikaz Azure Active Directory pristupati izvješću](../active-directory-view-access-usage-reports.md).

**Uvjetno pristup.**  Osim višestruka provjera autentičnosti, možete zatražiti administratori određenim uvjetima mora zadovoljavati korisnici mogli prijaviti u aplikaciju.  Uvjeti koji mogu postaviti obuhvaćaju rasponu IP adresa o klijentskim uređajima, članstvo u grupama za navedeni i stanje uređaja koji se koristi za pristup.  Dodatne informacije o [pristupu za uvjetno Azure Active Directory](../active-directory-conditional-access.md).

### <a name="easy-development"></a>Jednostavno razvoj

**Standardnih industrijskih protokola.**  Microsoft nastoji standardima za podršku.  Azure AD podržava SAML 2.0, 1.0 povezivanje OpenID, OAuth 2.0 i WS Federacija 1.2 protokola za provjeru autentičnosti.  API grafikonu je OData 4.0 usklađen.  Ako aplikacija već podržava SAML 2.0 ili povezivanje 1.0 OpenID protokoli za vanjsko za prijavu, dodavanjem podrška za Azure AD može biti jasan.  Saznajte više o [Azure AD podržane protokoli za provjeru autentičnosti](../active-directory-authentication-protocols.md).

**Otvaranje biblioteke izvora.**  Microsoft pruža potpuno podržane Otvori izvor biblioteke za popularne jezike i platforme za razvoj brzinu.  Izvorni kod licenciran u odjeljku Apache 2.0, a niste slobodni o ogranku i suradnju na projektima.  Dodatne informacije o [bibliotekama Azure AD provjere autentičnosti](../active-directory-authentication-libraries.md).

### <a name="worldwide-presence-and-high-availability"></a>Diljem svijeta podaci o prisutnosti i visoke dostupnosti

**Azure AD je uveden u podatkovnim centrima diljem svijeta i upravlja i nadzirati oko clock.**  Azure AD je sustav upravljanja identiteta za Microsoft Azure i Office 365 te je uveden u 28 podatkovnim centrima diljem svijeta.  Direktorija podataka se zajamčeno je replicirati na najmanje tri podatkovnim centrima.  Globalni učitavanja balancers provjerite korisnicima pristup najbliže kopiju Azure AD svojim podacima i automatski ponovno usmjeravanje zahtjevi za druge podatkovnim centrima ako je otkriven problem.

## <a name="next-steps"></a>Daljnji koraci

[Početak rada pisanja koda](../active-directory-developers-guide.md#getting-started).

[Prijava korisnika pomoću Azure AD](../active-directory-authentication-scenarios.md)
