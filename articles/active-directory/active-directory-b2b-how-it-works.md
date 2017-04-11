<properties
   pageTitle="Azure AD B2B suradnju pretpregled: kako to funkcionira | Microsoft Azure"
   description="U članku se opisuje kako Azure Active Directory B2B suradnje podržava više tvrtke odnosa omogućivanjem poslovni partneri za selektivno pristup tvrtke aplikacija"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-how-it-works"></a>Azure AD B2B suradnju pretpregled: načina funkcioniranja tijeka rada
Azure AD B2B suradnje temelji na poziv i iskoristite modela. Navedite adresu e-pošte stranama želite raditi, zajedno s aplikacijama koje želite koristiti. Azure AD im šalje se pozivnicu e-pošte s vezom. Korisnika partnera slijedi vezu i morati prijaviti pomoću njihovih Azure AD računa ili znak za Azure AD novi račun.

1. Administrator sustava poziva partnera korisnicima tako da prenesete [strukturirane .csv datoteke](active-directory-b2b-references-csv-file-format.md) pomoću portala za Azure.
2. Portala šalje pozovite poruke e-pošte za tim korisnicima partnera.
3. Korisnici partnera kliknite vezu u e-pošte, a zatraži prijavite se pomoću vjerodajnica posla (ako takvi stupci već u Azure AD) ili prijavite se kao korisnik Azure AD B2B suradnju.
4. Korisnici partnera vas preusmjerava u aplikaciju oni su pozvani, gdje sada imaju pristup.

## <a name="directory-operations"></a>Operacije direktorija
Korisnici partnera postoji u vašem Azure AD kao vanjski korisnici. To znači administratoru možete dodijeliti licence, dodijelite članstvo u grupi i dodatno dopustiti pristup tvrtke aplikacije pomoću portala za Azure ili pomoću Azure PowerShell poput samo za korisnike u svojoj tvrtki.

Tijekom plaćenu Azure AD pretplate (Basic ili Premium) nije potrebno koristiti Azure AD B2B, korisnicima koji imaju na plaćenu pretplatu za Azure AD (Basic ili Premium) primanje sljedeće dodatne prednosti:

 - Administratori mogu dodijeliti grupama u aplikacije, koja omogućuje za jednostavnije upravljanje pozvani korisničkog pristupa.
 - Administrator klijenta brendiranje služi za brandiranje pozivnicu e-pošte i sučelje otkup pruža više konteksta da biste pozvali partnera korisnike.

## <a name="related-articles"></a>Povezani članci
 Pregled naš članaka na Azure AD B2B suradnje

 - [Što je Azure AD B2B suradnje?](active-directory-b2b-what-is-azure-ad-b2b.md)
 - [Detaljni vodič](active-directory-b2b-detailed-walkthrough.md)
 - [Referenca za oblikovanje CSV datoteke](active-directory-b2b-references-csv-file-format.md)
 - [Tokena oblik vanjskog korisnika](active-directory-b2b-references-external-user-token-format.md)
 - [Promjene atributa objekt vanjskog korisnika](active-directory-b2b-references-external-user-object-attribute-changes.md)
 - [Trenutni pretpregled ograničenja](active-directory-b2b-current-preview-limitations.md)
 - [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
