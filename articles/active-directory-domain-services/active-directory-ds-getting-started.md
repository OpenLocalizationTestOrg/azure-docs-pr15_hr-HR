<properties
    pageTitle="Azure servisa Active Directory Domain Services: Stvaranje grupe administratora Kontroler AAD | Microsoft Azure"
    description="Uvod u Azure Active Directory Domain Services"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="get-started-with-azure-ad-domain-services"></a>Početak rada s Azure servisa Active Directory Domain Services

U ovom se članku vodi kroz konfiguracijske zadatke potrebne da biste omogućili Azure AD domenske servise za vaš klijent Azure AD.

## <a name="task-1-create-the-aad-dc-administrators-group"></a>Zadatak 1: Stvaranje grupe "AAD Kontroler administratori"
Prvi zadatak je stvaranje grupe administratora na klijentu Azure Active Directory. Posebne grupe administratora zove **AAD Kontroler administratori**. Članovi ove grupe odobravaju administratorske ovlasti na računalima koji su domene združeni na domenu upravljanih Azure servisa Active Directory Domain Services. Na računalima domene združeni ovu grupu dodaje u grupu "Administratori". Članovi ove grupe, k tome, možete koristiti udaljene radne površine daljinsko povezivanje s domene združeni strojeva.  

> [AZURE.NOTE] Nemate ovlasti administratora domene ili Administrator u tvrtki upravljanih domeni stvoren pomoću Azure servisa Active Directory Domain Services. Na upravljanih domene ovlasti po servis, a ne postaju dostupne korisnicima unutar klijentu. Međutim, možete koristiti na posebne grupe administratora stvorene u ovom ćete zadatku konfiguracije za izvođenje neke povlaštene operacije. Te operacije obuhvaćaju uključivanja računala s domenom pripadaju grupe administratora na domenu pridruženo strojeva konfiguriranje grupe pravila itd.

U ovom ćete zadatku konfiguracije Stvaranje grupe administratora i dodavanje jednog ili više korisnika u imeniku u grupu. Poduzmite sljedeće korake da biste stvorili grupu administratora za Azure servisa Active Directory Domain Services:

1. Dođite do **Azure klasični portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com))

2. Odaberite čvor **Servisa Active Directory** u lijevom oknu.

3. Odaberite klijenta Azure AD (imenik) za koju želite omogućiti Azure servisa Active Directory Domain Services. Možete stvoriti samo jednu domenu za svaki Azure AD direktorija.

    ![Odabir Azure AD direktorija](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Kliknite karticu **grupe** .

5. Da biste dodali grupe klijentu za Azure AD, kliknite **Dodaj grupu** iz okna zadatka pri dnu stranice.

    ![Dodavanje gumba za grupu](./media/active-directory-domain-services-getting-started/add-group-button.png)

6. Stvaranje grupe pod nazivom **AAD Kontroler administratori**. Postavite **VRSTU GRUPE** na **Sigurnost**.

    > [AZURE.WARNING] Da biste omogućili pristup unutar servisa Azure AD domene upravlja domene, stvorite grupu s ovim nazivom točan.

    ![Stvaranje grupe administratora](./media/active-directory-domain-services-getting-started/create-admin-group.png)

7. Dodajte opis grupe sustava pa drugima da biste dodijelili administratorske ovlasti unutar Azure servisa Active Directory Domain Services koristi ovu grupu.

8. Nakon stvaranja grupe kliknite naziv grupe da biste pogledali svojstva ovu grupu. Da biste dodali korisnike kao članovi ove grupe, kliknite gumb **Dodaj članove** na dnu ploče.

    ![Dodavanje gumba članova grupe](./media/active-directory-domain-services-getting-started/add-group-members-button.png)

9. U dijaloškom okviru **Dodaj članove** , odaberite korisnike koji moraju biti članovi ove grupe i potvrdite okvir kada završite.

    ![Dodavanje korisnika u grupu administratora](./media/active-directory-domain-services-getting-started/add-group-members.png)

<br>

## <a name="task-2-create-or-select-an-azure-virtual-network"></a>Zadatak 2: Stvaranje ili odabir Azure virtualne mreže
Sljedeći zadatak konfiguracije je za [Stvaranje ili odabir Azure virtualne mreže](active-directory-ds-getting-started-vnet.md).
