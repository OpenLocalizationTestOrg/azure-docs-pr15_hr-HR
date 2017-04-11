<properties
    pageTitle="Ne možete prijaviti na Azure pretplate | Microsoft Azure"
    description="U članku se opisuje kako otkloniti poteškoće s neke uobičajene probleme prijava Azure pretplate."
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Ne možete se prijaviti za upravljanje pretplate Azure

U ovom se članku vas vodi kroz neke od najčešćih načina za rješavanje problema vezanih uz prijavu.

## <a name="page-hangs-in-the-loading-status"></a>Stranica blokira u status učitavanja

Ako na stranicu u pregledniku internet blokira, pokušajte svaki od sljedećih koraka dok ne dođete do [Azure portal](https://portal.azure.com).

-   Osvježite stranicu.
-   Koristite neki drugi preglednik Internet.
-   Ako koristite Microsoft Internet Explorer, idite na portal za Azure pomoću način pregledavanja weba InPrivate. 

    ODGOVORI.  Kliknite **Alati** ![gumb Alati](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Sigurnost** > **Pregledavanje weba InPrivate**.

    B.  Otvorite [portal za Azure](https://portal.azure.com), a zatim se prijavite na portal.

## <a name="error-message-no-subscriptions-found"></a>Poruka o pogrešci "Nije pronađena pretplate"

Ako vaš račun nema potrebne dozvole, vidjet ćete poruku o pogrešci **nije pronađena nijedna pretplata** . Samo administrator računa možete doći do [Centra za račun](https://account.windowsazure.com/)nisu administratori servisa (SA) ili dodatnih administratora (CA).

**Scenarij 1: Poruka o pogrešci primili [portal za Azure](https://portal.azure.com)**

Da biste riješili taj problem, [Dodavanje dodatnih administratora ili vlasnika uloge](billing-add-change-azure-subscription-administrator.md) za račun.

**Scenarij 2: Poruka o pogrešci primili u [Centru račun za Azure](https://account.windowsazure.com/Subscriptions)**

Provjerite je li račun koji ste koristili administratorski račun. Da biste provjerili tko je administratorski račun, slijedite ove korake:

1.  Prijavite se na [portal za Azure](https://portal.azure.com).
2.  Na izborniku koncentrator odaberite **pretplatu**.
3.  Odaberite pretplatu u koju želite provjeriti, a zatim odaberite **Postavke**.
4.  Odaberite **Svojstva**. Administratorski račun pretplate prikazuje se u okviru **Administratorskog računa** .

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Prijavili ste se automatski kao drugi korisnik

Problem se može pojaviti ako koristite više od jednog korisničkog računa u web-pregledniku.

Da biste riješili taj problem, pokušajte jedan od sljedećih načina:

-   Čišćenje predmemorije i brisanje internetskih kolačića. U pregledniku Internet Explorer kliknite **Alati** ![gumb Alati](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Internetske mogućnosti** > **Brisanje**. Provjerite je li odabrano potvrdne okvire za privremene datoteke, kolačići, lozinku i povijest pregledavanja, a zatim kliknite Izbriši.

-   Vratiti izvorne postavke preglednika Internet Explorer da biste vratili osobne postavke koje ste unijeli. Kliknite **Alati** ![gumb Alati](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Internetske mogućnosti** > **Napredno** > potvrdite okvir **Izbriši osobne postavke** > **Ponovno postavi**.

-   Otvorite Azure portal u načinu pregledavanja weba InPrivate. Kliknite **Alati** ![gumb Alati](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Sigurnost** > **Pregledavanje weba InPrivate**.

## <a name="need-help-contact-support"></a>Potrebna vam je pomoć? Obratite se podršci. 

Ako i dalje potrebna pomoć, a zatim se [obratite službi za podršku](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) da biste brzo riješiti problem. 