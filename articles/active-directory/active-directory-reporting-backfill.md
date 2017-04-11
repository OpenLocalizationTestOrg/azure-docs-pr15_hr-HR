<properties
   pageTitle="Azure Active Directory puta backfill izvješća | Microsoft Azure"
   description="Vrijeme potrebno za prethodni izvješćivanja događaja koji se prikazuju u direktoriju Azure AD"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="stevepo"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-report-backfill-times"></a>Azure Active Directory izvješće backfill vremena

*Ovo je dokumentacija je dio [Azure Active Directory izvješćivanja vodič](active-directory-reporting-guide.md).*

Nakon direktorij uključila u izvješćima, backfill podaci izvješća za određeni broj dana, naznačen ovdje.

Izvješće                                                  | Opis
------------------------------------------------------- | -----------
Znak dodataka iz nepoznatih izvora                           | 30 dana
Znak dodataka nakon većeg broja neuspješnih pokušaja                        | 30 dana
Znak dodataka iz više geographies                      | 30 dana
Znak dodataka iz IP adresa pomoću sumnjivoj aktivnosti     | 30 dana
Znak dodataka vjerojatno zaražene uređaja                 | 30 dana
Nepravilni aktivnosti za prijavu                              | 30 dana
Korisnici s anomalous aktivnosti za prijavu                   | 30 dana
Korisnici s licenciranjem vjerodajnice                           | 30 dana
Izvješće o nadzoru                                            | 30 dana
Aktivnost (Azure AD) za ponovno postavljanje lozinke                      | 30 dana
Aktivnost (Upravitelj identiteta) za ponovno postavljanje lozinke              | 30 dana
Registracija aktivnosti (Azure AD) za ponovno postavljanje lozinke         | 30 dana
Registracija aktivnosti (Upravitelj identiteta) za ponovno postavljanje lozinke | 30 dana
Samoposlužni grupe aktivnosti (Azure AD)                 | 30 dana
Samoposlužni grupe aktivnosti (Upravitelj identiteta)         | 30 dana
Korištenje aplikacije                                       | 30 dana
Račun dodjeljivanje aktivnosti                           | 30 dana
Status dinamične lozinke                                | 30 dana
Račun pogreške u dodjeljivanju                             | 30 dana
Korištenje RMS-a                                               | 30 dana
Većina aktivni korisnici RMS-a                                   | 30 dana
Korištenje uređaja RMS-a                                        | 30 dana
Korištenje aplikacije RMS omogućeno                           | 30 dana
