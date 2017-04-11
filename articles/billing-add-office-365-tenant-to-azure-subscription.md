<properties
    pageTitle="Korištenje klijentu za Office 365 s pretplatom na Azure | Microsoft Azure"
    description="Saznajte kako dodati direktorija za Office 365 (klijentsko) Azure pretplatu da bi pridruživanja."
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
    ms.date="09/16/2016"
    ms.author="cjiang"/>

# <a name="associate-an-office-365-tenant-with-an-azure-subscription"></a>Pridruživanje klijentu za Office 365 Azure pretplate
Ako koju ste nabavili Azure i Office 365 pretplate zasebno u prošlosti, a sada ga želite dati pristup klijentu za Office 365 s Azure pretplate, jednostavno je da biste to učinili. U ovom se članku objašnjava kako.

> [AZURE.NOTE] Ovaj članak ne odnosi na kupci ugovor Enterprise (EA).

## <a name="quick-guidance"></a>Brze upute
Da biste se pridružili klijentu za Office 365 Azure pretplatu, koristite račun za Azure da biste dodali klijentu za Office 365, a zatim pretplate Azure pridružiti klijentu za Office 365.

## <a name="detailed-steps"></a>Detaljne upute
U ovom scenariju Kelley zida je korisnik pretplate u odjeljku račun za Azure kelley.wall@outlook.com. Kelley ima pretplatu na Office 365 u odjeljku račun kelley.wall@contoso.onmicrosoft.com. Sada Kelley želi da biste pristupili klijentu za Office 365 Azure pretplate.

![Snimka zaslona s Azure Active Directory stanja](./media/billing-add-office-365-tenant-to-azure-subscription/s31_msa-aad-status.png)

![Aktivni korisnici centar za administratore snimka sustava Office 365](./media/billing-add-office-365-tenant-to-azure-subscription/s32_office-365-user.png)

### <a name="prerequisites"></a>Preduvjeti
Za pridruživanje ispravno funkcionirao, potrebne su sljedeće preduvjete:

