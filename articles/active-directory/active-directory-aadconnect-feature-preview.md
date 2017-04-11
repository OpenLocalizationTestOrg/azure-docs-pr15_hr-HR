<properties
   pageTitle="Azure AD Connect: Značajke u pretpregledu | Microsoft Azure"
   description="U ovoj se temi opisuju u više detalja značajki koje su u pretpregledu u Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"  
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="06/27/2016"
   ms.author="billmath"/>

# <a name="more-details-about-features-in-preview"></a>Dodatne informacije o značajkama u pretpregledu
U ovoj se temi opisuje kako pomoću značajke trenutno u pretpregledu.

## <a name="group-writeback"></a>Grupa upisima
Mogućnost za grupu upisima u dodatne značajke će vam omogućiti da biste upisima **Grupama sustava Office 365** da biste na skup stabala sa sustavom Exchange instaliran. Ovo je grupi koja se uvijek usvojili u oblaku. Ako imate sustava Exchange na lokalno, a zatim možete napisati natrag te grupe na lokalni tako da korisnici s poštanskim sandučićem sustava Exchange na lokalni možete slati i primati poruke e-pošte iz te grupe.

Dodatne informacije o grupama sustava Office 365 i kako ih koristiti nalazi se [ovdje](http://aka.ms/O365g).

Ovu grupu bit će predstavljena kao grupu za raspodjelu u lokalnoj AD DS. Lokalnog poslužitelja za Exchange mora biti na web-mjesto sustava Exchange 2013 Kumulativno ažuriranje 8 (objavljeno u ožujku 2015) ili u okvir za Exchange 2016 prepoznati novu vrstu grupe.

**Bilježaka tijekom pretpregleda**

- Atribut adresara adresu trenutno nije popunjeno u pretpregledu. Bez taj atribut grupi neće biti vidljiv na globalnom popisu adresa. Najjednostavniji način za popunjavanje taj atribut je pomoću cmdleta ljuske PowerShell sustava Exchange `update-recipient`.
- Samo šuma sa shemom Exchange nisu valjani ciljnih web-mjesta za grupe. Ako je otkriven nijedan sustava Exchange, zatim upisima grupe neće biti moguće omogućiti.
- Trenutno podržava samo jedan-skupa stabala implementacije organizacije sustava Exchange. Ako imate više od jedne sustava Exchange tvrtke ili ustanove na lokalno, pa ćete rješenje GALSync lokalnog te grupe da se prikazuje u vašem šuma.
- Značajka upisima grupe trenutno ne rukovati sigurnosnim grupama i grupama za raspodjelu.

>[AZURE.NOTE] Pretplatu na Azure AD Premium za grupu upisima potreban je.

## <a name="user-writeback"></a>Korisnik upisima
> [AZURE.IMPORTANT] Uklonjena značajka pretpregleda upisima korisnika u kolovozu 2015 Ažuriraj Azure AD Connect. Ako to omogućila, trebali biste onemogućili tu značajku.

## <a name="next-steps"></a>Daljnji koraci
Nastavite [prilagođenu instalaciju programa Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
