<properties
    pageTitle="Preduvjeti za pristup Azure AD izvješćivanja API-JA. | Microsoft Azure"
    description="Dodatne informacije o preduvjetima za pristup Azure AD API-JA za izvješćivanje o pogreškama"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a>Preduvjeti za pristup Azure AD izvješćivanje API-JA 

[Azure AD izvješćivanja API-ji](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) ponuditi programatski pristup podacima kroz skup utemeljen na REST API-ji. Ove API-ji možete pozvati iz raznih programskog jezika i alati.

Izvješćivanje API koristi [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) da biste autorizirali API-ji web-mjesto. 

Da biste pripremili pristup izvješćivanja API, morate:

1. Stvaranje aplikacije komponente u klijentu za Azure AD 

2. Dodjela dozvole za aplikaciju odgovarajuće dozvole za pristup podacima Azure AD

3. Prikupite konfiguracijske postavke iz direktorija

Pitanja, probleme i povratne informacije, zatražite [Pomoć za izvješćivanje o pogreškama AAD](mailto:aadreportinghelp@microsoft.com).


## <a name="create-an-azure-ad-application"></a>Stvaranje aplikacije komponente Azure AD

Da biste konfigurirali direktorija za pristup Azure AD izvješćivanja API, morate se prijaviti Azure klasični portal pomoću administratorskog računa Azure pretplatu koja je član uloge direktorija globalni Administrator u klijentu za Azure AD.

> [AZURE.IMPORTANT] Aplikacije koje rade u odjeljku vjerodajnice s ovlastima "administrator" ovako može biti vrlo Napredna, stoga svakako zaštiti aplikacije ID-tajna vjerodajnice.


1. [Azure klasični portal](https://manage.windowsazure.com)u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Na popisu **servisa active directory** odaberite direktorija.

3. Na izborniku na vrhu kliknite **aplikacije**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Na donjoj traci kliknite **Dodaj**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/03.png) 

5. Na na **što želite učiniti?** dijaloški okvir, kliknite **Dodaj aplikaciju je moje tvrtke ili ustanove razvoju**. 

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/04.png) 


6. U dijaloškom okviru **Recite nam o aplikaciji** poduzeti sljedeće korake: 

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/05.png) 

    na. U tekstni okvir **naziv** upišite naziv (npr.: aplikacija za izvješćivanje o pogreškama API-JA).

    b. Odaberite **web-aplikacije i/ili web API**.

    c. Kliknite **Dalje**.


7. U dijaloškom okviru **Svojstva aplikacije** poduzeti sljedeće korake: 

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/06.png) 

    na. U tekstni okvir **URL za prijavu** upišite `https://localhost`.

    b. U tekstni okvir **Aplikacija ID URI** upišite ```https://localhost/ReportingApiApp```.

    c. Kliknite **dovrši**.



## <a name="grant-your-application-permission-to-use-the-api"></a>Dopuštanje programu dozvolu za korištenje na API-JA

1. [Azure klasični portal](https://manage.windowsazure.com/)u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Na popisu **servisa active directory** odaberite direktorija.

3. Na izborniku na vrhu kliknite **aplikacije**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/02.png)


3. Na popisu aplikacija odaberite novostvorenu aplikacije.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/07.png)

4. Na izborniku na vrhu kliknite **Konfiguriraj**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/08.png)


5. U odjeljku **dozvole drugim aplikacijama** za **Azure Active Directory** resursa, kliknite padajući popis **Dozvola za aplikaciju** , a zatim **čitanje direktorija podataka**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/09.png)


5. Na donjoj traci kliknite **Spremi**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/10.png)


## <a name="gather-configuration-settings-from-your-directory"></a>Prikupite konfiguracijske postavke iz direktorija

U ovom se odjeljku objašnjava se sljedeće postavke iz imenika:

- Naziv domene
- ID klijenta
- Tajna klijenta

Morate te vrijednosti prilikom konfiguriranja poziva za izvješćivanje API-JA. 


### <a name="get-your-domain-name"></a>Dohvaćanje naziva domene

1. [Azure klasični portal](https://manage.windowsazure.com)u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Na popisu **servisa active directory** odaberite direktorija.

3. Na izborniku na vrhu kliknite **domene**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/11.png) 

4. U stupcu **Naziv domene** , kopirajte naziv vaše domene.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/12.png) 


### <a name="get-the-applications-client-id"></a>Dohvaćanje aplikacije ID klijenta

1. [Azure klasični portal](https://manage.windowsazure.com)u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Na popisu **servisa active directory** odaberite direktorija.

3. Na izborniku na vrhu kliknite **aplikacije**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Na popisu aplikacija odaberite novostvorenu aplikacije.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/07.png)

4. Na izborniku na vrhu kliknite **Konfiguriraj**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/08.png)

4. Kopirajte **ID klijenta**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/13.png)


### <a name="get-the-applications-client-secret"></a>Dohvaćanje aplikacije klijenta tajna

Da biste dobili tajna vaše aplikacije klijenta, morate stvoriti novi ključ i spremiti njegovom vrijednošću nakon spremanja novi ključ jer nije moguće dohvatiti tu vrijednost kasnije više.

1. [Azure klasični portal](https://manage.windowsazure.com)u lijevom navigacijskom oknu kliknite **Active Directory**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/01.png) 

2. Na popisu **servisa active directory** odaberite direktorija.

3. Na izborniku na vrhu kliknite **aplikacije**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/02.png) 

4. Na popisu aplikacija odaberite novostvorenu aplikacije.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/07.png)

4. Na izborniku na vrhu kliknite **Konfiguriraj**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/08.png)

5. U odjeljku **tipke** poduzeti sljedeće korake: 

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/14.png)

    na. Na popisu trajanje, odaberite trajanje

    b. Na donjoj traci kliknite **Spremi**.

    ![Registracija aplikacije](./media/active-directory-reporting-api-prerequisites/10.png)

    c. Kopirajte vrijednost ključa.

## <a name="next-steps"></a>Daljnji koraci

- Želite li za pristup podacima iz Azure AD izvješćivanja API programski način? Pogledajte [Početak rada s Azure Active Directory izvješćivanja API -jem](active-directory-reporting-api-getting-started.md).

- Ako želite da biste saznali više o prijavljivanju Azure Active Directory potražite u članku [Azure Active Directory izvješćivanje vodič](active-directory-reporting-guide.md).  
