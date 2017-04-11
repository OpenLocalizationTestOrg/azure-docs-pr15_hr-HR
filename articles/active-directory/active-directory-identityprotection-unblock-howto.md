<properties
    pageTitle="Azure Active Directory identiteta zaštita – upute za Omogućivanje korisnicima | Microsoft Azure"
    description="Saznajte kako deblokirati korisnika koje je blokirala pravilo Azure Active Directory identiteta zaštitu."
    services="active-directory"
    keywords="Zaštita identiteta servisa Azure active directory, deblokiranje korisnika"
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
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>Azure Active Directory identiteta zaštita – upute za Omogućivanje korisnicima

Sa Azure Active Directory identiteta, možete konfigurirati pravila blokirati korisnike ako su zadovoljena konfigurirani uvjeta. Obično je blokirani korisnika kontakata službi za podršku postati deblokiran. Ovaj članak objašnjava korake možete izvršiti za deblokiranje blokirani korisnika.


## <a name="determine-the-reason-for-blocking"></a>Određivanje razlog za blokiranje

Kao prvi korak za deblokiranje korisnika, morate odrediti vrstu pravila koja je blokirao korisnik jer su sljedeće korake ovisno o ga. Azure Active Directory identiteta zaštita korisnika mogu se ili blokirati pravilo odgovornost za prijavu ili korisnička pravila rizika. 

Prikazat će vam vrste pravila koja je blokirao korisnika iz zaglavlja u dijaloškom okviru je unesen korisniku tijekom pokušaja prijave:

|Pravila | Dijaloški okvir korisnika|
|--- | --- |
|Rizik za prijavu | ![Blokirani prijava](./media/active-directory-identityprotection-unblock-howto/02.png) |
|Rizik korisnika | ![Blokiranog računa](./media/active-directory-identityprotection-unblock-howto/104.png) |


Korisnik koji je blokirao je:

- Je poznata i kao sumnjiva prijava pravilo odgovornost za prijavu
- Korisnička pravila rizika je poznata i kao račun rizik

 
## <a name="unblocking-suspicious-sign-ins"></a>Deblokiranjem sumnjiva znak dodataka

Da biste Deblokiraj na sumnjiva prijavu, imate sljedeće mogućnosti:

1. **Prijava s poznatih mjesto ili uređaja** – uobičajeni razlog blokiranih sumnjiva znak dodataka su pokušaja prijave s nepoznatim mjesta ili uređaja. Korisnici brzo možete odrediti hoće li to je blokiranja razlog da se pokušava prijaviti s poznatih mjesto ili uređaja.


3. **Izuzimanje iz pravila** – ako mislite da je trenutna konfiguracija pravila za prijavu uzrokuje probleme s za određene korisnike, možete isključiti korisnike iz nje. [Pravila odgovornost za prijavu](active-directory-identityprotection.md#sign-in-risk-policy) dodatne pojedinosti potražite u članku.
 
4. **Onemogućivanje pravila** – ako mislite da je pravilnik o konfiguraciji uzrokuje probleme s za sve korisnike, možete onemogućiti pravila. [Pravila odgovornost za prijavu](active-directory-identityprotection.md#sign-in-risk-policy) dodatne pojedinosti potražite u članku.


## <a name="unblocking-accounts-at-risk"></a>Deblokiranjem računi rizik

Da biste deblokiranje račun rizik, imate sljedeće mogućnosti:

1. **Ponovno postavljanje lozinke** – možete ponovno postaviti korisničku lozinku. U odjeljku [ponovno postavljanje lozinke za ručno sigurne](active-directory-identityprotection.md#manual-secure-password-reset) više pojedinosti.

2. **Odbaci sve događaje rizika** - korisnička pravila rizika blokira korisnika ako postane razinu rizika konfigurirani korisnika za blokiranje pristupa. Možete smanjiti korisnik je razina rizika zatvaranjem ručno prijavljenih rizika događaja. Dodatne informacije potražite u članku [ručno zatvaranja rizika događaja](active-directory-identityprotection.md#closing-risk-events-manually).

3. **Izuzimanje iz pravila** – ako mislite da je trenutna konfiguracija pravila za prijavu uzrokuje probleme s za određene korisnike, možete isključiti korisnike iz nje. [Korisnička rizika pravila](active-directory-identityprotection.md#user-risk-policy) dodatne pojedinosti potražite u članku.
 
4. **Onemogućivanje pravila** – ako mislite da je pravilnik o konfiguraciji uzrokuje probleme s za sve korisnike, možete onemogućiti pravila. [Korisnička rizika pravila](active-directory-identityprotection.md#user-risk-policy) dodatne pojedinosti potražite u članku.




## <a name="next-steps"></a>Daljnji koraci

 Želite li saznati više o zaštitu Azure AD identiteta? Pogledajte [Azure Active Directory identiteta zaštitu](active-directory-identityprotection.md).
 

