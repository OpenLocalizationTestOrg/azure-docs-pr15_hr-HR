<properties
    pageTitle="Omogućivanje Microsoft Windows pozdrav za tvrtke u tvrtki ili ustanovi | Microsoft Azure"
    description="Upute za omogućivanje Microsoft Passport u tvrtki ili ustanovi."
    services="active-directory"
    documentationCenter=""
    keywords="Konfiguriranje Microsoft Passport, Microsoft Windows pozdrav za implementaciju tvrtke"
    authors="markusvi"
    manager="femila"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/11/2016"
    ms.author="markvi"/>


# <a name="enable-microsoft-windows-hello-for-business-in-your-organization"></a>Omogućivanje Microsoft Windows pozdrav za tvrtke u tvrtki ili ustanovi

Nakon [povezivanja domene pridruženo uređaja Windows 10 s Azure Active Directory](active-directory-azureadjoin-devices-group-policy.md), učinite sljedeće da biste omogućili Microsoft Windows pozdrav za tvrtke u vašoj tvrtki ili ustanovi:

1. Implementacija Upravitelj konfiguracije centar sustava  

2. Konfiguriranje postavki pravila

3. Konfiguriranje certifikata profila  




## <a name="deploy-system-center-configuration-manager"></a>Implementacija Upravitelj konfiguracije centar sustava 

Da biste implementirali korisničkih certifikata koji se temelji na Windows pozdrav za ključeve tvrtke, potrebno je sljedeće:

