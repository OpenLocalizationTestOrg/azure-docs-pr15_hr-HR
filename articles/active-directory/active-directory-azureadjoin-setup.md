<properties
    pageTitle="Postavljanje uključivanje za Azure AD za korisnike | Microsoft Azure"
    description="U članku se objašnjava kako administratori postaviti uključivanje za Azure AD za lokalnog imenika i registracija uređaja."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="setting-up-azure-ad-join-in-your-organization"></a>Postavljanje Azure AD uključivanje u tvrtki ili ustanovi

Prije postavljanja Azure Active Directory uključivanje (Azure AD pridružiti), morate sinkronizirati gore lokalnog imenika korisnika s oblakom ili da biste ručno stvaranje upravljanih računa u Azure AD.

Detaljne upute za sinkroniziranje lokalne korisnike za Azure AD opisana su u odjeljku [Integracija vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).


Da biste ručno stvorili i upravljanje korisnicima u Azure AD, pogledajte [Upravljanje korisnicima u Azure AD](https://msdn.microsoft.com/library/azure/hh967609.aspx).

## <a name="set-up-device-registration"></a>Postavljanje Registracija uređaja
1. Prijavite se na portal sustava Azure kao administrator.
2. U lijevom oknu odaberite **Servisa Active Directory**.
3. Na kartici **direktorija** odaberite direktorija.
4. Odaberite karticu **Konfiguracija** .
5. Prijeđite na odjeljak **uređaji** .
6. Na kartici **uređaji** postaviti sljedeće:  
   * **NAJVEĆI broj od UREĐAJIMA po KORISNIKU**: Odaberite maksimalni broj uređaja koje korisnik može imati u Azure AD.  Ako korisnik dosegne ovaj kvote, ih nećete moći dodati dodatne uređaje dok se ne uklanjaju se jedan ili više svoje postojeće uređaje.
   * **OBAVEZAN višestruke provjere provjere autentičnosti za SPOJ uređaji**: postavljanje hoće li korisnici moraju unijeti drugi faktor provjere autentičnosti da biste se uključili svoj uređaj za Azure AD. Dodatne informacije o Azure višestruka provjera autentičnosti potražite u članku [Uvod u rad s Azure višestruke provjere autentičnosti u oblaku](..\multi-factor-authentication\multi-factor-authentication-get-started-cloud.md).
   * **KORISNICI mogu AZURE AD SPOJ uređaji**: Odaberite korisnici i grupe dopušteni za pridruživanje uređajima Azure AD.
   * **Dodatni ADMINISTRATORI Uključeno AZURE AD PRIDRUŽENO uređaji**: Azure AD Premium ili Enterprise mobilnost paket (EMS), možete odabrati korisnike koji imaju lokalne administratorske ovlasti na uređaju. Globalni administratori i vlasnici uređaj odobravaju lokalne administratorske ovlasti prema zadanim postavkama.

<center>![Postavljanje uređaja regisration](./media/active-directory-azureadjoin/active-directory-aadjoin-configure-devices.png)</center>

Nakon postavljanja uključivanje za Azure AD za korisnike, mogu povezati sa Azure AD kroz svoje tvrtke ili osobnih uređaje.

Ovo su tri scenarija možete koristiti da biste omogućili korisnicima da biste postavili Azure AD uključivanje:

- Korisnici pridružiti uređaj vlasništvu tvrtke izravno u Azure AD.
- Korisnici domene pridruživanja uređaj tvrtke vlasnik lokalni Active Directory, a zatim proširite uređaj za Azure AD.
- Korisnici Dodavanje računa tvrtke ili obrazovne ustanove za Windows na osobnim uređaja

## <a name="additional-information"></a>Dodatne informacije
* [Windows 10 za tvrtke: načina korištenja uređaji za posao](active-directory-azureadjoin-windows10-devices-overview.md)
* [Proširivanje mogućnosti oblak na uređajima Windows 10 do Azure Active Directory uključivanje](active-directory-azureadjoin-user-upgrade.md)
* [Saznajte više o korištenju scenariji za uključivanje za Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Povezivanje domene pridruženo uređaji Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)
* [Postavljanje uključivanje za Azure AD](active-directory-azureadjoin-setup.md)
