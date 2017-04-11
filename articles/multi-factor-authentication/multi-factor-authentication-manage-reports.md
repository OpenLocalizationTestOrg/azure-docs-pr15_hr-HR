<properties
    pageTitle="Izvješća Azure višestruka provjera autentičnosti"
    description="To opisuje kako koristiti značajku Azure višestruka provjera autentičnosti - izvješća."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="reports-in-azure-multi-factor-authentication"></a>Izvješća u Azure višestruka provjera autentičnosti

Azure višestruka provjera autentičnosti nudi nekoliko izvješća koja se mogu koristiti i tvrtke ili ustanove. Ta izvješća možete pristupiti putem višestruke provjere autentičnosti portala za upravljanje, koje morate imati licencu za Azure MFA davatelj usluga ili Azure MFA, Azure AD Premium ili paket mobilnost za tvrtke. Slijedi popis dostupnih izvješća.

Izvješća možete pristupiti putem portala za upravljanje Azure.

Ime| Opis
:------------- | :------------- |
Korištenje | Korištenje izvješća prikaz podataka o cjelokupan korištenje, korisnički sažetak i Detalji o korisniku.
Status poslužitelja|Ovo izvješće prikazuje status višestruke provjere autentičnosti poslužitelja povezan s vašim računom.
Povijest blokiranih korisnika|Ta izvješća Prikažite povijest zahtjevi za blokiranje ili oslobađanje korisnika.
Povijest zaobići korisnika|Prikazuje povijest zahtjeva za da biste zaobišli višestruke provjere autentičnosti za korisničke telefonski broj.
Upozorenje prijevara|Prikazuje povijest prijevare upozorenja poslane tijekom raspon datuma koji ste naveli.
U redu čekanja|Popis izvješća u redu čekanja za obradu i njihovo stanje. Veza na preuzimanje ili prikaz izvješće navedeni su po dovršetku izvješća.

## <a name="to-view-reports"></a>Da biste pogledali izvješća

1.  Prijavite se na http://azure.microsoft.com
2.  Na lijevoj strani odaberite servisa Active Directory.
3.  Odaberite jednu od sljedećih mogućnosti:
    - **Mogućnost 1**: kliknite karticu davatelji provjere autentičnosti višestruke provjere. Odaberite davatelja usluge MFA i kliknite gumb Upravljanje pri dnu.
    - **Mogućnost 2**: Odaberite direktorija, a zatim kliknite karticu konfiguracija. U odjeljku višestruka provjera autentičnosti odaberite Upravljanje postavkama servisa. Pri dnu stranice Postavke servisa MFA, kliknite portala veza Idi na.
4.  Upravljanje portalu Azure višestruke provjere autentičnosti vidjet ćete prikaz izvješća sekcije u lijevom navigacijskom oknu. Na tom mjestu možete odabrati izvješća na prethodno opisan.

<center>![Oblak](./media/multi-factor-authentication-manage-reports/report.png)</center>


**Dodatni resursi**

* [Za korisnike](./end-user/multi-factor-authentication-end-user.md)
* [Azure višestruka provjera autentičnosti na MSDN-u](https://msdn.microsoft.com/library/azure/dn249471.aspx)
