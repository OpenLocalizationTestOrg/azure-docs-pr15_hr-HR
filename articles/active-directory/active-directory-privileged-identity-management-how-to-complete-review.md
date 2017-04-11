<properties
   pageTitle="Upute za dovršetak kritike pristup | Microsoft Azure"
   description="Nakon što ste započeli pregled programa access u Azure AD povlaštene upravljanje identitetom Naučite dovršavanja i prikaz rezultata"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="06/30/2016"
   ms.author="kgremban"/>

# <a name="how-to-complete-an-access-review-in-azure-ad-privileged-identity-management"></a>Upute za dovršetak pregled programa access u Azure AD povlaštene upravljanje identitetom


Povlaštene ulogu administratora možete pregledati povlaštene pristup jednom je [počeo pregled sigurnost](active-directory-privileged-identity-management-how-to-start-security-review.md). Azure AD povlaštene identiteta Management (PIM) će automatski poslati poruku e-pošte na pitanja korisnika da biste pregledali njihov pristup. Ako korisnik jeste li dobili poruku e-pošte, možete im poslati upute u [kako izvesti sigurnost pregled](active-directory-privileged-identity-management-how-to-perform-security-review.md).

Nakon razdoblje Pregled sigurnosti ili sve korisnike dovršite njihove koja se sama pregled, slijedite korake u ovom se članku Upravljanje pregled i prikazali rezultate.

## <a name="manage-security-reviews"></a>Upravljanje recenzije sigurnosti

1. Idite na [portal za Azure](https://portal.azure.com/) i odaberite aplikaciju za **Azure AD povlaštene upravljanje identitetom** na nadzornu ploču.
2. Odaberite sekciju **Access pregledava** nadzorne ploče.
3. Odaberite pregled access koju želite upravljati.

Na pregled pristup detalja plohu dostupne su broj mogućnosti za upravljanje tom pregled.

![Pregled gumbi pristup PIM - snimka][1]

### <a name="remind"></a>Kao podsjetnik

Ako kritike pristupa postavljen tako da korisnici sami pregled, gumb **Remind** šalje obavijest. 

### <a name="stop"></a>Zaustavi

Pregledi svi pristup su završni datum, ali možete koristiti gumb **Zaustavi** da biste ga ranijeg završetka. Ako niste po ovaj put pregledan sve korisnike, oni nećete moći kada prekinete pregled. Kada je zaustavljena ne možete ponovno pokrenuti pregleda.

### <a name="apply"></a>Primjena

Po dovršetku pregled programa access ili jer dosegne završni datum ili ga ručno zaustavio gumb **Primijeni** implementira rezultat pregled. Ako je korisnički pristup odbijen na stranici pregled, to je korak koji će se ukloniti njihov Dodjela uloge.  

### <a name="export"></a>Izvoz

Ako želite da biste primijenili ručno rezultate pregleda sigurnost, možete izvesti na pregled. Gumb **Izvoz** započet će preuzimanje u CSV datoteku. Možete upravljati rezultate u programu Excel ili druge programe koji se otvori CSV datoteke.

### <a name="delete"></a>Brisanje

Ako niste u pregled izvući, izbrišite ga. Gumb **Izbriši** uklanja pregled iz aplikacije PIM.

> [AZURE.IMPORTANT] Će ne prikazuje se upozorenje prije brisanja pojavljuje, stoga svakako koju želite izbrisati tu pregled.


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]


<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-complete-review/PIM_review_buttons.png
