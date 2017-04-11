<properties
    pageTitle="Azure Active Directory B2C: Stvaranje klijent za Azure Active Directory B2C | Microsoft Azure"
    description="Tema o stvaranju klijent za Azure Active Directory B2C"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.topic="article"
    ms.devlang="na"
    ms.date="08/30/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-create-an-azure-ad-b2c-tenant"></a>Azure Active Directory B2C: Stvaranje klijent za Azure AD B2C

Da biste počeli koristiti Microsoft Azure Active Directory (Azure AD) B2C, slijedite tri korake navedene u ovom članku.

## <a name="step-1-sign-up-for-an-azure-subscription"></a>Korak 1: Registracija za Azure pretplate

Ako već imate pretplatu na Azure, preskočite ovaj korak. Ako nije, prijavite se za [Azure pretplate](../active-directory/sign-up-organization.md) i dobiti pristup Azure AD B2C.

## <a name="step-2-create-an-azure-ad-b2c-tenant"></a>Korak 2: Stvaranje klijent za Azure AD B2C

Poduzmite sljedeće korake da biste stvorili novi klijent Azure AD B2C. Trenutno B2C značajke ne može se uključiti u klijenata za postojeće.

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com/) kao Administrator pretplate. Ovo je iste tvrtke ili obrazovne ustanove ili isti Microsoftov račun koji ste koristili za registraciju za Azure.
2. Kliknite **Novi** > **aplikacije servisa** > **servisa Active Directory** > **direktorija** > **stvoriti prilagođene**.

    ![Snimka zaslona uz započinjanje za stvaranje klijenta](./media/active-directory-b2c-get-started/new-directory.png)

3. Odaberite **ime**, **Naziv domene** i **državi ili regiji** za vaš klijent.
4. Potvrdite mogućnost koja vas obavještava da **je B2C direktorija**.
5. Kliknite kvačicu da biste dovršili akciju.

    ![Snimka zaslona s potvrdite okvir da biste stvorili B2C direktorija](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Vaš klijent se sada stvara i prikazat će se u proširenja za Active Directory. Su napravljeni globalni Administrator klijenta. Po potrebi možete dodati druge globalnog administratora.

    > [AZURE.IMPORTANT]
    Ako namjeravate koristiti B2C klijentu za aplikaciju radnog, pročitajte članak [radnog](active-directory-b2c-reference-tenant-type.md)skalu nasuprot drugih korisnika pretpregleda B2C. Imajte na umu da su tamo Poznati problemi kada izbrišete postojećem klijentu za B2C i ponovno stvoriti s istim nazivom domene. Morate stvoriti B2C klijenta s drugi naziv domene.

## <a name="step-3-navigate-to-the-b2c-features-blade-on-the-azure-portal"></a>Korak 3: Idite na plohu značajke B2C portala za Azure

1. Dođite do proširenje servisa Active Directory na navigacijskoj traci na lijevoj strani.
2. Pronađite klijentu na kartici **direktorija** i kliknite ga.
3. Kliknite karticu **Konfiguracija** .
4. Kliknite vezu **Upravljanje B2C postavke** u odjeljku **Administracija B2C** .

    ![Snimka zaslona s konfiguracija direktorija za B2C](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. Portal za Azure s plohu značajke B2C prikazuje će se otvoriti u novoj kartici preglednika ili prozor.

    > [AZURE.IMPORTANT]
    To može potrajati i do 2-3 minute za vaš klijent da bi mogao pristupiti na portalu Azure. Nakon nekog vremena će riješili taj problem, ponovni ove korake. Ako nije, obratite se podršci.

6. Prikvačite plohu za vaše Startboard radi lakšeg pristupa. (Alat za Pin označen crvenom bojom u gornjem desnom kutu plohu značajke)

    ![Snimka zaslona s značajke plohu B2C](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    Možete upravljati korisnici i grupe, samostalno ponovno postavljanje konfiguracije i tvrtke brendiranje značajke vaš klijent za [Azure klasični portal](https://manage.windowsazure.com/).

## <a name="easy-access-to-the-b2c-features-blade-on-the-azure-portal"></a>Jednostavan pristup značajkama plohu B2C portala za Azure

Da biste poboljšali vidljivost, dodali smo prečac za značajke plohu B2C portala za Azure.

1. Prijavite se na portal za Azure kao globalni Administrator B2C klijentu. Ako ste se već prijavili na drugi klijent, prijeđite klijenata (u gornjem desnom kutu).
2. Na lijevoj strani navigacije kliknite **Pregledaj** .
3. Kliknite **Azure AD B2C** da biste pristupili značajki plohu B2C.

    ![Snimka zaslona s Pregledaj plohu B2C značajke](./media/active-directory-b2c-get-started/b2c-browse.png)

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako se registrirali aplikaciju s Azure AD B2C i sastavljanje aplikacija za brzi početak rada tako da pročitate [Azure Active Directory B2C: Registracija aplikacije](active-directory-b2c-app-registration.md).
