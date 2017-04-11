<properties
   pageTitle="Dodavanje i upravljanje njima više Azure Active Directory direktorija | Microsoft Azure"
   description="Upute i najbolje prakse za dodavanje i upravljanje Azure Active Directory direktorija, u kojoj se navodi direktorija kao potpuno neovisno resursa"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="add-and-manage-multiple-azure-active-directory-directories"></a>Dodavanje i upravljanje njima više direktorija Azure Active Directory

U Azure Active Directory (Azure AD), svaki imenik nije potpuno nezavisne resursa: ravnopravnih članova, potpuno značajkama i logički neovisno o drugim direktorija kojima upravljate. Nema nadređenih i podređenih odnosa između direktorija. U ovom nezavisnosti između direktorija obuhvaća nezavisnosti resursa, administratora nezavisnosti i sinkronizacija nezavisnosti.

##<a name="resource-independence"></a>Nezavisnosti resursa

Ako je stvaranje ili brisanje resursa u jedan direktorija, ima bez utjecaja na bilo kojeg resursa u neki drugi direktorij, uz djelomično iznimku vanjskih korisnika, što je opisano ispod. Ako koristite naziv prilagođene domene "contoso.com" s jednog direktorija, nije moguće koristiti s drugim direktorija.

##<a name="administrative-independence"></a>Administrativni nezavisnosti

Ako nisu administratora korisnik direktorija "Contoso" stvara direktorij test 'Test' pa:
- Prema zadanim postavkama korisnika koji stvara direktorij je dodani kao vanjskog korisnika u taj novi direktorij pa dodijeljena uloga globalnog administratora u tom direktoriju.
- Administratori direktorija "Contoso" imati bez Izravni administratorske ovlasti direktorij 'Test' osim ako administrator 'Test' posebno im daje ovlasti. Administratori "Contoso" možete kontrolirati pristup direktoriju 'Test' ako oni kontrolirati korisnički račun koji je stvorio 'Test'.
- Ako promijenite (Dodavanje ili uklanjanje) ulogu administratora za korisnika u imeniku jedan promjena utjecati korisnik može imati u drugom imeniku administratorsku ulogu.

##<a name="synchronization-independence"></a>Sinkronizacija nezavisnosti

Možete konfigurirati svaki imenik Azure AD neovisno za dohvaćanje podataka koji se sinkroniziraju iz jednu instancu nešto od sljedećeg:
  - Sinkroniziranje direktorija (DirSync) alat za sinkronizaciju podataka s jednog skupa stabala AD.
  - Azure Active Directory poveznik za Forefront Upravitelj identiteta, Sinkronizacija podataka s jednog ili više lokalnog šuma, i/ili izvora podataka za koje nisu Azure AD.

##<a name="add-an-azure-ad-directory"></a>Dodavanje u imeniku Azure AD

Da biste dodali u imeniku Azure AD na portalu za Azure klasični, odaberite proširenje Azure Active Directory na lijevoj strani, a zatim dodirnite **Dodaj**.

> [AZURE.NOTE]   Za razliku od drugih Azure resursa na direktorija nisu podređeni resursi Azure pretplate. Ako otkazivanje ili omogućuju pretplatu Azure istječe, i dalje možete pristupiti direktorija podatke pomoću Azure PowerShell, Azure grafikonu API-JA ili druge sučelja kao što je centar za administratore sustava Office 365. Možete povezati i drugu pretplatu s imenikom servisa.

Općenite pregled Azure AD licenciranja problema i najbolje prakse potražite u članku [što je Azure Active Directory licenciranje?](active-directory-licensing-what-is.md).
