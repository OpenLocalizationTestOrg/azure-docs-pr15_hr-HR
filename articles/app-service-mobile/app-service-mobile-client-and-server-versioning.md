<properties
  pageTitle="Postupak klijentske i poslužiteljske SDK rada s verzijama na mobilna aplikacija i servisa Mobile | Aplikacije servisa za Azure"
  description="Popisa klijenata SDK-ovi i kompatibilnost s verzijama SDK poslužitelja za mobilne usluge i Azure mobilne aplikacije"
  services="app-service\mobile"
  documentationCenter=""
  authors="adrianhall"
  manager="erikre"
  editor=""/>

<tags
  ms.service="app-service-mobile"
  ms.workload="mobile"
  ms.tgt_pltfrm="mobile-multiple"
  ms.devlang="dotnet"
  ms.topic="article"
  ms.date="10/01/2016"
  ms.author="adrianha"/>

# <a name="client-and-server-versioning-in-mobile-apps-and-mobile-services"></a>Postupak klijentske i poslužiteljske rada s verzijama na mobilna aplikacija i servisa Mobile

Najnoviju verziju servisa Azure Mobile je značajka **Mobilne aplikacije** servisa Azure aplikacije.

U aplikacijama Mobile postupak klijentske i poslužiteljske SDK-ovi koji su izvorno temelje se na one u mobilne usluge, ali su *nije* kompatibilna međusobno.
To jest, morate koristiti *Mobilne aplikacije* klijenta SDK s poslužiteljem *Mobilne aplikacije* SDK i slično za *Mobilne usluge*. Ovaj ugovor je određeno posebno zaglavlje vrijednost koja se koristi tako da je postupak klijentske i poslužiteljske SDK-ovi, `ZUMO-API-VERSION`.

Napomena: svaki put kada se ovaj dokument odnosi *Mobilne usluge* pozadinskog, ga ne moraju nužno biti moraju se nalaziti na mobilne usluge. Sada je moguće migrirati servis za mobilne uređaje pokrenuti aplikacije servisa za bez ikakvih promjena kod, ali servis će i dalje koristiti verzije SDK *Mobilne usluge* .

Dodatne informacije o migraciji usluzi aplikacije bez ikakvih promjena kod potražite u članku [Migracija mobilne usluge za aplikacije servisa za Azure].

## <a name="header-specification"></a>Specifikacija zaglavlja

Tipku `ZUMO-API-VERSION` možda je li navedena u zaglavlju HTTP ili niza upita. Vrijednost je niz verzije u obrazac **x.y.z, a**.

Ako, na primjer:

Početak https://service.azurewebsites.net/tables/TodoItem

ZAGLAVLJA: ZUMO API-VERZIJA: 2.0.0

OBJAVLJIVANJE https://service.azurewebsites.net/tables/TodoItem?ZUMO-API-VERSION=2.0.0

## <a name="opting-out-of-version-checking"></a>Prestanku provjeru verzije

Možete odabrati iz verzije provjere tako da postavite vrijednost **true** za aplikaciju za postavljanje **MS_SkipVersionCheck**. Navedite to u vašem web.config ili u odjeljku postavke aplikacije portala za Azure.

> [AZURE.NOTE] Postoji nekoliko Promjena ponašanja između mobilnih usluga i mobilne aplikacije, posebice u područja izvanmrežnu sinkronizaciju, provjeru autentičnosti i slanje obavijesti. Trebali biste odabrati samo iz verzije Provjera nakon dovršena testiranje da biste bili sigurni da te promjene behavioral prekinuti funkcionalnosti aplikacije programa.

## <a name="summary-of-compatibility-for-all-versions"></a>Sažetak kompatibilnosti za sve verzije

Grafikon u nastavku prikazuje kompatibilnost sve vrste klijenta i poslužitelja. S pozadinskom je klasificirati kao mobilnih **usluga** ili mobilne **aplikacije** koji se temelji na poslužitelju SDK koja se koristi.

|                           | **Mobilne usluge** Node.js ili .NET | **Mobilne aplikacije** Node.js ili .NET |
| ----------                | -----------------------             |   ----------------              |
| [Servisi za mobilne klijente] | ok                                  | Pogreška\*                         |
| [Aplikacije za mobilne klijente]     | Pogreška\*                             | ok                              |

\*To se može kontrolirati određivanjem **MS_SkipVersionCheck**.


<!-- IMPORTANT!  The anchors for Mobile Services and Mobile Apps MUST be 1.0.0 and 2.0.0 respectively, since there is an exception error message that uses those anchors. -->

<!-- NOTE: the fwlink to this document is http://go.microsoft.com/fwlink/?LinkID=690568 -->

## <a name="1.0.0"></a>Mobilna usluga klijenta i poslužitelja

Klijent SDK-ovi u tablici u nastavku su kompatibilni s **Mobilnih usluga**.

