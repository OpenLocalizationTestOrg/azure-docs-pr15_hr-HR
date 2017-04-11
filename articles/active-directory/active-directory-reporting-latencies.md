<properties
   pageTitle="Izvješćivanje o pogreškama Latencies Azure Active Directory | Microsoft Azure"
   description="Vrijeme potrebno za izvješćivanje događaje prikazivati Azure Active Directory"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="azure-active-directory-report-latencies"></a>Latencies izvješće Azure Active Directory

*Ovo je dokumentacija je dio [Azure Active Directory izvješćivanja vodič](active-directory-reporting-guide.md).*

Izvješće                                                  | Minimum  | Prosjek    | Najviše
------------------------------------------------------- | -------- | ---------- | ----------
**Izvješća o sigurnosti**                                    |          |            |
Nepravilni aktivnosti za prijavu                              | dva sata  | četiri sata    | osam sati
Znak dodataka iz nepoznatih izvora                           | dva sata  | četiri sata    | osam sati
Znak dodataka nakon većeg broja neuspješnih pokušaja                        | dva sata  | četiri sata    | osam sati
Znak dodataka iz više geographies                      | dva sata  | četiri sata    | osam sati
Znak dodataka iz IP adresa pomoću sumnjivoj aktivnosti     | dva sata  | četiri sata    | osam sati
Znak dodataka vjerojatno zaražene uređaja                 | dva sata  | četiri sata    | osam sati
Korisnici s anomalous aktivnosti za prijavu                   | dva sata  | četiri sata    | osam sati
Korisnici s licenciranjem vjerodajnice                           | dva sata  | četiri sata    | osam sati
**Aplikacija izvješća**                                 |          |            |
Račun dodjeljivanje aktivnosti                           | dva sata  | četiri sata    | osam sati
Račun pogreške u dodjeljivanju                             | dva sata  | četiri sata    | osam sati
Korištenje aplikacije                                       | dva sata  | četiri sata    | osam sati
Status dinamične lozinke                                | dva sata  | četiri sata    | osam sati
**Nadzora i izvješća o aktivnosti**                            |          |            |
Izvješće o nadzoru                                            | 1 min | 15 minuta | 30 minuta
Aktivnost (Azure AD) za ponovno postavljanje lozinke                      | dva sata  | četiri sata    | osam sati
Aktivnost (Upravitelj identiteta) za ponovno postavljanje lozinke              | dva sata  | četiri sata    | osam sati
Registracija aktivnosti (Azure AD) za ponovno postavljanje lozinke         | dva sata  | četiri sata    | osam sati
Registracija aktivnosti (Upravitelj identiteta) za ponovno postavljanje lozinke | dva sata  | četiri sata    | osam sati
Samoposlužni grupe aktivnosti (Azure AD)                 | dva sata  | četiri sata    | osam sati
Samoposlužni grupe aktivnosti (Upravitelj identiteta)         | dva sata  | četiri sata    | osam sati
**RMS izvješća**                                         |          |            |
Većina aktivni korisnici RMS-a                                   | dva sata  | četiri sata    | osam sati
Korištenje RMS-a                                               | dva sata  | četiri sata    | osam sati
Korištenje uređaja RMS-a                                        | dva sata  | četiri sata    | osam sati
Korištenje aplikacije RMS omogućeno                           | dva sata  | četiri sata    | osam sati
**Privatna pretpregled izvješća**                             |          |            |
Sve aktivnosti za korisničku prijavu                               | dva sata  | četiri sata    | osam sati
