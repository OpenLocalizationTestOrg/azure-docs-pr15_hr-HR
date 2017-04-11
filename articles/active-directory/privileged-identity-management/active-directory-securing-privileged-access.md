<properties
    pageTitle="Zaštita povlaštene programa Access u Azure AD | Microsoft Azure"
    description="Tema na kojem se objašnjava postupke za osiguravanje povlaštene pristup svim Azure, Azure Active Directory i Microsoft Online Services."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="mwahl"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="kgremban"/>


# <a name="securing-privileged-access-in-azure-ad"></a>Zaštita povlaštene programa access u Azure AD

Zaštita povlaštene pristup je ključnih prvi korak da biste zaštitili poslovnih sredstava Moderna tvrtke ili ustanove. Povlaštene računi su oni koje upravljati i upravljanje IT sustava. Cyber attackers ciljne tih računa da biste pristupili podataka i sustava tvrtki ili ustanovi. Da bi se osigurala povlaštene pristup treba izdvojiti računi i sustavi iz rizik od izlaganja zlonamjernom korisniku.

Da biste dobili povlaštene pristup putem oblaka usluga pokretanja više korisnika. Možete uključiti globalni administratori sustava Office 365, Azure pretplate administratorima i korisnicima koji imaju Administrativni pristup u VMs ili na SaaS aplikacija.

Microsoft preporučuje slijediti ovaj vodič [Zaštita povlaštene](https://technet.microsoft.com/library/mt631194.aspx)pristup.

Za korisnike koji se koriste Azure Active Directory za upravljanje pristupom Azure, Office 365 ili druge Microsoftove servise i aplikacije, tih načela primjenjuju bez obzira korisničke račune se upravlja i autentičnost servisa Active Directory ili servisa Azure Active Directory. Sljedeći odjeljci sadrže dodatne informacije o značajkama Azure AD za podršku zaštita povlaštene pristup.

## <a name="multi-factor-authentication"></a>Višestruka provjera autentičnosti

Da biste povećali sigurnost provjere autentičnosti administratora, zahtijevaju višestruke provjere autentičnosti (MFA) prije nego date ovlasti. MFA je način provjera koji su koji zahtijeva korištenje više od samo korisničko ime i lozinku. Pruža drugu razinu zaštite znak dodaci za korisnika i transakcije.

Azure višestruka provjera autentičnosti olakšava zaštitu pristupa s podacima i aplikacijama tijekom sastanka korisnika zahtjev za jednostavne postupak prijave. On nudi Jaka provjera autentičnosti putem raspon jednostavno Provjera mogućnosti, uključujući telefonski pozivi, poruke s tekstom, obavijesti za mobilne aplikacije ili kod za potvrdu i 3 tokena OATH proizvođača.

Pregled funkcioniranje Azure višestruka provjera autentičnosti potražite u članku ovom videozapisu.

>[AZURE.VIDEO windows-azure-multi-factor-authentication]

Dodatne informacije potražite u članku [MFA za Office 365 i MFA za Azure](https://blogs.technet.microsoft.com/ad/2014/02/11/mfa-for-office-365-and-mfa-for-azure/).

## <a name="time-bound-privileges"></a>Vrijeme granica ovlasti

Neke tvrtke ili ustanove može se dogoditi da imaju previše korisnika korisnik s velikim ovlastima uloge. Korisnik možda su dodani u ulogu za određene aktivnosti, kao što je da biste se registrirali za uslugu, ali niste koristili te ovlasti često kasnije.

Da biste donji vrijeme za izlaganje ovlasti i povećanje vidljivosti u njihova upotreba, ograničite korisnicima samo poduzimanja na ovlasti samo u vrijeme ČAS, kada koje su im potrebne za izvođenje zadatka. Azure Active Directory i Microsoft Online Services, možete koristiti [Azure AD povlaštene upravljanje identitetom (PIM)](http://aka.ms/AzurePIM).


![PIM nadzorne ploče][2]


## <a name="attack-detection"></a>Otkrivanje napadi

[Azure Active Directory identiteta zaštita](../active-directory-identityprotection.md) nudi konsolidirani prikaz u događaje rizik i potencijalne slabe točke utjecaja identiteta vaše tvrtke ili ustanove. Na temelju rizika događaje identiteta zaštita izračunava razinu rizika korisnika za svakog korisnika, što vam omogućuje da konfiguriranje pravilnika o utemeljen na rizika automatski zaštitu identitet tvrtke ili ustanove. Ta pravila, zajedno s drugim uvjetno pristup kontrolama za Azure Active Directory i EMS, možete automatski blokirati korisnika ili prijedloge koji sadrže lozinki i provođenje višestruke provjere autentičnosti.

![Zaštita Azure AD identiteta][3]

## <a name="conditional-access"></a>Uvjetno programa access

S kontrolom uvjetno pristup Azure Active Directory provjerava određenim uvjetima koje ste odabrali prilikom provjere autentičnosti korisnika, prije omogućivanja pristupa u aplikaciju. Kada su ti uvjeti zadovoljeni, korisnik je autentičnost provjerena i dopuštenje za pristup aplikaciji.


![Postavljanje pravila za uvjetno pristup s MFA][4]


## <a name="related-articles"></a>Povezani članci

- Omogućivanje [Azure višestruka provjera autentičnosti](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)
- Omogući [Upravljanje identitetom povlaštene Azure AD](../active-directory-privileged-identity-management-configure.md)
- Omogući [zaštitu Azure AD identiteta](../active-directory-identityprotection.md)
- Omogućivanje [pristupa uvjetno kontrola](../active-directory-conditional-access.md)


Dodatne informacije o izgradnji Potpuna sigurnost vodič, potražite u odjeljku "obaveze klijenta i vodič" [Microsoft Cloud Security za Enterprise projektantima](http://aka.ms/securecustomer) dokumenta. Dodatne informacije o zanimljivih Microsoftove servise kao pomoć u bilo kojem od ovih tema, zatražite od Microsoftova predstavnika ili posjetite naš [Cybersecurity rješenja stranice](https://www.microsoft.com/microsoftservices/campaigns/cybersecurity-protection.aspx).

<!--Image references-->
[1]: ../media/active-directory-privileged-identity-management-configure/Search_PIM.png
[2]: ../media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ../media/active-directory-identityprotection/29.png
[4]: ../media/active-directory-conditional-access/conditionalaccess-saas-apps.png
