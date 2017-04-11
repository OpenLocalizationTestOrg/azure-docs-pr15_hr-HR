<properties
   pageTitle="Čarobnjak za sigurnost Azure AD povlaštene upravljanje identitetom"
   description="Prilikom prvog korištenja nastavkom Azure Active Directory povlaštene upravljanje identitetom, primit ćete s Čarobnjak za sigurnost. U ovom se članku opisuju koraci za korištenje čarobnjaka."
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
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>Čarobnjak za sigurnost Azure AD povlaštene upravljanje identitetom

Ako ste prvu osobu da biste pokrenuli Azure povlaštene identiteta Management (PIM) za tvrtku ili ustanovu, prikazat će se upit o čarobnjaka. Čarobnjak olakšava razumijevanje sigurnosni rizik povlaštene identiteta i kako koristiti PIM za smanjivanje tih rizika. Ne morate unesite željene promjene u postojeće dodjele uloga u čarobnjaku ako želite učiniti kasnije.

## <a name="what-to-expect"></a>Što mogu očekivati

Prije pokretanja pomoću PIM vašoj tvrtki ili ustanovi sve dodjele uloga je trajno: korisnici uvijek su u sljedećim ulogama čak i ako se ne trenutno moraju njihove ovlasti.  U prvom koraku čarobnjaka prikazuje popis vrlo velikim ovlastima uloga i koliko korisnika trenutno se te uloge. Omogućuje dubinsku analizu određenu ulogu da biste saznali više o korisnicima ako je jedan ili više njih nisu poznati.

Drugi korak čarobnjaka vam omogućuje da biste promijenili dodjele uloga administratora.  

> [AZURE.WARNING]Nije važno da imaju barem jedan globalni administrator i više od jedne povlaštene ulogu administratora pomoću računa tvrtke ili ustanove (nije Microsoftov račun). Ako vam se prikazuje samo jedan povlaštene ulogu administratora, tvrtke ili ustanove nećete moći upravljati PIM ako je izbrisana taj račun.
> Osim toga, ostavite dodjele uloga trajna ako korisnik ima Microsoftova računa (račun koji se koriste za prijavu na Microsoftove servise kao što su Skype i Outlook.com). Ako planirate zahtijevaju MFA za aktivaciju za uloge, taj korisnik će biti zaključan.


Nakon što ste napravili promjene, čarobnjak više neće prikazivati prema gore. Kada sljedeći put ili drugog povlaštene ulogu administratora koristili PIM, vidjet ćete PIM nadzornu ploču.  

- Ako želite dodati ili ukloniti korisnike iz uloge ili mijenjati dodijeljene iz trajna za uvjete, dodatne informacije potražite u pri [kako dodati ili ukloniti uloge korisnika](active-directory-privileged-identity-management-how-to-add-role-to-user.md).
- Ako želite dati pristup da biste upravljali PIM više korisnika, pročitajte više u [kako omogućiti pristup da biste upravljali u PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md).



## <a name="next-steps"></a>Daljnji koraci
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
