<properties
   pageTitle="Otklanjanje poteškoća: Active Directory stavka je nedostaju ili nisu dostupne | Microsoft Azure "
   description="Što učiniti kada se stavka izbornika servisa Active Directory ne prikazuje na portalu za upravljanje Azure."
   services="active-directory"
   documentationCenter="na"
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/16/2016"
   ms.author="mbaldwin"/>

# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Otklanjanje poteškoća: Active Directory stavka je nedostaju ili nisu dostupni

Mnoge upute za korištenje značajke Azure Active Directory i servise počinju s "Idite na Portal za upravljanje Azure i kliknite **servisa Active Directory**." No što učiniti ako se ne prikazuje stavku kućni broj ili izbornik servisa Active Directory ili ako je označen **Nije dostupno**? U ovoj se temi namijenjen za pomoć. Opisuje uvjete pod kojim **Servisa Active Directory** ne pojavi ili nije dostupan i u članku se objašnjava kako da biste nastavili.

## <a name="active-directory-is-missing"></a>Nema servisa Active Directory

Obično stavku **Servisa Active Directory** pojavit će se u lijevom navigacijskom izborniku. Upute u Azure Active Directory postupke pretpostavimo znači da je u prikazu.

![Snimka zaslona: Active Directory u Azure](./media/active-directory-troubleshooting/typical-view.png)

Stavka servisa Active Directory pojavljuje se u lijevom navigacijskom izborniku kada bilo koji od sljedećih uvjeta. U suprotnom, stavka se ne prikazuje.

* Trenutnog korisnika prijavljeni pomoću Microsoftova računa (prijašnjeg Windows Live ID).

    OR

* Azure klijentu ima direktorij i trenutnog računa je administrator direktorija.

    OR

* Azure klijentu sadrži najmanje jedan prostora za naziv kontrole za Azure AD pristupa (ACS). Dodatne informacije potražite u članku [Prostora za naziv kontrole programa Access](https://msdn.microsoft.com/library/azure/gg185908.aspx).

    OR

* Azure klijentu sadrži najmanje jedan davatelj Azure višestruke provjere autentičnosti. Dodatne informacije potražite u članku [Administriranje Azure multi-factor davatelja usluge provjere autentičnosti](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

Da biste stvorili prostor naziva za kontrolu pristupa ili davatelja višestruka provjera autentičnosti, kliknite **+ Novo** > **Aplikacije servisa** > **Servisa Active Directory**.

Da biste dobili administratorska prava za direktorij imati administrator dodijeliti ulogu administratora za vaš račun. Detalje potražite u članku [Dodjela administratorskih uloga](active-directory-assign-admin-roles.md).

## <a name="active-directory-is-not-available"></a>Active Directory nije dostupna

Kada kliknete **+ Novo** > pojavit će se**Aplikacija servisa** **Active Directory** stavke. Konkretno, servisa Active Directory stavke prikazuje se kad neku od značajki servisa Active Directory, primjerice direktorija, kontrola pristupa ili multi-factor davatelja provjere autentičnosti, na raspolaganju trenutnog korisnika.

Međutim, dok je učitavanja stranice, stavke zasivljen i označen **Nije dostupna**. To je privremeno stanje. Ako Pričekajte nekoliko sekundi, postaje dostupan. Ako je produžen odgode, često osvježavanje web-stranicu rješava problem.

![Snimka zaslona: Active Directory nije dostupna](./media/active-directory-troubleshooting/not-available.png)