Napomena: klijentskog mobilne usluge programa SDK-ovi *ne* šalju vrijednost zaglavlja za `ZUMO-API-VERSION`. Ako servis dobiva taj zaglavlje ili vrijednost niza upita, pogreška vratit će se, osim ako izričito odabrali ste smanjivati prethodno opisan.

### <a name="MobileServicesClients"></a>Mobilnu verziju klijentskog *servisa* SDK-ovi

| Klijent platforme                   | Verzija                                                                   | Vrijednost zaglavlja verzija |
| -------------------               | ------------------------                                                  | -------------------  |
| Upravljani klijenta (Windows, Xamarin) | [1.3.2](https://www.nuget.org/packages/WindowsAzure.MobileServices/1.3.2) | n/d                  |
| iOS                               | [2.2.2](http://aka.ms/gc6fex)                                             | n/d                  |
| Android                           | [2.0.3](https://go.microsoft.com/fwLink/?LinkID=280126)                   | n/d                  |
| HTML                              | [1.2.7](http://ajax.aspnetcdn.com/ajax/mobileservices/MobileServices.Web-1.2.7.min.js) | n/d     |

### <a name="mobile-services-server-sdks"></a>Mobilne *usluge* server SDK-ovi

| Poslužitelj platforme  | Verzija                                                                                                        | Zaglavlja prihvaćenom verzija |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Verzija WindowsAzure.MobileServices.Backend.* 1.0.x](https://www.nuget.org/packages/WindowsAzure.MobileServices.Backend/) | **Nema zaglavlja verzija** |
| Node.js          | (uskoro dostupno)                        | **Nema zaglavlja verzija** |

<!-- TODO: add Node npm version -->

### <a name="behavior-of-mobile-services-backends"></a>Ponašanje pozadinski sustav mobilne usluge

| ZUMO API-VERZIJA | Vrijednost MS_SkipVersionCheck | Odgovor |
| ---------------- | ---------------------------- | -------- |
| Nije naveden    | Sve                          | 200 – u redu |
| Bilo koja vrijednost        | TRUE                         | 200 – u redu |
| Bilo koja vrijednost        | FALSE ili nije naveden          | 400 - Neispravan zahtjev |

## <a name="2.0.0"></a>Azure mobilne aplikacije klijenta i poslužitelja

### <a name="MobileAppsClients"></a>Mobilne *aplikacije* klijenta SDK-ovi

Provjera verzije uvedena počevši od sljedećih verzija klijenta SDK za **Azure mobilne aplikacije**:

| Klijent platforme                   | Verzija                   | Vrijednost zaglavlja verziju |
| -------------------               | ------------------------  | -----------------    |
| Upravljani klijenta (Windows, Xamarin) | [2.0.0](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/2.0.0) | 2.0.0 |
| iOS                               | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=529823) | 2.0.0  |
| Android                           | [3.0.0](http://go.microsoft.com/fwlink/?LinkID=717033&clcid=0x409) | 3.0.0 |

<!-- TODO: add HTML version when released -->

### <a name="mobile-apps-server-sdks"></a>Mobilne *aplikacije* server SDK-ovi

Provjera verzije je sve obuhvaćeno pratiti verzije SDK poslužitelja:

| Poslužitelj platforme  | SDK                                                                                                        | Zaglavlja prihvaćenom verzija |
| ---------------- | ------------------------------------------------------------                                                   | ----------------------- |
| .NET             | [Microsoft.Azure.Mobile.Server](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) | 2.0.0 |
| Node.js          | [Azure-– aplikacije mobile)](https://www.npmjs.com/package/azure-mobile-apps)                         | 2.0.0 |

### <a name="behavior-of-mobile-apps-backends"></a>Ponašanje pozadinski sustav mobilne aplikacije

| ZUMO API-VERZIJA | Vrijednost MS_SkipVersionCheck | Odgovor |
| ---------------- | ---------------------------- | -------- |
| x.y.z, a ili Null    | TRUE                         | 200 – u redu |
| Null             | FALSE ili nije naveden          | 400 - Neispravan zahtjev |
| 1.x.y            | FALSE ili nije naveden          | 400 - Neispravan zahtjev |
| 2.0.0-2.x.y      | FALSE ili nije naveden          | 200 – u redu |
| 3.0.0-3.x.y      | FALSE ili nije naveden          | 400 - Neispravan zahtjev |


## <a name="next-steps"></a>Daljnji koraci

- [Migriranje servis za mobilne uređaje u aplikacije servisa za Azure]


[Servisi za mobilne klijente]: #MobileServicesClients
[Aplikacije za mobilne klijente]: #MobileAppsClients


[Mobile App Server SDK]: http://www.nuget.org/packages/microsoft.azure.mobile.server
[Migriranje servis za mobilne uređaje u aplikacije servisa za Azure]: app-service-mobile-migrating-from-mobile-services.md

