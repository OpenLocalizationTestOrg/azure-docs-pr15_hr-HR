<properties
    pageTitle="Azure Active Directory Domain Services: Vodič za administraciju | Microsoft Azure"
    description="Stvaranje je Organizacijska jedinica (Organizacijska Jedinica) na Azure AD domenske servise koji se upravlja domena"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="create-an-organizational-unit-ou-on-an-azure-ad-domain-services-managed-domain"></a>Stvaranje je Organizacijska jedinica (Organizacijska Jedinica) na domeni upravljanih programa Azure servisa Active Directory Domain Services
Azure domene servisa Active Directory Domain Services upravljanih obuhvaćaju dva spremnika ugrađene naziva 'AADDC računala' i "AADDC korisnike". Spremnik 'AADDC računala' je računalo objekata na svim računalima na kojima su se pridružili upravljanog domenu. Spremnik 'AADDC korisnika' sadrži korisnicima i grupama u klijentu za Azure AD. Povremeno, možda će biti potrebno da biste stvorili računa servisa upravljanih domenu za implementaciju radnih opterećenja. U tu svrhu možete stvoriti na prilagođenu tvrtke ili ustanove jedinice (OU) upravljanih domeni i stvaranje računa servisa unutar tog Organizacijska Jedinica. U ovom se članku objašnjava stvaranje je Organizacijska Jedinica u upravljanih domene.


## <a name="install-ad-administration-tools-on-a-domain-joined-virtual-machine-for-remote-administration"></a>Instalacija alata za administraciju AD na virtualnog računala domene pridruženo za daljinsko administriranje
Azure upravljanih domene servisa Active Directory Domain Services može upravljati daljinski pomoću poznatih servisa Active Directory administrativne alate kao što je Active Directory Administrativni centar (ADAC) ili AD PowerShell. Administratori za vanjske korisnike nemaju ovlasti za povezivanje s kontrolera domena upravljanih domeni putem udaljene radne površine. Da biste administriranje upravljanih domene, instalirajte značajku AD Administracija Alati na virtualnog računala pridruženo upravljanih domeni. Potražite u članku upute za [administraciju programa Azure AD domene upravlja domenskih servisa](active-directory-ds-admin-guide-administer-domain.md) .

## <a name="create-an-organizational-unit-on-the-managed-domain"></a>Stvaranje Organizacijska jedinica upravljanih domeni
Sad kad AD Administrativni alati instaliranih na domenu pridruženo virtualnog računala, koristimo te alate da biste stvorili Organizacijska jedinica upravljanih domeni. Poduzmite sljedeće korake:

> [AZURE.NOTE] Samo članovi grupe 'AAD Kontroler administratora' imaju potrebne ovlasti da biste stvorili prilagođeni OU. Provjerite je li korisnik koji pripada grupi za poduzeti sljedeće korake.

1. Na početnom zaslonu kliknite **Stavku Administrativni alati**. Trebali biste vidjeti Administrativni alati AD instalirano virtualnog računala.

    ![Administrativni alati instalirana na poslužitelju](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Kliknite **Centar za administratora servisa Active Directory**.

    ![Administracijski centar za Active Directory](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Da biste pogledali domenu, kliknite naziv domene u lijevom oknu (na primjer, "contoso100.com").

    ![ADAC – prikaz domene](./media/active-directory-domain-services-admin-guide/create-ou-adac-overview.png)

4. U oknu **zadataka** desnoj strani u odjeljku čvor naziva domene kliknite **Novo** . U ovom primjeru smo kliknite **Novo** u odjeljku čvor 'contoso100(local)' u oknu **zadataka** desnoj strani.

    ![ADAC – novi Organizacijska Jedinica](./media/active-directory-domain-services-admin-guide/create-ou-adac-new-ou.png)

5. Vidjet ćete mogućnost stvaranja Organizacijska jedinica. Kliknite **Organizacijsku jedinicu** da biste pokrenuli dijaloški okvir **Stvaranje Organizacijska jedinica** .

6. U dijaloškom okviru **Stvaranje Organizacijska jedinica** unesite **naziv** za novu organizacijsku jedinicu. Unesite kratak opis za organizacijsku jedinicu. Polje **Upravlja tako da** mogu postaviti i za organizacijsku jedinicu. Da biste stvorili prilagođeni OU, kliknite **u redu**.

    ![ADAC – OU dijaloški okvir stvaranje](./media/active-directory-domain-services-admin-guide/create-ou-dialog.png)

7. I novostvorenom OU sad trebao izgledati u u AD Administrativni centar (ADAC).

    ![ADAC – OU stvorili](./media/active-directory-domain-services-admin-guide/create-ou-done.png)


## <a name="permissionssecurity-for-newly-created-ous"></a>Dozvole i sigurnosne za novostvorenu organizacijske jedinice
Prema zadanim postavkama korisnika (član grupe "AAD Kontroler administratori") koji je stvorio prilagođene OU moguć je administratorskim ovlastima (Potpuna kontrola) putem organizacijsku jedinicu. Korisnik može nastaviti i dodjela ovlasti za drugih korisnika ili grupu 'AAD Kontroler administratori' po želji. U sljedećim snimku zaslona, korisnik 'bob@domainservicespreview.onmicrosoft.com' koji je stvorio novi Organizacijska jedinica 'MyCustomOU' moguć je Potpuna kontrola iznad nje.

 ![ADAC – novi OU sigurnosti](./media/active-directory-domain-services-admin-guide/create-ou-permissions.png)


## <a name="notes-on-administering-custom-ous"></a>Bilješke o administriranju prilagođene organizacijske jedinice
Sad kad ste stvorili prilagođene OU, možete nastaviti i stvorite korisnike, grupe, računala i računa servisa u ovom OU. Ne možete premjestiti korisnike ili grupe za "AAD Kontroler korisnike' OU prilagođene organizacijske jedinice.

> [AZURE.WARNING] Korisnički računi, grupe, računa servisa i računalne objekte koje ste stvorili u odjeljku prilagođene organizacijske jedinice nisu dostupne u klijentu za Azure AD. Drugim riječima, ti objekti neće prikazati pomoću API Azure AD grafikonu ili u korisničkom Sučelju Azure AD. Ti se objekti dostupne su samo u vašoj domeni upravljanog Azure servisa Active Directory Domain Services.


## <a name="related-content"></a>Srodni sadržaji

- [Administriranje domene upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-administer-domain.md)

- [Administracijski centar servisa Active Directory: Početak rada](https://technet.microsoft.com/library/dd560651.aspx)

- [Postupni vodič za servis računi](https://technet.microsoft.com/library/dd548356.aspx)
