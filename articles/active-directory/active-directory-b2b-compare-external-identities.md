<properties
   pageTitle="Usporedba mogućnosti za upravljanje vanjskim identiteta pomoću servisa Azure Active Directory | Microsoft Azure"
   description="Uspoređuje Azure Active Directory B2B suradnje, B2C i više klijentske aplikacije za provjeru autentičnosti i ovlaštenja za podršku za vanjske identiteta"
   services="active-directory"
   documentationCenter="" 
   authors="arvindsuthar"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="02/24/2016"
   ms.author="asuthar"/>

# <a name="comparing-capabilities-for-managing-external-identities-using-azure-active-directory"></a>Usporedba mogućnosti za upravljanje vanjskim identiteta pomoću servisa Azure Active Directory

Uz upravljanje pristupom snage mobilne aplikacije SaaS, Azure Active Directory (Azure AD) može pridonijeti vašoj tvrtki ili ustanovi Omogućivanje zajedničkog korištenja resursa poslovni partneri i isporučiti aplikacije za tvrtke i korisnici.

## <a name="developing-applications-for-businesses"></a>Razvoj aplikacija za tvrtke

Ne nude servisa i aplikacije, kao što je servis za podatkovne veze za tvrtke? Azure AD nudi identiteta platformu koja omogućuje sastavljanje aplikacije jednostavno integrirane milijune tvrtkama ili ustanovama koje ste već konfigurirali Azure AD kao dio implementacija sustava Office 365 ili druge servise za enterprise.

**Primjer:** Farmaceutskog distributer nudi medicinsku opremu i informacije sustavi zdravstvene grana. Su potrebne za polja aplikaciju analize mjehuričaste prakse i djelatnike klijentima da biste upravljali vlastite identiteta. U ovom tvrtke odabrali Azure AD kao platformu identiteta za aplikaciju za upravljanje njihove vježbe pruža identitete Azure AD svojim korisnicima znak "at" kopija kada je to potrebno. Dodatne informacije potražite u članku [Vodič za Azure Active Directory programiranje](active-directory-developers-guide.md).

## <a name="enabling-business-partner-access-to-your-corporate-resources"></a>Omogućivanje pristup partnera tvrtke resursi za tvrtke

Jeste li poslovni partneri i drugim korisnicima izvan tvrtke koje moraju imati pristup poslovni resursi, kao što je web-mjesta sustava SharePoint ili sustavu ERP? Azure AD omogućuje administratorima da biste dodijelili vanjske korisnike (možda ili ne postoji u Azure AD) s jednom prijava pristup aplikacijama za tvrtke. To poboljšava sigurnost kao korisnici pristup izgubiti kada napuste partner tvrtke ili ustanove, a vi upravljate pravilnike za pristup unutar tvrtke ili ustanove. To također pojednostavljuje administracije kao što je ne morate upravljanje vanjskim partnera imeničkog ili po odnosi vanjski pristup partnera.

**Primjer:** Slike tvrtke nudi trgovaca na malo s fotografijom services za obradu slike i pristajete najveću maloprodaja mreža ispisa kiosks. Oni odabrali Azure AD da biste omogućili tisuće korisnicima na njihove maloprodaja poslovni partneri da biste koristili vlastiti vjerodajnice da biste preuzeli najnovije marketinške materijale partnera i Promjena redoslijeda fotografiju obradu zalihama tvrtke dobavljača ekstranet. Dodatne informacije potražite u članku [Azure AD B2B suradnje](active-directory-b2b-what-is-azure-ad-b2b.md).

## <a name="developing-applications-for-consumers"></a>Razvoj aplikacija za koje korisnici

Morate li sigurno i isplativo objavljivanje online aplikacija, kao što je naprijed spremište Maloprodaja, milijuni koje korisnici? Azure AD nudi platformu Omogućivanje društvenih te prijavu korisničkog imena i lozinke, brandiranog samostalne Registracija i samostalno ponovno postavljanje lozinke za koje korisnici aplikacije. To povećava praktičnost vaše kupcima tijekom smanjivanje opterećenje vaše razvojnim inženjerima.

**Primjer:** Na \#1 sportaša franšizne u svijetu potrebne za izravno sudjelovati s njegova sporta 450 milijuna. Da biste to učinili, ugrađeni su mobilne aplikacije pomoću Azure AD za pohranu za provjeru autentičnosti i profila korisnika. Sporta dobiti pojednostavljeni registraciju i prijavu pomoću koristite računi društvenih kao što su Facebook ili mogu koristiti klasične korisničkog imena i lozinke za objedinjenog sučelje preko iOS, Android i Windows telefoni. Stvaranje na uspostaviti platforme Azure AD znatno smanjiti prilagođeni kod tijekom održavanja na franšizne prilagođenog brendiranja i alleviating opasnosti o sigurnosti, breaches podataka i skalabilnost. Dodatne informacije potražite u članku [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/).

## <a name="comparison-of-azure-ad-capabilities"></a>Usporedba Azure AD sposobnosti

Svaki od scenariji već se spominju u ovom članku je adresirane po mogućnostima Azure AD. U ovoj su tablici trebali biste pojasnili su najrelevantniji na koje mogućnosti:

| **Razmislite o ovim proizvodom...**       | [Azure AD više klijentu SaaS aplikacije](active-directory-developers-guide.md)    | [Azure AD B2B suradnje](active-directory-b2b-what-is-azure-ad-b2b.md)        | [Azure AD B2C](https://azure.microsoft.com/documentation/services/active-directory-b2c/)                |
|-----------------------|-------------------------|----------------------------|------------------------|
| **Ako morate unijeti...** | Servis za tvrtke | pristup partnera Moje aplikacije  | Servis za koje korisnici |
| **I sam slično...**  | Distributer Pharma      | Tvrtke za obradu slike            | Sportovi franšizne       |
| **Implementacija aplikacije za...**  | Upravljanje vježbe     | Dobavljača ekstranet          | Nogomet sporta            |
| **Određivanje...**        | Liječničke ureda        | Odobrene poslovni partneri | Svi korisnici s e-pošte      |
| **Dostupne kada...**      | Identifikacijskom administrator klijenta | Moje pozivnice za administratore           | Korisnik registrira      |