- Potreban vam je vjerodajnice administratora servisa Azure pretplate. Dodatnih administratora nije moguće izvesti podskup korake.
- Potreban vam je vjerodajnice globalni administrator klijentu za Office 365.
- Adresu e-pošte od administratora servisa moraju se nalaziti u klijentu za Office 365.
- Adresu e-pošte od administratora servisa mora odgovarati koji od klijentu za Office 365 globalnog administratora.
- Ako trenutno koristite adresu e-pošte koja je Microsoftov račun i račun tvrtke ili ustanove, privremeno promijeniti administrator servisa Azure pretplatu za korištenje drugog Microsoftova računa. Možete stvoriti novi Microsoftov račun na [stranicu prijava za Microsoftov račun](https://signup.live.com/).


Da biste promijenili administratora servisa, slijedite ove korake:

1. Prijavite se na [portal za upravljanje računima](https://account.windowsazure.com/subscriptions).
2. Odaberite pretplatu u koju želite promijeniti.
3. Odaberite **Uređivanje pojedinosti pretplate**.

    ![Snimka zaslona Azure informacija o pretplati, s "Uređivanje pojedinosti pretplate" istaknuta](./media/billing-add-office-365-tenant-to-azure-subscription/s33_azure-edit-subscription-details.png)

4. U okviru **ADMINISTRATOR SERVISA** unesite adresu e-pošte novi administrator servisa.

    ![Snimka zaslona dijaloškog okvira "Uredi pretplate"](./media/billing-add-office-365-tenant-to-azure-subscription/s34_change-subscription-service-admin.png)

### <a name="associate-the-office-365-tenant-with-the-azure-subscription"></a>Pridruživanje klijentu za Office 365 Azure pretplate
Da biste se pridružili klijentu za Office 365 Azure pretplatu, slijedite ove korake:

1.  Prijavite se na [portal za upravljanje računa](https://account.windowsazure.com/subscriptions) s ovlastima administratora servisa.
2.  U lijevom oknu odaberite **Servisa ACTIVE DIRECTORY**.

    ![Snimka zaslona Active Directory unosa](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

    > [AZURE.NOTE] Prikazat će se ne klijentu za Office 365. Ako vidite, prijeđite na sljedeći korak.

    ![Snimka zaslona sa zadanom imeniku Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s36-aad-tenant-default.png)

3. Dodavanje klijentu za Office 365 u pretplatu za Azure.

    na. Odaberite **NOVO** > **DIREKTORIJA** > **PRILAGOĐENE Stvori**.

    ![Stvaranje prilagođene snimka programa Azure Active Directory](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)

    b. Na stranici **Dodavanje direktorija** u **IMENIKU**, odaberite **Koristi postojeći imenik**. Zatim odaberite **sam spremni ste odjavljeni sada**i odaberite **dovrši** ![dovršiti ikona](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Snimka zaslona "Koristi postojeći imenik"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)

    c. Nakon što ste odjavljeni, prijavite se pomoću vjerodajnica globalni administrator za klijentski sustav Office 365.

    ![Snimka zaslona sustava Office 365 globalni administrator prijavu](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)

    d. Odaberite **Dalje**.

    ![Snimka zaslona provjere valjanosti](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)

    e. Odaberite **Odjavi se sada**.

    ![Snimka zaslona sign-out](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)

    f. Prijavite se na [portal za upravljanje računa](https://account.windowsazure.com/subscriptions) s ovlastima administratora servisa.

    ![Snimka zaslona Azure prijavu](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

    g. Trebali biste vidjeti klijentu za Office 365 na nadzornoj ploči.

    ![Snimka zaslona nadzorne ploče](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

4. Promijenite direktorij povezan s pretplatom Azure.

    na. Odaberite **Postavke**.

    ![Snimka zaslona Azure klasični portala izgleda popisa postupaka](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)

    b. Odaberite pretplatu Azure, a zatim **Uređivanje DIREKTORIJA**.
    ![Snimka zaslona Azure pretplate uređivanje direktorija](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)

    c. Odaberite **Dalje** ![dalje ikona](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).

    ![Snimka zaslona "Promijeni pridružene direktorija"](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)

    > [AZURE.WARNING] Primit ćete upozorenje da će ukloniti sve dodatnih administratora.

    ![Azure-potvrda-direktorija-mapiranja](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)

    >[AZURE.WARNING] Osim toga, svi korisnici [na temelju uloga kontrole pristupa (RBAC)](./active-directory/role-based-access-control-configure.md) s pristupom kome je dodijeljeno u postojeće grupe resursa i bit će uklonjene. Međutim, upozorenja primate samo spominjanja uklanjanje dodatnih administratora.

    ![Dodijeljeno-korisnika – ukloniti--grupa resursa](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)

    d. Odaberite **dovrši** ![dovršiti ikona](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

5. Sada možete dodati račune za Office 365 za tvrtke ili ustanove kao dodatnih administratora klijenta Azure Active Directory.

    na. Odaberite karticu **ADMINISTRATORI** , a zatim odaberite **DODAJ**.

    ![Snimka zaslona Azure klasični postavkama portala za administratore kartica](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)

    b. Unesite račun tvrtke ili ustanove klijentu za Office 365, odaberite Azure pretplatu, a zatim odaberite **dovrši** ![dovršiti ikona](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Snimka Azure dodavanje dodatnih administratora dijaloškog okvira](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)

    c. Vratite se na karticu **ADMINISTRATORI** . Trebali biste vidjeti račun tvrtke ili ustanove prikazuje kao zajednički administrator.

    ![Snimka zaslona kartice za administratore](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)

6. Sljedeće možete testirati pristup zajedničkoj administratora sustava.

    na. Odjavite se iz portala za upravljanje računima.

    b. Otvorite [portal za upravljanje računima](https://account.windowsazure.com/subscriptions) ili [Azure portal](https://portal.azure.com/).

    c. Ako je Azure stranica za prijavu u veze **prijavite pomoću računa tvrtke ili ustanove**, odaberite vezu. U suprotnom, preskočite ovaj korak.

    ![Stranica za prijavu u snimka Azure](./media/billing-add-office-365-tenant-to-azure-subscription/3-sign-in-to-azure.png)

    d. Unesite vjerodajnice za suautorstvo administrator, a zatim odaberite **prijavite se u**.

    ![Stranica za prijavu u snimka Azure](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="next-steps"></a>Daljnji koraci
Scenariji za povezane obuhvaćaju sljedeće:

- Već imate pretplatu na Office 365 i spremni ste za Azure pretplatu, ali želite koristiti postojeće korisničke račune za Office 365 za pretplatu Azure.
- Su Azure pretplatnik, a želite dobiti pretplatu na Office 365 za korisnike u postojeće instanci Azure Active Directory.

Da biste saznali kako izvršiti sljedeće zadatke, potražite u članku [Korištenje postojećih Office 365 račun s pretplatom Azure i obrnuto](billing-use-existing-office-365-account-azure-subscription.md).
