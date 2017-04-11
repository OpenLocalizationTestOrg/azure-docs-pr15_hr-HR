
<properties
    pageTitle="Otklanjanje poteškoća dinamički članstvo u grupama | Microsoft Azure"
    description="Savjeti za otklanjanje poteškoća za dinamičku članstvo u grupama u Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="curtand"/>


# <a name="troubleshooting-dynamic-memberships-for-groups"></a>Dinamični članstva u grupama za otklanjanje poteškoća

**Konfigurirali pravilo na grupu, ali se ne članstva ažurirati u grupi**<br/>Provjerite je li postavka **Omogući delegirani upravljanje grupe** postavljen na **da** na kartici **Konfiguriraj** . Ta postavka će vidjeti samo ako ste se prijavili kao korisnik kojem je dodijeljen licencu za Azure Active Directory Premium. Provjerite je li vrijednosti za korisničke atribute pravila: ima li korisnici koji zadovoljavaju pravila?

**Li konfigurirali pravilo, ali sada se ukloniti postojeće članove pravila**<br/>Ovo je očekivano. Kada je omogućen ili promijenili pravilo, uklanjaju se postojeće članove grupe. Korisnici vratio procjene pravila su dodavati kao članove u grupu.     

**Ne vidite promijeni članstvo instantly kada dodati ili promijeniti pravilo, zašto ne?**<br/>Procjena namjenski članstvo povremeno se mijenja u proces asinkronih pozadine. Postupak koliko dugo traje određen broj korisnika u imeniku i veličine stvoren kao rezultat pravila grupe. Obično direktorija s mali broj korisnika će vidjeti članstva grupe manje od nekoliko minuta. Mape s velikim brojem korisnika može potrajati 30 minuta ili dulje za popunjavanje.

Ti članci sadrže dodatne informacije na Azure Active Directory.

* [Upravljanje pristupom resursima s grupama Azure Active Directory](active-directory-manage-groups.md)
* [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
* [Što je Azure Active Directory?](active-directory-whatis.md)
* [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