- **Trenutni granu Upravitelj konfiguracije centar sustava** - morate instalirati verziju 1606 ili bolje. Dodatne informacije potražite u članku [dokumentacija za Upravitelj konfiguracije centar sustava](https://technet.microsoft.com/library/mt346023.aspx) i [Blog tima za sustav centar Upravitelj konfiguracije](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx).
- **Infrastruktura javnog ključa (PKI)** – da biste omogućili Microsoft Windows pozdrav za tvrtke pomoću korisničkih certifikata, potreban vam je PKI na mjestu. Ako ga nemate ili ne želite koristiti za korisničkih certifikata, možete implementirati novi kontroler koji sadrži Windows Server 2016 Sastavi 10551 (ili bolje) instaliran. Slijedite korake da biste [instalirali kontroler domene replike u postojeću domenu](https://technet.microsoft.com/library/jj574134.aspx) ili da biste [instalirali na novu servisa Active Directory skupa stabala, ako stvarate novo okruženje](https://technet.microsoft.com/library/jj574166). (Na ISOs dostupnih za preuzimanje sustava [Signiant Media Exchange](https://datatransfer.microsoft.com/signiant_media_exchange/spring/main?sdkAccessible=true).)


## <a name="configure-policy-settings"></a>Konfiguriranje postavki pravila

Da biste konfigurirali sustava Microsoft Windows pozdrav za tvrtke – postavke pravilnika, imate dvije mogućnosti:

- Pravilnik grupe u servisu Active Directory 
- Upravitelj konfiguracijama centar sustava 


Putem pravila grupe u servisu Active Directory je preporučeni postupak da biste konfigurirali Microsoft Windows pozdrav za postavke pravilnika za tvrtke. 



Pomoću upravitelja konfiguracije centar sustava je željeni način kada ih koristiti za implementaciju certifikata. Scenarij:

- Osigurava kompatibilnost s novija scenariji za implementaciju
- Potreban je na klijentskoj strani 1607 za verzije sustava Windows 10 ili bolje.

### <a name="configure-microsoft-windows-hello-for-business-via-group-policy-in-active-directory"></a>Konfiguriranje Microsoft Windows pozdrav za tvrtke putem pravilnika grupe u servisu Active Directory

 
**Korake**:

1.  Otvorite upravitelj poslužitelja, a zatim otvorite **Alati** > **Upravljanje pravilnikom grupe**.
2.  Iz upravljanje pravilnikom grupe, pomaknite se do čvor domene koja odgovara domene u kojoj želite omogućiti Azure AD uključiti.
3.  Desnom tipkom miša kliknite **Objekti pravilnika grupe**, a zatim odaberite **Novo**. Naziv objekta pravilnika grupe, na primjer, omogućite Windows pozdrav za tvrtke. Kliknite **u redu**.
4.  Desnom tipkom miša kliknite novi objekt pravilnika grupe, a zatim odaberite **Uređivanje**.
5.  Dođite do **konfiguracije računala** > **pravila** > **Administrativni predlošci** > **komponente Windows** > **Windows pozdrav za tvrtke**.
6.  Desnom tipkom miša kliknite **Omogući Windows pozdrav za tvrtke**, a zatim odaberite **Uređivanje**.
7.  Odaberite gumb mogućnosti **omogućeno** , a zatim kliknite **Primijeni**. Kliknite **u redu**.
8.  Sada možete povezati objekt pravilnika grupe na mjesto po izboru. Da biste omogućili ovo pravilo za sve domene pridruženo Windows 10 uređaje u vašoj tvrtki ili ustanovi, veza pravila grupe s domenom. Ako, na primjer:
 - Na određene organizacijsku jedinicu (OU) u servisu Active Directory gdje će se nalaziti računala domene pridruženo Windows 10
 - Određenu sigurnosnu grupu koja sadrži Windows 10 domene pridruženo računala na kojima će se automatski registered s Azure AD


### <a name="configure-windows-hello-for-business-using-system-center-configuration-manager"></a>Konfiguriranje Windows pozdrav za tvrtke pomoću upravitelja konfiguracije centar sustava


**Korake**:


1. Otvorite **Upravitelj konfiguracije centar sustava**, a zatim otiđite do **imovinu i usklađenost > postavke za usklađenost > pristup resursu tvrtke > Windows pozdrav za profili poslovnih**.

    ![Konfiguriranje Windows pozdrav za tvrtke](./media/active-directory-azureadjoin-passport-deployment/01.png)


2. Na alatnoj traci na vrhu kliknite **Stvaranje Windows pozdrav za profil tvrtke**.

    ![Konfiguriranje Windows pozdrav za tvrtke](./media/active-directory-azureadjoin-passport-deployment/02.png)

2. U dijaloškom okviru **Općenito** poduzeti sljedeće korake:

    ![Konfiguriranje Windows pozdrav za tvrtke](./media/active-directory-azureadjoin-passport-deployment/03.png)

    na. U tekstni okvir **naziv** upišite naziv za svoj profil, na primjer, **Moj profil WHfB**.

    b. Kliknite **Dalje**.


2. U dijaloškom okviru **Podržane platforme** odaberite platforme koja će biti tamo uz ovaj Windows pozdrav profila tvrtke, a zatim kliknite **Dalje**.

    ![Konfiguriranje Windows pozdrav za tvrtke](./media/active-directory-azureadjoin-passport-deployment/04.png)


2. U dijaloškom okviru **Postavke** poduzeti sljedeće korake:

    ![Konfiguriranje Windows pozdrav za tvrtke](./media/active-directory-azureadjoin-passport-deployment/05.png)

    na. Kao **Konfiguriranje Windows pozdrav za tvrtke**, odaberite **omogućeno**.

    b. Kao **koristiti pouzdana platformu modula (TPM)**, odaberite **obavezno**. 

    c. Kao **način provjere autentičnosti**, odaberite **certifikat utemeljen**.

    d. Kliknite **Dalje**.



2. U dijaloškom okviru **Sažetak** , kliknite **Dalje**.

2. U dijaloškom okviru **dovršetka** kliknite **Zatvori**.


2. Na alatnoj traci na vrhu kliknite **Implementiraj**.

    ![Konfiguriranje Windows pozdrav za tvrtke](./media/active-directory-azureadjoin-passport-deployment/06.png)



## <a name="configure-the-certificate-profile"></a>Konfiguriranje certifikata profila 

Ako koristite provjeru autentičnosti utemeljenu na certifikat za provjeru autentičnosti lokalnog, morate konfigurirati i implementacija certifikat profila. Ovaj zadatak morate postaviti NDES poslužitelj i zareza Registracija certifikat web-mjesta uloga u sustavu centar upravitelju konfiguracije. Dodatne informacije potražite u članku [preduvjeti za potvrdu profile u upravitelju konfiguracije](https://technet.microsoft.com/library/dn261205.aspx).

1. Otvorite **Upravitelj konfiguracije centar sustava**, a zatim otiđite do **imovinu i usklađenost > postavke za usklađenost > pristup resursu tvrtke > profili certifikat**.


2. Odaberite predložak pametna kartica prijavu prošireni korištenja ključa (EKU).

Na stranici profila certifikat **SCEP registraciju** morate odaberite **Instaliraj s za rad u suprotnom neće funkcionirati** kao **Davatelja ključ za pohranu**.



## <a name="next-steps"></a>Daljnji koraci
* [Windows 10 za tvrtke: načina korištenja uređaji za posao](active-directory-azureadjoin-windows10-devices-overview.md)
* [Proširivanje mogućnosti oblak na uređajima Windows 10 do Azure Active Directory uključivanje](active-directory-azureadjoin-user-upgrade.md)
* [Provjera autentičnosti identiteta bez lozinke kroz Microsoft Passport](active-directory-azureadjoin-passport.md)
* [Saznajte više o korištenju scenariji za uključivanje za Azure AD](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Povezivanje domene pridruženo uređaji Azure AD za Windows 10 sučelja](active-directory-azureadjoin-devices-group-policy.md)
* [Postavljanje uključivanje za Azure AD](active-directory-azureadjoin-setup.md)
