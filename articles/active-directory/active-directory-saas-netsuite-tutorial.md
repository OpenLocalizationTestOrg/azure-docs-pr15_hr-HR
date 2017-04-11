<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s NetSuite | Microsoft Azure"
    description="Saznajte kako koristiti NetSuite s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!"
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

#<a name="tutorial-how-to-integrate-netsuite-with-azure-active-directory"></a>Praktični vodič: Kako integrirati NetSuite Azure Active Directory

Pomoću ovog praktičnog vodiča vidjet ćete kako se povezati NetSuite okruženju sustava Azure Active Directory (Azure AD). Ćete saznati kako konfigurirati jedinstvenu prijavu na NetSuite, kako omogućiti dodjele resursa automatiziranog korisnika i dodjela korisnici imaju pristup NetSuite. 

##<a name="prerequisites"></a>Preduvjeti

1. Da biste pristupili Azure Active Directory putem [Azure klasični portal](https://manage.windowsazure.com), najprije morate imati valjanu Azure pretplatu.

2. Morate imati administratorski pristup [NetSuite](http://www.netsuite.com/portal/home.shtml) pretplatu. Besplatne probne račun koji koristite za neki servis.

##<a name="step-1-add-netsuite-to-your-directory"></a>Korak 1: Dodavanje NetSuite direktorija

1. [Azure klasični portal](https://manage.windowsazure.com)u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![U lijevom navigacijskom oknu odaberite servisa Active Directory.][0]

2. Popis **direktorija** odaberite direktorija koji želite dodati NetSuite da biste.

3. Na gornjoj izborniku kliknite **aplikacije** .

    ![Kliknite aplikacije.][1]

4. Kliknite **Dodaj** pri dnu stranice.

    ![Kliknite Dodaj da biste dodali nove aplikacije.][2]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Kliknite Dodaj aplikaciju iz galerije.][3]

6. U **okvir za pretraživanje**upišite **NetSuite**. Zatim odaberite **NetSuite** među rezultatima i kliknite **dovrši** da biste dodali aplikaciju.

    ![Dodajte NetSuite.][4]

7. Sada trebali biste vidjeti stranice za brzo pokretanje za NetSuite:

    ![Brzi početak stranice NetSuite korisnika u Azure AD][5]

##<a name="step-2-enable-single-sign-on"></a>Korak 2: Omogućili jedinstvenu prijavu

1. U Azure AD, na stranici za brzi početak rada za NetSuite, kliknite gumb **Konfiguracija jedinstvenu prijavu** .

    ![Konfiguriranje jedan prijave gumb][6]

2. Otvorit će se dijaloški okvir i vidjet ćete zaslona s pitanjem "Kako biste željeli korisnika da biste se prijavili NetSuite?" Odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Odaberite Azure AD jedinstvene prijave][7]

    > [AZURE.NOTE] Dodatne informacije o različitim jedan prijave mogućnostima, [kliknite ovdje](../active-directory-appssoaccess-whatis/#how-does-single-sign-on-with-azure-active-directory-work)

3. Na stranici **Konfiguracija postavki aplikacije** za polja **Odgovor URL-a** upišite URL klijentu NetSuite pomoću jednog od sljedećih oblika:
    - `https://<tenant-name>.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.netsuite.com/saml2/acs`
    - `https://<tenant-name>.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na1.sandbox.netsuite.com/saml2/acs`
    - `https://<tenant-name>.na2.sandbox.netsuite.com/saml2/acs`

    ![Upišite u klijentu URL-a][8]

4. Na stranici **Konfiguracija jedinstvenu prijavu na NetSuite** kliknite **Preuzimanje metapodataka**, a zatim spremite datoteku certifikata lokalno na vašem računalu.

    ![Preuzmite metapodatke.][9]

5. Otvaranje na novoj kartici u pregledniku i prijavite se u vašem Netsuite web-mjesto tvrtke kao administrator.

6. Na alatnoj traci pri vrhu stranice kliknite **Postavljanje**, a zatim kliknite **Upravitelj instalacijskog programa**.

    ![Otvorite Upravitelj instalacijskog programa][10]

7. Popis **Zadataka postavljanja** odaberite **integracije**.

    ![Otvorite integraciju sustava][11]

8. U odjeljku **Upravljanje provjere autentičnosti** kliknite **SAML jedinstvenu prijavu**.

    ![Idite na SAML jedinstvenu prijavu][12]

9. Na stranici **Postavljanje SAML** poduzeti sljedeće korake:

    - U Azure Active Directory, kopirajte **URL daljinskog prijava** vrijednost i zalijepite je u **Stranicu za prijavu davatelja identiteta** polja u NetSuite.

        ![Stranica za postavljanje SAML.][13]

    - U NetSuite, odaberite **Primarni način provjere autentičnosti**.

    - Za svako polje koje su označene **Metapodataka za davatelja identiteta SAMLV2**odaberite **Prenesi datoteku IDP metapodataka**. Kliknite **Pregledaj** da biste prenijeli datoteku metapodataka koji ste preuzeli u koraku #4.

        ![Prijenos metapodatke][16]

    - Kliknite **Pošalji**.

9. U Azure AD, odaberite potvrdni okvir potvrdu konfiguracije jedan prijave da biste omogućili certifikat koji ste prenijeli NetSuite. Zatim kliknite **Dalje**.

    ![Potvrdite okvir za potvrdu][14]

10. Na posljednjoj stranici dijaloški okvir upišite u adrese e-pošte ako želite primati obavijesti putem e-pošte za pogrešaka i upozorenja vezane uz održavanje konfiguraciju jedan prijave. 

    ![Upišite adresu e-pošte.][15]

11. Kliknite **dovrši** da biste zatvorili dijaloški okvir. Nakon toga kliknite **atribute** pri vrhu stranice.

    ![Kliknite atribute.][17]

12. Kliknite **Dodavanje korisnika atribut**.

    ![Kliknite Dodavanje atributa korisnika.][18]

13. Za svako polje **Naziv atributa** upišite u `account`. Za svako polje **Vrijednost atributa** upišite svoj ID za račun NetSuite. Upute za pronalaženje svoj ID računa nalaze se ispod:

    ![Dodajte svoj NetSuite ID računa.][19]

    - U NetSuite, kliknite **Postavljanje** gornjem navigacijskom izborniku.
    - Zatim u odjeljku **Zadaci postavljanja** lijevi navigacijski izbornik, odaberite sekciju za **integraciju** i kliknite **Preference Web Services**.
    - Kopirajte NetSuite računa ID-a i zalijepite ih u polje **Vrijednost atributa** u Azure AD.

        ![Dobiti svoj ID računa][20]

14. U Azure AD kliknite **dovrši** da biste završili Dodavanje atributa SAML. Na izborniku dna pa kliknite **Primijeni promjene** .

    ![Spremite promjene.][21]

15. Da bi korisnici mogu izvršavati jedinstvenu prijavu u NetSuite, oni morate najprije se dodijeliti odgovarajuće dozvole u NetSuite. Slijedite upute u nastavku da biste dodijelili te dozvole.

    - Na gornjem navigacijskom izborniku kliknite **Postavljanje**, a zatim kliknite **Upravitelj instalacijskog programa**.

        ![Otvorite Upravitelj instalacijskog programa][10]

    - U lijevom navigacijskom izborniku odaberite **Korisnika/uloge**, a zatim kliknite **Upravljanje ulogama**.

        ![Idi na Upravljanje ulogama][22]

    - Kliknite **Novo radno mjesto**.

    - Upišite **naziv** koji novo radno mjesto, a zatim potvrdite okvir **Jedan prijave samo** .

        ![Naziv nove uloge.][23]

    - Kliknite **Spremi**.

    - Na izborniku na vrhu kliknite **dozvole**. Zatim kliknite **Postavljanje**.

        ![Idite na dozvole][24]

    - Odaberite **Postavljanje gore SAM jedinstvenu prijavu**, a zatim kliknite **Dodaj**.

    - Kliknite **Spremi**.

    - Na gornjem navigacijskom izborniku kliknite **Postavljanje**, a zatim kliknite **Upravitelj instalacijskog programa**.

        ![Otvorite Upravitelj instalacijskog programa][10]

    - U lijevom navigacijskom izborniku odaberite **Korisnika/uloge**, a zatim kliknite **Upravljanje korisnicima**.

        ![Idi na Upravljanje korisnicima][25]

    - Odabir korisnika za testiranje. Zatim kliknite **Uredi**.

        ![Idi na Upravljanje korisnicima][26]

    - U dijaloškom okviru uloge odaberite ulogu koji ste stvorili i kliknite **Dodaj**.

        ![Idi na Upravljanje korisnicima][27]

    - Kliknite **Spremi**.

19. Da biste testirali konfiguraciju, potražite u odjeljku ispod pod naslovom [Dodjela korisnicima NetSuite](#step-4-assign-users-to-netsuite).

##<a name="step-3-enable-automated-user-provisioning"></a>Korak 3: Omogući automatsko dodjeljivanje korisnika

> [AZURE.NOTE] Prema zadanim postavkama, dodijeljenu korisnici će se dodati podružnici korijen od NetSuite okruženju.

1. U Azure Active Directory, na stranici za brzi početak rada za NetSuite, kliknite **Konfiguriraj korisnika dodjele resursa**.

    ![Konfiguriranje dodjeljivanje korisnika][28]

2. U dijaloškom okviru koji će se otvoriti upišite u administratorskih vjerodajnica za NetSuite, a zatim kliknite **Dalje**.

    ![Upišite NetSuite administratorske vjerodajnice.][29]

3. Na stranici za potvrdu upišite adresu e-pošte za primanje obavijesti o dodjeljivanje pogreške.

    ![Provjerite je li.][30]

4. Kliknite **dovrši** da biste zatvorili dijaloški okvir.

##<a name="step-4-assign-users-to-netsuite"></a>Korak 4: Dodjela korisnika u NetSuite

1. Da biste testirali konfiguraciju, pokrenite stvaranje novog računa test u direktoriju.

2. Na stranici NetSuite brzi početak rada kliknite gumb **Dodijeliti korisnicima** .

    ![Kliknite Dodijeli korisnika][31]

3. Odaberite korisnički test pa kliknite gumb **Dodjela** pri dnu zaslona:

 - Ako to još niste omogućili dodjele resursa automatiziranog korisnika, a vidjet ćete sljedeći upit da biste potvrdili:

        ![Confirm the assignment.][32]

 - Ako ste omogućili automatiziranog korisnika dodjele resursa, zatim vidjet ćete upit da biste odredili vrstu uloge korisnika morala bi NetSuite. Upravo dodijeljenu korisnicima prikazivati u svom okruženju NetSuite nakon nekoliko minuta.

4. Testirajte postavke jedan prijave, otvorili ploču za pristup pri [https://myapps.microsoft.com](https://myapps.microsoft.com/), prijavite se u obzir test pa kliknite na **NetSuite**.

##<a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Popis vodiče za integraciju SaaS aplikacije](active-directory-saas-tutorial-list.md)

[0]: ./media/active-directory-saas-netsuite-tutorial/azure-active-directory.png
[1]: ./media/active-directory-saas-netsuite-tutorial/applications-tab.png
[2]: ./media/active-directory-saas-netsuite-tutorial/add-app.png
[3]: ./media/active-directory-saas-netsuite-tutorial/add-app-gallery.png
[4]: ./media/active-directory-saas-netsuite-tutorial/add-netsuite.png
[5]: ./media/active-directory-saas-netsuite-tutorial/quick-start-netsuite.png
[6]: ./media/active-directory-saas-netsuite-tutorial/config-sso.png
[7]: ./media/active-directory-saas-netsuite-tutorial/sso-netsuite.png
[8]: ./media/active-directory-saas-netsuite-tutorial/sso-config-netsuite.png
[9]: ./media/active-directory-saas-netsuite-tutorial/config-sso-netsuite.png
[10]: ./media/active-directory-saas-netsuite-tutorial/ns-setup.png
[11]: ./media/active-directory-saas-netsuite-tutorial/ns-integration.png
[12]: ./media/active-directory-saas-netsuite-tutorial/ns-saml.png
[13]: ./media/active-directory-saas-netsuite-tutorial/ns-saml-setup.png
[14]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-confirm.png
[15]: ./media/active-directory-saas-netsuite-tutorial/sso-email.png
[16]: ./media/active-directory-saas-netsuite-tutorial/ns-sso-setup.png
[17]: ./media/active-directory-saas-netsuite-tutorial/ns-attributes.png
[18]: ./media/active-directory-saas-netsuite-tutorial/ns-add-attribute.png
[19]: ./media/active-directory-saas-netsuite-tutorial/ns-add-account.png
[20]: ./media/active-directory-saas-netsuite-tutorial/ns-account-id.png
[21]: ./media/active-directory-saas-netsuite-tutorial/ns-save-saml.png
[22]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-roles.png
[23]: ./media/active-directory-saas-netsuite-tutorial/ns-new-role.png
[24]: ./media/active-directory-saas-netsuite-tutorial/ns-sso.png
[25]: ./media/active-directory-saas-netsuite-tutorial/ns-manage-users.png
[26]: ./media/active-directory-saas-netsuite-tutorial/ns-edit-user.png
[27]: ./media/active-directory-saas-netsuite-tutorial/ns-add-role.png
[28]: ./media/active-directory-saas-netsuite-tutorial/netsuite-provisioning.png
[29]: ./media/active-directory-saas-netsuite-tutorial/ns-creds.png
[30]: ./media/active-directory-saas-netsuite-tutorial/ns-confirm.png
[31]: ./media/active-directory-saas-netsuite-tutorial/assign-users.png
[32]: ./media/active-directory-saas-netsuite-tutorial/assign-confirm.png
