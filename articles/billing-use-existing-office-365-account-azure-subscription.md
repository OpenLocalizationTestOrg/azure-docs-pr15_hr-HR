<properties
    pageTitle="Zajedničko korištenje s jednog klijenta za Azure AD putem pretplate na Office 365 i Azure | Microsoft Azure"
    description="Saznajte kako zajednički koristiti Azure AD klijenta sustava Office 365 i njezine korisnike s pretplatom Azure i obrnuto"
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Korištenje postojećih računa za Office 365 s pretplatom Azure i obrnuto
Scenarij: Već imate pretplatu na Office 365 i spremni ste za Azure pretplatu, ali želite koristiti postojeće korisničke račune za Office 365 za pretplatu Azure. Umjesto toga su Azure pretplatnika i želite da se pretplate na Office 365 za korisnike sustava postojeće Azure Active Directory. U ovom se članku prikazuje kako je lako postizanju.

> [AZURE.NOTE] Ovaj članak ne odnosi na kupci ugovor Enterprise (EA). Ako vam je potrebna dodatna pomoć u ovom članku, [obratite se podršci](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) u bilo kojem trenutku da biste brzo riješiti problem.


## <a name="quick-guidance"></a>Brze upute

- Ako već imate pretplatu na Office 365 i želite da biste se registrirali za Azure, koristite mogućnost **prijavite se pomoću računa tvrtke ili ustanove** . Nastavite Azure postupak prijave pomoću računa sustava Office 365. Pogledajte [detaljne upute u nastavku ovog članka](#s1).

- Ako već imate pretplatu na Azure i želite da biste dobili pretplatu na Office 365, prijavite se u Office 365 pomoću računa za Azure. Zatim nastavite s korake za prijavu. Kada dovršite prijavu, pretplata na Office 365 se dodaju u istoj instanci Azure Active Directory koji pripada Azure pretplatu. Dodatne informacije potražite u članku sekcije [detaljne upute u nastavku ovog članka](#s2).

>[AZURE.NOTE] Da biste dobili pretplatu na Office 365, račun koji koristite za prijavu, morate biti član uloge direktorija globalni administrator ili administrator naplate u klijentu za Azure Active Directory. [Saznajte kako odrediti uloga u Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

Da biste shvatili što se događa kada dodate pretplatu na račun, pogledajte [osnovne informacije u nastavku članka](#background-information).

## <a name="detailed-steps"></a>Detaljne upute
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>Scenarij 1: Office 365 korisnike namjeravate kupiti Azure
U ovom scenariju, pretpostavimo da Kelley zida je korisnika koji ima pretplatu na Office 365 i planiranje pretplatiti Azure. Postoje dvije dodatne aktivni korisnici, Zorica i Sandra. Račun Kelley na admin@contoso.onmicrosoft.com.

![Centar za administratore sustava Office 365 korisnika](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Da biste se registrirali za Azure, slijedite ove korake:

1. Prijavite se za Azure pri [Azure.com](https://azure.microsoft.com/). Kliknite **isprobajte besplatno**. Na sljedećoj stranici kliknite **Pokreni odmah**.

    ![Besplatno pokušajte Azure.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Kliknite **prijavite se pomoću računa tvrtke ili ustanove**.

    ![Prijavite se u Azure.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Prijavite se pomoću računa sustava Office 365. U ovom slučaju je Kelley na račun za Office 365.

    ![Prijavite se pomoću računa sustava Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. Ispunite podatke i dovršite postupak prijave.

    ![Ispunite podatke, a zatim ispunite prijave.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![Kliknite Pokreni moj servis za upravljanje.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Sada sve je spremno. Na portalu za Azure, trebali biste vidjeti istim korisnicima koji se pojavljuje. Da biste to provjerili, slijedite ove korake:

1. Na zaslonu prikazanom u prethodno kliknite **započnite upravljati moj servis** .
2. Kliknite **Pregledaj**, a zatim kliknite **Servisa Active Directory**.

    ![Kliknite Pregledaj, a zatim kliknite servisa Active Directory.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Kliknite **KORISNICI**.

    ![Na kartici korisnika](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Svi korisnici, uključujući Kelley, navedene su prema očekivanjima.

    ![Popis korisnika](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>Scenarij 2: Azure korisnike namjeravate kupiti Office 365

U ovom scenariju Kelley zida je korisnik pretplate u odjeljku račun za Azure admin@contoso.onmicrosoft.com. Kelley želi pretplata na Office 365 i koristiti isti directory je već instalirana s Azure.

>[AZURE.NOTE] Da biste dobili pretplatu na Office 365, računa koji koristite za prijavu mora biti član uloge direktorija globalni administrator ili administrator naplate u klijentu za Azure Active Directory. [Saznajte kako odrediti uloga u Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

![Postavke Azure portala pretplate](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Korisnici servisa Active Directory za Azure portala](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Za pretplatu na Office 365, slijedite ove korake:

1. Idite na [stranicu proizvoda za Office 365](https://products.office.com/business), a zatim odaberite plan prikladan za vas.
2. Kada odaberete željenu tarifu, prikazat će se sljedeća stranica. Unesite podatke u obrazac. U gornjem desnom kutu stranice kliknite **Prijava** .

    ![Probna stranica za Office 365](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Prijavite se pomoću vjerodajnica za račun. U ovom primjeru je Kelley na račun.

    ![Prijava u Office 365](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Kliknite **isprobajte odmah**.

    ![Provjerite svoje narudžbe za Office 365.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. Na stranici Potvrda narudžbe, kliknite **Nastavi**.

    ![Potvrda narudžbe za Office 365](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Sada sve je spremno. U centru za administratore sustava Office 365, trebali biste vidjeti korisnici iz imenika tvrtke Contoso prikazuju se kao aktivni korisnici. Da biste to provjerili, slijedite ove korake:

1. Otvorite centar za administratore sustava Office 365.
2. Proširite **korisnika**, a zatim **Aktivni korisnici**.

    ![Korisnici centar za administratore sustava Office 365](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>Kako odrediti je vaša uloga u Azure Active Directory

1. Prijavite se na [portal za Azure](https://portal.azure.com/).
2. Kliknite **Pregledaj**, a zatim kliknite **Servisa Active Directory**.

    ![Active Directory na portalu za Azure](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Kliknite **KORISNICI**.

    ![Korisnici servisa Active Directory Azure zadani portala](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Kliknite korisnika. U ovom primjeru korisnik je Kelley zida.

    Obratite pozornost na to polje uloge **Tvrtke ili ustanove**.

    ![Identitet korisnika Azure portala](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Informacije o pretplatama Azure i Office 365
Office 365 i Azure pomoću servisa Azure Active Directory za upravljanje korisnicima i pretplate. Razmislite o Azure directory kao spremnik u kojem možete grupirati korisnika i pretplate. Da biste koristili isti korisnički račun pretplate Azure i Office 365, morate provjerite je li pretplate stvaraju u istoj mapi. Imajte na umu sljedeće detalje:

- Pod direktorija, ne, Suprotno tome će stvoriti pretplatu.
- Korisnici pripadaju direktorija, ne, Suprotno tome.
- Pretplatu stane u imeniku korisnika koji stvara pretplate. Zbog toga pretplate na Office 365 je uz isti račun kao pretplate Azure kada se koristi taj račun da biste stvorili pretplata na Office 365.

![Dodatne informacije](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Dodatne informacije potražite u članku [kako Azure pretplate pridružuju Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

>[AZURE.NOTE] Azure pretplate su vlasništvu pojedinačnih korisnika u imeniku.

>[AZURE.NOTE] Direktorija samog su vlasništvu pretplate na Office 365. Ako korisnici u direktoriju imaju dozvole za potreban, možete raditi na ove pretplate.

## <a name="next-steps"></a>Daljnji koraci
Ako koju ste nabavili pretplate na Azure i Office 365 zasebno u prošlosti, a želite dati pristup klijentu za Office 365 s Azure pretplate, potražite u članku [Pridruživanje klijentu za Office 365 Azure pretplate](billing-add-office-365-tenant-to-azure-subscription.md).

> [AZURE.NOTE] Ako i dalje imate pitanja, [obratite se podršci za](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) da biste brzo riješiti problem.
