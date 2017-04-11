<properties
    pageTitle="Azure AD Connect sinkronizacije: pokretanje čarobnjaka za instalaciju drugi put | Microsoft Azure"
    description="Objašnjava kako funkcionira Čarobnjak za instalaciju drugi put kada pokrenete."
    keywords="Čarobnjak za instalaciju Azure AD Connect omogućuje možete konfigurirati postavke održavanja drugi put kada pokrenete ga"
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
    ms.date="08/31/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a>Azure AD Connect sinkronizacije: pokretanje čarobnjaka za instalaciju drugi put
Prvi put pokrenete čarobnjak za instalaciju Azure AD Connect je vodit će vas kroz kako konfigurirati instalaciju. Ako ponovno pokrenite čarobnjak za instalaciju nudi mogućnosti za održavanje.

Na izborniku start pod nazivom **Azure AD Connect**možete pronaći čarobnjak za instalaciju.

![Izbornik Start](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

Kad pokrenete čarobnjak za instalaciju, vidjet ćete stranicu s ove mogućnosti:

![Stranica s popisom dodatne zadataka](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

Ako ste instalirali ADFS s Azure AD Connect, morate još više mogućnosti. Dodatne mogućnosti za ADFS su zabilježeni u [ADFS upravljanje](active-directory-aadconnect-federation-management.md#ad-fs-management).

Odaberite jedan od zadataka i kliknite **Dalje** da biste nastavili.

> [AZURE.IMPORTANT] Dok su otvorite čarobnjak za instalaciju obustavljeno su sve operacije u modula za sinkroniziranje. Provjerite je li zatvorite čarobnjak za instalaciju čim što izvršite željene promjene konfiguracije.

## <a name="view-current-configuration"></a>Konfiguracija trenutnog prikaza
Ta mogućnost omogućuje brz prikaz trenutno konfigurirana mogućnosti.

![Stranica s popisom sve mogućnosti i njihovo stanje](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

Kliknite **prethodno** da biste se vratili. Ako odaberete **Izlaz**, zatvorite čarobnjak za instalaciju.

## <a name="customize-synchronization-options"></a>Prilagodba mogućnosti za sinkronizaciju
Ova se mogućnost koristi da biste unijeli promjene konfiguracije sinkronizacije. Pogledajte podskup mogućnosti iz prilagođene konfiguracije put instalacije. Čak i ako se prethodno koristili eksplicitnih instalacije vidite tu mogućnost.

- [Dodavanje više direktorija](active-directory-aadconnect-get-started-custom.md#connect-your-directories). Uklanjanje direktorij potražite u članku [Brisanje poveznik](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).
- [Promjena domene i OU filtriranja](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).
- Uklanjanje grupe filtriranja.
- [Promjena dodatne značajke](active-directory-aadconnect-get-started-custom.md#optional-features).

Druge mogućnosti iz početne instalacije nije moguće promijeniti, a nisu dostupne. Su sljedeće mogućnosti:

- Promijeniti atribut userPrincipalName i sourceAnchor.
- Promijenite spojno način za objekte iz različitih skupa stabala.
- Omogućite grupne filtriranje.

## <a name="refresh-directory-schema"></a>Osvježi shemu direktorija
Ta se mogućnost koristi ako ste promijenili shemu u jednom od lokalnih šuma AD DS. Na primjer, možda imate instaliran Exchange ili nadograditi na Windows Server 2012 sheme s objektima na uređaj. U ovom slučaju morate uputiti Azure AD Connect ponovno čitanje shemu iz AD DS i ažuriranje predmemoriji. Ova akcija obnavlja i pravila za sinkronizaciju. Ako dodate u shemi sustava Exchange, primjerice, pravila sinkronizacije za Exchange dodaju se konfiguracije.

Kad odaberete tu mogućnost, navedene su direktorija u konfiguraciji. Možete zadržati zadane postavke i Osvježi sve šuma ili poništavanje odabira neke od njih.

![Stranica s popisom svih direktorija u okruženju](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a>Konfiguriranje pripremna načinu rada
Tu mogućnost možete omogućiti i onemogućiti pripremna način rada na poslužitelju. Dodatne informacije o pripremna načinu i kako se koristi pronaći ćete u [operacija](active-directory-aadconnectsync-operations.md#staging-mode).

Mogućnost prikazuje ako pripremna trenutno omogućuje ili onemogućuje:  
![Mogućnost koja se prikazuje trenutno stanje pripremna načinu rada](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

Da biste promijenili stanje, odaberite ovu mogućnost i odabir ili poništavanje potvrdni okvir.  
![Mogućnost koja se prikazuje trenutno stanje pripremna načinu rada](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a>Promjena korisničku prijavu
Ta mogućnost omogućuje promjenu iz sinkronizaciju lozinke za vanjski pristup ili, Suprotno tome. Ne možete promijeniti da biste **konfigurirali**.

Dodatne informacije o tu mogućnost, potražite u članku [korisničku prijavu](active-directory-aadconnect-user-signin.md#changing-user-sign-in-method).

## <a name="next-steps"></a>Daljnji koraci

- Dodatne informacije o konfiguraciji modela koristi Azure AD Connect sinkronizacije u [Razumijevanje deklarativno dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning.md).

**Pregled tema**

- [Azure AD Connect sinkronizacije: razumijevanje i Prilagodba sinkronizacije](active-directory-aadconnectsync-whatis.md)
- [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
