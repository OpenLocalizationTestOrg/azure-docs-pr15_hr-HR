<properties
    pageTitle="Upravljanje aplikacijama s Azure Active Directory | Microsoft Azure"
    description="Članak prednosti integracije Azure Active Directory s lokalnim, oblaka i SaaS aplikacije."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="10/10/2016"
      ms.author="markvi"/>

# <a name="managing-applications-with-azure-active-directory"></a>Upravljanje aplikacijama s Azure Active Directory

Osim tijeka rada ili sadržaja tvrtke imaju dva osnovni sistemski preduvjeti za sve aplikacije:

1. Da biste povećali produktivnost, mora biti lako otkrivanje i pristup aplikacijama

2. Da biste omogućili sigurnost i upravljanja, u tvrtki ili ustanovi kontrola i nadzor na tko smije i zapravo pristupa sve aplikacije

U svijetu oblaka aplikacije to se može najbolje postići pomoću identiteta kontrolu "*TKO može izvršiti*".

U računalstvu terminologija:

- *Koji* se naziva *identiteta* - Upravljanje korisnicima i grupama

- *Što* se naziva *Upravljanje pristupom* – upravljanje pristupa zaštićenim resursi

Oba komponente zajedno nazivaju *identiteta i upravljanje pristupom (IAM)*, koji definira grupi [Gartner](http://www.gartner.com/it-glossary/identity-and-access-management-iam) kao "*disciplina sigurnosti koji omogućuje desnom osobe da biste pristupili prave resurse na desnoj strani puta desnom razloga*".

Redu, što je problem? Ako je IAM *ne upravlja* na jednom mjestu s mogućnosti integriranih rješenja:

- Identitet administratori imaju pojedinačno stvaranje i ažuriranje korisničkih računa u svim aplikacijama zasebno, redundantnih i dugotrajan aktivnosti.

- Korisnici imaju memorize više vjerodajnice za pristup aplikacijama koje su im potrebne za rad s. Zbog toga korisnici često zapišite svoje lozinke ili pomoću drugih rješenja za upravljanje lozinku koja predstavlja sigurnosni rizik druge podatke.

- Suvišne, dugotrajan aktivnosti smanjite količinu vremena korisnicima i administratorima radite poslovnih aktivnosti povećati donja crta za svoju tvrtku.

Zato što obično sprječava tvrtke ili ustanove s usvojio integrirani IAM rješenja?

- Rješenja za najčešće tehničke temelje se na platformama softver koje treba uvesti i prilagođen tako da svaki tvrtke ili ustanove za svoje aplikacije.

- Oblak aplikacije često su prihvatile brzinom veća od IT tvrtki ili ustanovi možete integrirati s postojeća rješenja IAM.

- Sigurnost i nadzor tooling potrebna dodatna prilagođavanje i integracija da biste postigli potpun E2E scenariji.

## <a name="azure-active-directory-integrated-with-applications"></a>Azure Active Directory integriran s aplikacijama

Azure Active Directory je Microsoftov potpun identitet kao rješenje servisa (IDaaS) koji:

- IAM kao omogućuje servise u oblaku 

- Pruža access središnje upravljanje jedinstvene prijave (SSO), a izvješćivanja 

- Upravljanje integrirani pristupa podržava za [tisuće aplikacije](https://azure.microsoft.com/marketplace/active-directory/) u galeriji aplikacije, uključujući Salesforce, servisa Google Apps, okvir, Concur i više. 


Azure Active Directory, sve aplikacije objavljivanja partnera i kupcima (poslovni ili potrošača) imaju isti identiteta i pristupiti mogućnostima upravljanja.<br> Omogućuje znatno smanjiti radu troškove.

Što se događa ako trebate implementirati aplikaciju koja nije naveden u galeriji aplikacije? Dok je malo više dugotrajan od konfiguriranje SSO za aplikacije iz galerije aplikacije, Azure AD omogućuje čarobnjak koji vam pomaže u konfiguraciji.

Vrijednost Azure AD prelazi "samo" oblaka aplikacije. Vam može poslužiti ga s aplikacijama na lokalni unosom sigurne daljinski pristup. S sigurne daljinski pristup, možete isključiti za potrebe za VPN-ovi ili druge implementacije upravljanje tradicionalni daljinski pristup.

Unosom access središnje upravljanje i jednom prijava (SSO) za sve aplikacije Azure AD nudi rješenje glavni podataka sigurnost i produktivnost problema.

- Korisnici mogu pristupati više aplikacija s jedne prijave daje još jedanput dobiti i generiranje ili poslovni aktivnosti operacije gotovo.

- Identitet administratori mogu upravljati pristup aplikacijama na jednom mjestu.

Očite je nudio za korisnika, a za svoju tvrtku. Pogledajmo o čemu pozornije razmotrite prednosti administrator identiteta i tvrtke ili ustanove.

## <a name="integrated-application-benefits"></a>Prednosti integrirane aplikacije

Postupak SSO sastoji se od dva koraka:

- Provjera autentičnosti, postupak provjere valjanosti identitet korisnika.

- Autorizacija odluku da želite omogućiti ili bloka pristup resursu s pravilima programa access.

Prilikom korištenja Azure AD za upravljanje aplikacijama i omogućivanje SSO:

- Provjera autentičnosti obavlja na lokalni (npr. AD) ili Azure AD računa.

- Autorizacija se izvršava na Azure AD dodjele i zaštitu pravila osiguravanje iskustvo dosljedan krajnjeg korisnika i omogućujući vam da biste dodali dodjele, mjesta i uvjeta MFA na bilo koju aplikaciju, bez obzira na to njegovim Interna mogućnostima.

Je razumjeti da su način autorizacija enacted na ciljnu aplikaciju razlikuje se ovisno o tome kako je aplikacija integrirani Azure AD.

- **Davatelj usluga unaprijed integrirane aplikacije** Kao što su Office 365 i Azure, to su aplikacije ugrađena izravno na Azure AD i potrebe za oslanjanjem na njemu za svoje potpun identiteta i pristup mogućnostima upravljanja. Pristup te aplikacije omogućena imeničkim podacima i tokena izdavanja.

- **Unaprijed integrirane aplikacije Microsoft i prilagođene aplikacije** To su nezavisne oblaka aplikacije koje ovise o u imeniku interne i mogu raditi neovisno o Azure AD. Pristup te aplikacije omogućena je prema izdavanja do vjerodajnice aplikacije određene mapirati računa aplikacije. Ovisno o mogućnostima aplikacija vjerodajnicu možda tokena vanjski pristup ili korisničko ime i lozinku za račun koja vam je dodijeljena prethodno u aplikaciji.

- **Lokalni aplikacije** Aplikacija objaviti putem proxyja aplikacije Azure AD prvenstveno Omogućivanje pristupa lokalnim aplikacijama. Te aplikacije oslanjate središnje na lokalnu instancu directory kao što je Windows Server Active Directory. Pristup te aplikacije omogućena je prema pokretanje proxy poslužitelja za isporuku sadržaja aplikacije krajnjem korisniku tijekom honoring obaveznu prijavu lokalnog.

Na primjer, ako korisnik spaja tvrtke ili ustanove, morate stvoriti račun za korisnika u Azure AD za primarni postupke za prijavu. Ako je taj korisnik potreban pristup upravljanih aplikacije kao što su Salesforce, morate stvoriti račun za tog korisnika u Salesforce i veza na račun za Azure da biste SSO rad. Kada korisnik napusti vašoj tvrtki ili ustanovi, preporučuje se da biste izbrisali račun za Azure AD i svi računi postoji zamjena u obliku u na IAM sprema aplikacija korisnika imali pristup.

## <a name="access-detection"></a>Otkrivanje programa Access

Moderna tvrtki odjelima često ne uzimaju sve aplikacije za oblak koji se koriste. U kombinaciji s oblaka aplikacija otkrivanje Azure AD nudi rješenje da biste otkrili te aplikacije.

## <a name="account-management"></a>Upravljanje računima

Upravljanje računima za razne aplikacije Najčešći je ručnog procesa obavlja IT ili podršku osobne u tvrtki ili ustanovi. Azure AD potpuno automatsko upravljanje računima preko svih servisnih aplikacija davatelja integriran i tim aplikacijama unaprijed integriran Microsoft automatiziranog korisnika dodjele resursa za podršku ili SAML JIT.

## <a name="automated-user-provisioning"></a>Dodjeljivanje automatiziranog korisnika

Neke aplikacije omogućuje automatizaciju sučelja za stvaranje i uklanjanje (ili deaktivacija) računa. Ako davatelja nudi takvo sučelje, leveraged po Azure AD. Smanjuje radu troškove jer administrativne zadatke dogoditi automatski i poboljšava sigurnost okruženja jer je smanjuje mogućnost od neovlaštenog pristupa.

## <a name="access-management"></a>Upravljanje pristupom

Korištenje Azure AD možete upravljati pristup aplikacije koje koriste pojedinačne ili pravila utemeljenima na dodjele. Možete i delegirati pristup upravljanja odgovarajućim osobama u tvrtki ili ustanovi osiguravanje najbolje nadzor i smanjuje mogućnost teret na službom za korisnike.

## <a name="on-premises-applications"></a>Lokalni aplikacije

Ugrađenoj u aplikaciji proxy omogućuje objavljivanje aplikacija lokalnim korisnicima posljedica su i dosljedni pristupiti doživljaj Moderna oblaka aplikacije i pogodnosti iz Azure AD nadzor, izvješćivanje i sigurnosne mogućnosti.

## <a name="reporting-and-monitoring"></a>Izvješćivanje i praćenje

Azure AD omogućuje unaprijed integrirani izvješćivanje i praćenje mogućnosti koje vam omogućuje da znate tko ima pristup aplikacijama i zapravo koriste ih.

## <a name="related-capabilities"></a>Srodni mogućnosti

S Azure AD možete zaštititi aplikacija s pravilima zrnastog access i unaprijed integrirani MFA. Da biste saznali više o Azure MFA potražite u članku [Azure MFA](https://azure.microsoft.com/services/multi-factor-authentication/).

## <a name="getting-started"></a>Početak rada

Da biste započeli integraciji aplikacije sustava Azure AD, pogledajte [Vodič za integraciju Azure Active Directory s aplikacijama početak rada](active-directory-integrating-applications-getting-started.md).

## <a name="see-also"></a>Vidi također

[Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
