<properties
    pageTitle="Azure AD Connect sinkronizacije: razumijevanje i Prilagodba sinkronizacije | Microsoft Azure"
    description="U članku se objašnjava kako Azure AD Connect sinkronizirati te kako prilagoditi."
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
    ms.date="08/29/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understand-and-customize-synchronization"></a>Azure AD Connect sinkronizacije: razumijevanje i prilagoditi sinkronizacije
Sinkronizacija servisa Azure Active Directory povezivanje (Azure AD Connect sync) je glavni komponenta Azure AD Connect. Ga vodi brigu o sve operacije vezane uz sinkroniziranje podataka identiteta između lokalnog okruženja i Azure AD. Azure AD Connect sinkronizacija je naslijediti DirSync, Azure AD sinkronizaciju i Upravitelj identiteta Forefront s Azure Active Directory poveznik konfiguriran.

U ovoj se temi je početna stranica za **sinkronizaciju Azure AD Connect** (naziva se i **modula za sinkroniziranje**) popise i veze na sve druge teme koji se odnose na njega. Veze na Azure AD Connect potražite u članku [integriranje vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).

Servis za sinkronizaciju sastoji se od dviju komponenata, komponenta za **sinkronizaciju Azure AD Connect** lokalnog i davatelj usluge u Azure AD naziva **Azure AD Connect sinkronizaciju servisa**. Servis je uobičajeno DirSync, Azure AD sinkronizaciju i Azure AD Connect.

## <a name="azure-ad-connect-sync-topics"></a>Azure AD Connect sinkronizaciju teme

Tema | Što obuhvaća i kada za čitanje
----- | -----
**Osnove sinkronizacije za Azure AD Connect** |
[Objašnjenje arhitektura](active-directory-aadconnectsync-understanding-architecture.md) | Za one koji su novi korisnik modula za sinkroniziranje i želite saznati više o arhitektura i izraze koji se koriste.
[Tehnički pojmovi](active-directory-aadconnectsync-technical-concepts.md) | Kratka verzija arhitektura teme i ukratko objašnjava izraze koji se koriste.
[Povezivanje topologija za Azure AD](active-directory-aadconnect-topologies.md) | U članku se opisuje različite Topologija i scenariji podržava modula za sinkroniziranje.
**Prilagođena konfiguracija** |
[Ponovno pokrenuti čarobnjak za instalaciju](active-directory-aadconnectsync-installation-wizard.md) | U članku se objašnjava što mogućnosti koje su dostupne kada ponovno pokrenite čarobnjak za instalaciju Azure AD Connect.
[Objašnjenje deklarativno dodjele resursa](active-directory-aadconnectsync-understanding-declarative-provisioning.md)| U članku se opisuje konfiguracija modela pod nazivom deklarativno dodjele resursa.
[Objašnjenje deklarativno izraza za dodjelu resursa](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) | U članku se opisuje sintaksa za jezik izraza koji se koristi u deklarativno dodjele resursa.
[Objašnjenje zadanu konfiguraciju](active-directory-aadconnectsync-understanding-default-configuration.md)| U članku se opisuje-okviru pravila i zadanom konfiguracijom. Opisuje pravila funkcioniranje scenarije – okvir za rad.
[Razumijevanje korisnika i kontakata](active-directory-aadconnectsync-understanding-users-and-contacts.md) | Nastavlja na prethodnu temu, a zatim u članku se opisuje kako konfiguraciju za korisnike i kontakte to zajedno, posebice u okruženju više skupa stabala.
[Kako promijeniti zadanu konfiguraciju](active-directory-aadconnectsync-change-the-configuration.md) | Vodit će vas kroz kako uobičajene konfiguracije promijeniti atribut tokova.
[Najbolje prakse za promjenu zadanu konfiguraciju](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) | Podržava ograničenja i promjene konfiguracije izvan okvira.
[Konfiguriranje filtriranja](active-directory-aadconnectsync-configure-filtering.md) | U članku se opisuje različite mogućnosti da biste ograničili koji objekti se sinkronizirane za Azure AD i detaljne upute za konfiguriranje ove mogućnosti.
**Značajke i scenarijima** |
[Sprječava nenamjerno brisanje](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) | U članku se opisuje značajku *sprječava nenamjerno briše* i kako ga konfigurirati.
[Raspored](active-directory-aadconnectsync-feature-scheduler.md) | U članku se opisuje ugrađenih rasporeda koji je uvoz, sinkroniziranje i izvoz podataka.
[Implementacije sinkronizacije lozinke](active-directory-aadconnectsync-implement-password-synchronization.md) | U članku se opisuje kako funkcionira sinkronizacija lozinku, kako implementirati i kako raditi i otklanjanje poteškoća.
[Uređaj upisima](active-directory-aadconnect-feature-device-writeback.md) | U članku se opisuje način na koji se upisima uređaja u aplikaciji Azure AD Connect.
[Proširenja direktorija](active-directory-aadconnectsync-feature-directory-extensions.md) | U članku se opisuje kako proširiti sheme Azure AD s vlastite prilagođene atribute.
**Sinkronizacija servisa** |
[Azure AD Connect značajke servisa sinkronizacije](active-directory-aadconnectsyncservice-features.md) | U članku se opisuje davatelj usluge sinkronizaciju i kako promijeniti postavke sinkronizacije u Azure AD.
[Dupliciranje otpornost atributa](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) | U članku se opisuje kako omogućiti i koristiti otpornost vrijednosti duplicirane atribut **userPrincipalName** i **proxyAddresses** .
**Operacije i korisničkog Sučelja** |
[Upravitelj servisa za sinkronizaciju](active-directory-aadconnectsync-service-manager-ui.md) | U članku se opisuje korisničko Sučelje za Upravitelj sinkronizacije servisa, uključujući [operacije](active-directory-aadconnectsync-service-manager-ui-operations.md), [poveznika](active-directory-aadconnectsync-service-manager-ui-connectors.md), [Metaverse Designer](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md)i kartice [Metaverse pretraživanja](active-directory-aadconnectsync-service-manager-ui-mvsearch.md) .
[Operativnih zadataka i napomene](active-directory-aadconnectsync-operations.md) | U članku se opisuje radu opasnosti, kao što su Izrada oporavak.
**kako...** |
[Ponovno postavljanje računa za Azure AD](active-directory-aadconnectsync-howto-azureadaccount.md) | Upute za ponovno postavljanje vjerodajnica računa servisa za povezivanje s Azure AD Connect sinkronizaciju Azure AD.
**Dodatne informacije i reference** |
[Priključci](active-directory-aadconnect-ports.md) | Popise koje priključke morate otvoriti između modula za sinkroniziranje i lokalnog imenika i Azure AD.
[Atributi koji se sinkroniziraju sa servisom Azure Active Directory](active-directory-aadconnectsync-attributes-synchronized.md) | Popis svih atributa koji se sinkroniziraju između lokalnog servisa Active Directory i Azure AD.
[Referenca za funkcije](active-directory-aadconnectsync-functions-reference.md) | Popis svih funkcija dostupna u deklarativno dodjele resursa.

## <a name="additional-resources"></a>Dodatni resursi

* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
