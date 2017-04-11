<properties
    pageTitle="Postavke pravilnika i MDM grupe | Microsoft Azure"
    description="Pruža informacije o pravilima grupe i mobilnog uređaja postavke upravljanja (MDM) koje želite koristiti na uređajima tvrtke vlasništvu. Ta pravila primjenjuju se na cijelu uređaj korisnika."
    services="active-directory"
    keywords="što su grupe pravila i postavke MDM za Roaming stanje Enterprise, Roaming stanje Enterprise, windows oblak"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="group-policy-and-mdm-settings"></a>Postavke pravilnika grupe i MDM

Pomoću ove pravilnik grupe i postavke upravljanja (MDM) za mobilni uređaj samo na uređajima tvrtke vlasništvu jer je ta pravila primjenjuju se na cijelu uređaj korisnika. Primjena pravila na MDM da biste onemogućili postavke sinkronizacije za osobnog, vlasništvo korisnika uređaj će negativno utjecati na korištenje uređaja. Osim toga, druge korisničke račune na uređaju također mogu utjecati pravila.

Velike tvrtke koje želite upravljati roaming za osobne uređaje (neupravljani) pomoću portala za Azure radi omogućivanja i onemogućivanja roaming umjesto da pomoću pravila grupe ili MDM.
U tablicama u nastavku opisane su dostupne postavke pravila.

## <a name="mdm-settings"></a>Postavke za MDM
Postavke pravilnika MDM odnose se na Windows 10 i Windows 10 Mobile.  Postoji podrška za Windows 10 Mobile samo za Microsoftov račun koji se temelje roaming putem korisnički račun za OneDrive.  Pogledajte odjeljak "Uređaji i krajnje točke" pojedinosti o koje uređaje podržani su za Azure AD temelji sinkroniziranje.

| Ime                               | Opis                                                          |
|------------------------------------|----------------------------------------------------------------------|
| Omogućivanje veze za Microsoftov račun | Korisnicima omogućuje provjeru autentičnosti pomoću Microsoftova računa na uređaju |
| Dopusti postavki sinkronizacije             | Korisnicima omogućuje premještati postavke sustava Windows i aplikacije podatke. Onemogući to pravilo će onemogućiti sinkronizaciju, kao i sigurnosnih kopija na mobilnim uređajima                  |

## <a name="group-policy-settings"></a>Postavke pravilnika grupe
Postavke pravilnika grupe primjenjuju se na Windows 10 uređajima koji su se pridružite se domene servisa Active Directory. Tablica sadrži i naslijeđenih postavke koji će izgledati kao da biste upravljali postavkama sinkronizacije, ali to funkcionira za Enterprise stanje Roaming za Windows 10, koje su navedene s 'Do not use' u opisu.

| Ime                                | Opis |
|-------------------------------------|-------------|
| Korisnički računi: Blok Microsoftovi računi  |Ta postavka pravila spriječit dodavanja novih Microsoftova računa na ovom računalu|
| Sinkronizacija                         |Sprječava korisnika da se premještati postavke sustava Windows i aplikacije podataka|
| Sinkronizacija personalizacija             |Sinkroniziranje onemogućuje grupe teme|
| Sinkronizirajte postavke preglednika        |Sinkroniziranje onemogućuje grupe za Internet Explorer|
| Lozinke se neće sinkronizirati               |Onemogućuje sinkronizaciju lozinke grupe|
| Sinkronizacija drugih postavki za Windows  |Sinkroniziranje onemogućuje druge Windows postavke grupe|
| Sinkronizacija radne površine personalizaciju |Nemojte koristiti; nema učinka|
| Se neće sinkronizirati na veze s ograničenim prometom  |Onemogućuje roaming na veze, kao što je mobilni 3 G s ograničenim prometom|
| Aplikacija se neće sinkronizirati                    |Nemojte koristiti; nema učinka|
|Sinkronizacija postavki aplikacije             |Onemogućuje roaming podataka aplikacije|
|Sinkronizacija postavki za pokretanje           |Nemojte koristiti; nema učinka|


## <a name="related-topics"></a>Povezane teme
- [Pregled Roaming stanje Enterprise](active-directory-windows-enterprise-state-roaming-overview.md)
- [Omogućivanje enterprise stanje roaming servisa Azure Active Directory](active-directory-windows-enterprise-state-roaming-enable.md)
- [Postavke i podaci za roaming najčešća Pitanja](active-directory-windows-enterprise-state-roaming-faqs.md)
- [Referenca za Windows 10 roaming postavke](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
