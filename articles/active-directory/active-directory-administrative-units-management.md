<properties
   pageTitle="Upravljanje administratora jedinice u Azure Active Directory"
   description="Pomoću administratora jedinice za precizniji delegiranje dozvole u servisu Azure Active Directory"
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

# <a name="administrative-units-management-in-azure-ad---public-preview"></a>Upravljanje administratora jedinice u Azure AD - javno pretpregled

Ovaj članak opisuje administrativne jedinice – novi spremnik Azure Active Directory resursa koje je moguće koristiti za prenošenjem administratorske dozvole putem podskupova korisnika i Primjena pravilnika podskupu korisnika. Administrativni jedinice Azure Active Directory, omogućiti središnje administratori za dodjeljivanje dozvola administratorima regionalne ili da biste postavili pravila zrnastog razine.

To je korisno u tvrtkama ili ustanovama s nezavisne odjelima, na primjer, velike university koji se sastoji od više Autonomna školske (tvrtke školi, inženjerska školi i tako dalje) koje su nezavisne međusobno. Takve odjelima imaju vlastiti IT administratore koji kontrolu pristupa, Upravljanje korisnicima i postaviti pravila za svoje dijeljenja. Središnje administratori želite da biste mogli dodijeliti te odjela dozvola administratorima putem korisnika u svoje određeni odjelima. Točnije, u ovom primjeru središnji administrator možete, ako, primjerice, stvoriti administratora jedinica za određeni školu (tvrtke školu) i popuniti samo poslovni korisnici obrazovne ustanove. Zatim središnji administrator tvrtke školske IT osoblju možete dodati u opsegu ulogu drugim riječima, dodijelite IT osoblju od tvrtke školske administratorske dozvole samo putem poslovne administratora jedinice u školi.

> [AZURE.IMPORTANT]
> Možete stvoriti i koristiti administratora jedinice samo ako omogućite Azure Active Directory Premium. Dodatne informacije potražite u članku [Uvod u rad s Azure AD Premium](active-directory-get-started-premium.md).

Iz središnji administrator točku prikaza, administratora jedinica je objekt direktorija koje se mogu stvoriti i popunjen resursi. **U ovom izdanju ove resurse može biti samo korisnici.** Kada stvorite i popunjeno, administratora jedinica mogu se kao opseg granted Ograniči samo putem resursa koji se nalazi u administratora jedinica.

## <a name="managing-administrative-units"></a>Upravljanje administratora jedinice

U ovom izdanju pretpregled možete stvoriti i upravljanje administratora jedinice pomoću cmdleta Azure Active Directory Module za Windows PowerShell.

Dodatne informacije o softverskim preduvjetima i instaliranje modula Azure AD i informacije o modul Azure AD cmdleti za upravljanje administratora jedinice, uključujući sintaksa, parametar opise i primjere, pročitajte članak [Upravljanje Azure AD pomoću komponente Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx).


## <a name="next-steps"></a>Daljnji koraci
[Azure Active Directory izdanja](active-directory-editions.md)
