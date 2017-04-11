<properties
    pageTitle="Slabe točke otkrio Azure Active Directory identiteta zaštita | Microsoft Azure"
    description="Pregled slabe točke otkrio Azure Active Directory identiteta zaštitu."
    services="active-directory"
    keywords="Zaštita identiteta Azure active directory, otkrivanje aplikacije oblaka, Upravljanje aplikacijama, sigurnost, rizika, razina rizika, slabe, sigurnosna pravila"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="vulnerabilities-detected-by-azure-active-directory-identity-protection"></a>Slabe točke otkrio Azure Active Directory identiteta zaštitu 

Slabe točke su slabosti u okruženju sustava mogu napasti napadaču. Preporučujemo adresa te slabe točke za poboljšanje sigurnosti posture vaše tvrtke ili ustanove i spriječili exploiting ih attackers.
<br><br>
![Slabe točke](./media/active-directory-identityprotection-vulnerabilities/101.png "vulnerabilities")
<br>

Sljedeći odjeljci sadrže pregled slabe točke dojavi identiteta zaštitu.

## <a name="multi-factor-authentication-registration-not-configured"></a>Višestruka provjera autentičnosti Registracija nije konfigurirano 

U ovom slabe pomaže u kontrolu za implementaciju Azure višestruke provjere autentičnosti u tvrtki ili ustanovi. 

Azure višestruke provjere autentičnosti omogućuje drugu razinu zaštite za provjeru autentičnosti korisnika. On olakšava zaštitili pristupa podacima i aplikacijama dok korisnik zahtjev za jednostavne postupak prijave za sastanak. On nudi Jaka provjera autentičnosti putem raspon mogućnosti jednostavno provjere – telefonski poziv, tekstne poruke ili mobilne aplikacije obavijesti ili potvrdu Šifra i 3 strana OATH tokeni.

Preporučujemo da zahtijevate Azure višestruke provjere autentičnosti za prijavu dodaci za korisnika. Višestruka provjera autentičnosti reproducira ulogu za pristup utemeljen na rizika uvjetnog pravila dostupne putem identiteta zaštitu.

Dodatne informacije potražite u članku [što je Azure višestruka provjera autentičnosti?](../multi-factor-authentication/multi-factor-authentication.md)


## <a name="unmanaged-cloud-apps"></a>Aplikacija neupravljani oblaka

U ovom slabe olakšava prepoznali neupravljani oblaka aplikacije u tvrtki ili ustanovi.
 
Moderna tvrtki odjelima često su svjesni aplikacija oblaka korisnika u tvrtki ili ustanovi koristite za izvođenje zadataka. Da biste vidjeli zašto administratori promijenile opasnosti o neovlašteni pristup podataka tvrtke, oštećenja mogućih podataka i drugih sigurnosni rizik jednostavno je. 

Preporučujemo da vašoj tvrtki ili ustanovi implementacija oblaka aplikacije otkrivanje da biste otkrili neupravljani oblaka aplikacije, a da biste upravljali te aplikacije pomoću servisa Azure Active Directory.

Dodatne informacije potražite u članku [Pronalaženje neupravljani oblaka aplikacije s otkrivanje aplikacije oblaka](active-directory-cloudappdiscovery-whatis.md).



##<a name="security-alerts-from-privileged-identity-management"></a>Sigurnosnih upozorenja iz upravljanje identitetom povlaštene

U ovom slabe pomaže vam otkrivanje i riješili upozorenja o povlaštene identiteta u tvrtki ili ustanovi.  

Da biste korisnicima omogućili da izvršavanje povlaštene operacije, tvrtkama ili ustanovama morate dopustiti korisnicima privremena ili stalna povlaštene pristup u Azure AD resursi Azure ili Office 365 ili drugih aplikacija SaaS. Svaki od tih povlaštene korisnike povećava plošni napada vaše tvrtke ili ustanove. U ovom slabe pomaže vam identificiranje korisnika s nepotrebnih povlaštene pristupom i poduzeti da biste uklonili mogu predstavljati sigurnosni rizik. 

Preporučujemo da se vaša tvrtka ili ustanova koristi Azure AD povlaštene upravljanje identitetom da biste upravljali, kontrole, a monitor ovlasti identiteta i njihovih pristupa resursima servisa Azure AD kao i druge Microsoftove internetske servise kao što su Office 365 ili Microsoft Intune.

Dodatne informacije potražite u članku [Azure AD povlaštene upravljanje identitetom](active-directory-privileged-identity-management-configure.md). 



## <a name="see-also"></a>Vidi također

 - [Zaštita identiteta Azure Active Directory](active-directory-identityprotection.md)
