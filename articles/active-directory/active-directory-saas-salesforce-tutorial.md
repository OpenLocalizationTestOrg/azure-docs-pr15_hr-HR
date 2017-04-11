<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Salesforce | Microsoft Azure"
    description="Saznajte kako pomoću servisa Salesforce s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="05/16/2016"
    ms.author="asmalser-msft"/>

#<a name="tutorial-how-to-integrate-salesforce-with-azure-active-directory"></a>Praktični vodič: Kako integrirati Salesforce Azure Active Directory

Pomoću ovog praktičnog vodiča vidjet ćete kako se povezati okruženje za Salesforce Azure Active Directory. Ćete saznati kako konfigurirati jedinstvenu prijavu za Salesforce, kako omogućiti dodjele resursa automatiziranog korisnika i dodjela korisnici imaju pristup Salesforce.

##<a name="prerequisites"></a>Preduvjeti

1. Da biste pristupili Azure Active Directory putem [Azure klasični portal](https://manage.windowsazure.com), najprije morate imati valjanu Azure pretplatu.

2. U [Salesforce.com](https://www.salesforce.com/)morate imati valjani klijenta.

> [AZURE.IMPORTANT] Ako koristite račun za **Probno razdoblje** Salesforce.com, pa vam neće biti moguće konfigurirati dodjele resursa automatiziranog korisnika. Probne računi imaju API pristupna za omogućeno dok se ne mogu se kupiti.
> 
> To ograničenje mogu snalaženje pomoću [računa za razvojne inženjere besplatne](https://developer.salesforce.com/signup) za dovršetak ovog praktičnog vodiča.

Ako koristite Salesforce testnog okruženja, pročitajte članak [Vodič za integraciju s memorijom za testiranje Salesforce](https://go.microsoft.com/fwLink/?LinkID=521879).

##<a name="video-tutorials"></a>Vodiči za videozapis

Možda slijedite ovaj Praktični vodič pomoću videozapise u nastavku.

**Ugrađenim prvi dio: kako omogućiti jedinstvenu prijavu**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-enable-single-sign-on]

**Ugrađenim dio dvije: Kako automatizirati dodjeljivanje korisnika**

> [AZURE.VIDEO integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning]

##<a name="step-1-add-salesforce-to-your-directory"></a>Korak 1: Dodavanje Salesforce direktorija

1. [Azure klasični portal](https://manage.windowsazure.com)u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![U lijevom navigacijskom oknu odaberite servisa Active Directory.][0]

2. Popis **direktorija** odaberite direktorija koji želite dodati Salesforce da biste.

3. Na gornjoj izborniku kliknite **aplikacije** .

    ![Kliknite aplikacije.][1]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Kliknite Dodaj da biste dodali nove aplikacije.][2]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Kliknite Dodaj aplikaciju iz galerije.][3]

6. U **okvir za pretraživanje**upišite **Salesforce**. Zatim odaberite **Salesforce** među rezultatima i kliknite **dovrši** da biste dodali aplikaciju.

    ![Dodajte Salesforce.][4]

7. Sada trebali biste vidjeti stranice za brzo pokretanje za Salesforce:

    ![Brzi početak stranice Salesforce, u Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Korak 2: Omogućili jedinstvenu prijavu

1. Prije nego što možete konfigurirati jedinstvenu prijavu, morate postaviti i implementirati prilagođenu domenu za okruženje sustava Salesforce. Upute o tome kako to učiniti potražite u članku [Postavljanje domene naziv](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_setup.htm&language=en_US).

2. Na Salesforce, brzi Start stranice u Azure AD, kliknite gumb **Konfiguracija jedinstvenu prijavu** .

    ![Konfiguriranje jedan prijave gumb][6]

3. Otvorit će se dijaloški okvir i vidjet ćete zaslona s pitanjem "Kako biste željeli korisnika da biste se prijavili Salesforce?" Odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Odaberite Azure AD jedinstvene prijave][7]

    > [AZURE.NOTE] Dodatne informacije o različitim jedan prijave mogućnostima, [kliknite ovdje](../active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work)

4. Na stranici **Konfiguracija postavki aplikacije** popunite **Prijavite se na URL** upisivanjem u URL domene Salesforce pomoću sljedećih oblika:
 - Račun tvrtke:`https://<domain>.my.salesforce.com`
 - Račun za razvojne inženjere:`https://<domain>-dev-ed.my.salesforce.com` 

    ![Upišite prijavu na URL-a][8]

5. Na stranici **Konfiguracija jedinstvenu prijavu na Salesforce** kliknite **Preuzmite certifikat**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Preuzmite certifikat][9]

6. Otvaranje na novoj kartici u preglednik i prijavite se na račun servisa Salesforce administrator.

7. U navigacijskom oknu **Administrator** kliknite **Sigurnosne kontrole** proširite odjeljak povezani. Zatim na **Jednom postavke za prijavu**.

    ![Kliknite jedan prijave postavke u odjeljku Sigurnost kontrole][10]

8. Na stranici **Postavke za jedan znak na** kliknite gumb **Uredi** .

    ![Kliknite gumb za uređivanje][11]

    > [AZURE.NOTE] Ako ne možete omogućiti postavke jedinstvene prijave za svoj račun servisa Salesforce, možda ćete morati obratite se podršci za Salesforce, da bi se značajka omogućena.

9. Odaberite **SAML omogućena**, a zatim kliknite **Spremi**.

    ![Odaberite SAML omogućeno][12]

10. Da biste konfigurirali SAML jedan prijave postavke, kliknite **Novo**.

    ![Odaberite SAML omogućeno][13]

11. Na stranici **SAML jedan prijave postavka uređivanje** provjerite sljedećih konfiguracija:

    ![Snimka zaslona konfiguracija koji trebali napraviti][14]

 - Za polje **naziv** Upišite neslužbeni naziv za tu konfiguraciju. Ponuda vrijednost za **naziv** automatsko popunjavanje tekstni okvir **Naziv API -JA** .

 - U Azure AD kopirajte vrijednost **Izdavač URL** i pa ih zalijepite u polje **izdavača** u Salesforce.

 - U **tekstni okvir Id entitet**, upišite naziv domene Salesforce pomoću sljedećeg uzorka:
     - Račun tvrtke:`https://<domain>.my.salesforce.com`
     - Račun za razvojne inženjere:`https://<domain>-dev-ed.my.salesforce.com`

 - Kliknite **Pregledaj** ili **Odaberite datoteku** da biste otvorili dijaloški okvir **Odabir datoteke za prijenos** odaberite certifikat Salesforce, a zatim kliknite **Otvori** da biste prenijeli certifikata.

 - **Vrsta identiteta SAML**odaberite **pridruživanju sadrži korisničko ime korisnika salesforce.com**.

 - **Mjesto identiteta SAML**, odaberite **identitet se element NameIdentifier izjave predmet**

 - U Azure AD kopirajte **URL daljinskog prijava** vrijednost i zatim je zalijepite u polje **URL za prijavu davatelja identiteta** u Salesforce.

 - **Servis davatelja pokrenut zahtjev za povezivanje**, odaberite **HTTP preusmjeravanje**.

 - Kliknite **Spremi** da biste primijenili SAML jedan prijave postavki.

12. U lijevom navigacijskom oknu u Salesforce, kliknite **Upravljanje domenom** proširite odjeljak povezani, a zatim **Moje domene**.

    ![Kliknite na mojoj domeni][15]

13. Pomaknite se prema dolje do odjeljka **Konfiguriranje provjere autentičnosti** pa kliknite gumb **Uredi** .

    ![Kliknite gumb za uređivanje][16]

14. U odjeljku **Servis za provjeru autentičnosti** odaberite neslužbeni naziv konfiguracijom SAML SSO pa zatim kliknite **Spremi**.

    ![Odaberite konfiguraciju jedinstvene Prijave][17]

    > [AZURE.NOTE] Ako je odabrano više servis za provjeru autentičnosti, zatim kada korisnici pokušaju pokretanje jedinstvenu prijavu u okruženju sustava Salesforce bit će mu ponuđena da biste odabrali koji servis za provjeru autentičnosti ih želite prijaviti s. Ako ne želite da se to dogodi, zatim trebali biste **ostavite sve druge servise za provjeru autentičnosti poništen**.

15. U Azure AD, odaberite potvrdni okvir potvrdu konfiguracije jedan prijave da biste omogućili certifikat koji ste prenijeli Salesforce. Zatim kliknite **Dalje**.

    ![Potvrdite okvir za potvrdu][18]

16. Na posljednjoj stranici dijaloški okvir upišite u adrese e-pošte ako želite primati obavijesti putem e-pošte za pogrešaka i upozorenja vezane uz održavanje konfiguraciju jedan prijave. 

    ![Upišite adresu e-pošte.][19]

17. Kliknite **dovrši** da biste zatvorili dijaloški okvir. Da biste testirali konfiguraciju, potražite u odjeljku ispod pod naslovom [Dodjela korisnicima Salesforce](#step-4-assign-users-to-salesforce).

##<a name="step-3-enable-automated-user-provisioning"></a>Korak 3: Omogući automatsko dodjeljivanje korisnika

1. Na stranici Azure AD brzi početak rada za Salesforce, kliknite gumb **Konfiguracija korisnika dodjele resursa** .

    ![Kliknite gumb Konfiguracija korisnika dodjele resursa][20]

2. U dijaloški okvir **Konfiguracija korisnika dodjeljivanje** upišite svoje korisničko ime za administratore servisa Salesforce i lozinku.

    ![Upišite administrator korisničko ime ili lozinka][21]

    > [AZURE.NOTE] Ako konfigurirate radnog okruženja, najbolje je da biste stvorili novi račun za administratore u Salesforce posebno za ovaj korak. Račun mora imati dodijeljena u Salesforce profil **Administratora sustava** .

3. Da biste dobili tokena sigurnosti Salesforce, otvorite novu karticu i prijavite u istom Salesforce administratorskog računa. U gornjem desnom kutu stranice kliknite svoje ime, a zatim na **Moje postavke**.

    ![Kliknite svoje ime, a zatim kliknite na moje postavke][22]

4. U lijevom navigacijskom oknu kliknite na **osobne** proširite odjeljak povezani, a zatim na **Ponovno postavi moj sigurnosni Token**.

    ![Kliknite svoje ime, a zatim kliknite na moje postavke][23]

5. Na stranici **Ponovno postavi moj sigurnosni Token** kliknite gumb **Vrati sigurnosni Token** .

    ![Pročitajte upozorenja.][24]

6. Provjera ulazne pošte povezan s ovim računom za administratore. Potražite poruku e-pošte iz Salesforce.com koja sadrži novi sigurnosni token.

7. Kopiranje token, otvorite prozor programa Azure AD i zalijepite ih u polje **Korisničkog sigurnosni Token** . Zatim kliknite **Dalje**.

    ![Zalijepite u sigurnosnog tokena][25]

8. Na stranici za potvrdu možete primati obavijesti putem e-pošte za dodjelu resursa neuspjeha dogoditi. Kliknite **dovrši** da biste zatvorili dijaloški okvir.

    ![Upišite adresu e-pošte da biste primali obavijesti][26]

##<a name="step-4-assign-users-to-salesforce"></a>Korak 4: Dodjela korisnika u Salesforce

1. Da biste testirali konfiguraciju, najprije stvaranje novog računa test u direktoriju.

2. Na stranici Salesforce brzi početak rada kliknite gumb **Dodijeliti korisnicima** .

    ![Kliknite Dodijeli korisnika][27]

3. Odaberite korisnički test pa kliknite gumb **Dodjela** pri dnu zaslona:

 - Ako to još niste omogućili dodjele resursa automatiziranog korisnika, a vidjet ćete sljedeći upit da biste potvrdili:

        ![Confirm the assignment.][28]

 - Ako ste omogućili automatiziranog korisnika dodjele resursa, zatim vidjet ćete upit za definiranje mora imati vrstu Salesforce profila korisnika. Upravo dodijeljenu korisnicima prikazivati u svom okruženju Salesforce nakon nekoliko minuta.

        ![Confirm the assignment.][29]

        > [AZURE.IMPORTANT] Ako su dodjele resursa u okruženje **za razvojne inženjere** Salesforce, imat ćete vrlo ograničeni broj licenci koje su dostupne za svaki profil. Dakle, najbolje je za implementaciju korisnika **Chatter besplatni korisnički** profil koji sadrži 4,999 raspoloživu licencu.

4. Testirajte postavke jedan prijave, otvorili ploču za pristup pri [https://myapps.microsoft.com](https://myapps.microsoft.com/), a zatim prijavite se u obzir test pa kliknite na **Salesforce**.

##<a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Popis vodiče za integraciju SaaS aplikacije](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-salesforce-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-salesforce-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-salesforce-tutorial/add-app.png
[3]: ./media/active-directory-saas-salesforce-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-salesforce-tutorial/add-salesforce.png
[5]: ./media/active-directory-saas-salesforce-tutorial/salesforce-added.png
[6]: ./media/active-directory-saas-salesforce-tutorial/config-sso.png
[7]: ./media/active-directory-saas-salesforce-tutorial/select-azure-ad-sso.png
[8]: ./media/active-directory-saas-salesforce-tutorial/config-app-settings.png
[9]: ./media/active-directory-saas-salesforce-tutorial/download-certificate.png
[10]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso.png
[11]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-edit.png
[12]: ./media/active-directory-saas-salesforce-tutorial/sf-enable-saml.png
[13]: ./media/active-directory-saas-salesforce-tutorial/sf-admin-sso-new.png
[14]: ./media/active-directory-saas-salesforce-tutorial/sf-saml-config.png
[15]: ./media/active-directory-saas-salesforce-tutorial/sf-my-domain.png
[16]: ./media/active-directory-saas-salesforce-tutorial/sf-edit-auth-config.png
[17]: ./media/active-directory-saas-salesforce-tutorial/sf-auth-config.png
[18]: ./media/active-directory-saas-salesforce-tutorial/sso-confirm.png
[19]: ./media/active-directory-saas-salesforce-tutorial/sso-notification.png
[20]: ./media/active-directory-saas-salesforce-tutorial/config-prov.png
[21]: ./media/active-directory-saas-salesforce-tutorial/config-prov-dialog.png
[22]: ./media/active-directory-saas-salesforce-tutorial/sf-my-settings.png
[23]: ./media/active-directory-saas-salesforce-tutorial/sf-personal-reset.png
[24]: ./media/active-directory-saas-salesforce-tutorial/sf-reset-token.png
[25]: ./media/active-directory-saas-salesforce-tutorial/got-the-token.png
[26]: ./media/active-directory-saas-salesforce-tutorial/prov-confirm.png
[27]: ./media/active-directory-saas-salesforce-tutorial/assign-users.png
[28]: ./media/active-directory-saas-salesforce-tutorial/assign-confirm.png
[29]: ./media/active-directory-saas-salesforce-tutorial/assign-sf-profile.png
