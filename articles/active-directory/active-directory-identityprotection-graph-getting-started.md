<properties
    pageTitle="Početak rada s Azure Active Directory identiteta zaštitu i Microsoft Graph | Microsoft Azure"
    description="Nudi Uvod u Microsoft Graph upita za popis te rizika događaje povezane podatke iz servisa Azure Active Directory."
    services="active-directory"
    keywords="Zaštita identiteta servisa Azure active directory, rizika događaj, slabe, Microsoft Graph sigurnosna pravila"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="markvi"/>

# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Početak rada s Azure Active Directory identiteta zaštitu i Microsoft Graph

Microsoft Graph je krajnja točka za API objedinjenih tvrtke Microsoft i Polazno [Azure Active Directory identiteta zaštitu,](active-directory-identityprotection.md) API-ji. Naš prvi API **identityRiskEvents**omogućuje upita Microsoft Graph za popis te [rizika događaje](active-directory-identityprotection-risk-events-types.md) povezane podatke. U ovom se članku upoznat će vas ispitivanje ovaj API-JA. Za u dubine Uvod, cijeli dokumentaciju i pristup Explorer grafikonu potražite u članku [Microsoft Graph web-mjesta](https://graph.microsoft.io/).

Postoje tri koraka za pristup podacima identiteta zaštitu kroz Microsoft Graph:

1. Dodajte aplikaciju s klijentskim tajna. 

2. Pomoću ovog tajna i nekoliko ostale informacije za provjeru autentičnosti Microsoft Graph, koji primate token za provjeru autentičnosti. 

3. Koristite ovaj token za upućivanje zahtjeva za krajnjoj API-JA i vratiti identiteta zaštita podataka.

Prije nego što počnete, morat ćete:

- Administratorska prava za stvaranje aplikacija u Azure AD
- Naziv domene za vaš klijent (na primjer, contoso.onmicrosoft.com)



## <a name="add-an-application-with-a-client-secret"></a>Dodavanje aplikacije s klijentskim tajna


1. [Prijavite se](https://manage.windowsazure.com) na portal za Azure klasični kao administrator. 

1. Na lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Na izborniku na vrhu kliknite **aplikacije**.

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)

4. Kliknite **Dodaj** pri dnu stranice.

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju je moje tvrtke ili ustanove razvoju**.

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)


5. U dijaloškom okviru **Recite nam o aplikaciji** poduzeti sljedeće korake:

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)

    na. U tekstni okvir **naziv** upišite naziv aplikacije (npr.: AADIP rizika događaj API aplikacija).

    b. Kao **vrstu**odaberite **aplikaciju i/ili web-API na webu**.

    c. Kliknite **Dalje**.


5. U dijaloškom okviru **Svojstva aplikacije** poduzeti sljedeće korake:

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)

    na. U tekstni okvir **URL za prijavu** upišite `http://localhost`.

    b. U tekstni okvir **Aplikacije ID URI** upišite `http://localhost`.

    c. Kliknite **dovrši**.


Možete konfigurirati sada aplikacije.

![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)



## <a name="grant-your-application-permission-to-use-the-api"></a>Dopuštanje programu dozvolu za korištenje na API-JA


1. Na stranici vaše aplikacije, na izborniku na vrhu kliknite **Konfiguriraj**. 

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)

2. U odjeljku **dozvole na druge aplikacije** kliknite **Dodaj aplikaciju**.

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)


2. U dijaloškom okviru **dozvole drugim aplikacijama** poduzeti sljedeće korake:

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)

    na. Odaberite **Microsoft Graph**.

    b. Kliknite **dovrši**.


1. Kliknite **dozvola za aplikaciju: 0**, a zatim odaberite **čitati sve podatke za identitet rizika događaj**.

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)

1. Kliknite **Spremi** pri dnu stranice.

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)


## <a name="get-an-access-key"></a>Početak tipkovni prečac

1. Na stranici vaše aplikacije u odjeljku **tipke** odaberite godinu kao trajanje.

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)

1. Kliknite **Spremi** pri dnu stranice.

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

1. u odjeljku tipke kopirajte vrijednost novostvoreni ključ i pa ih zalijepite u sigurnom mjestu.

    ![Stvaranje aplikacije](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)

    > [AZURE.NOTE] Ako izgubite ovog ključa, morat ćete vratite se u ovom odjeljku i stvaranje novog ključa. Ovaj ključ zadržavanje na tajna: možete svakome tko ima pristup vašim podacima.


1. U odjeljku **Svojstva** kopirajte **ID klijenta**, a zatim je zalijepite u sigurnom mjestu. 


## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a>Autentičnost Microsoft Graph i događaje API-JA rizika identiteta za upite

Sada, imat ćete:

- ID klijenta koji ste prethodno kopirali

- Ključ koji ste prethodno kopirali

- Naziv domene za vaš klijent


Za provjeru autentičnosti, pošaljite zahtjev za objavu da biste `https://login.microsoft.com` pomoću sljedećih parametara u tijelu:

- grant_type: "**client_credentials**"

- resursa: "**https://graph.microsoft.com**"

- client_id:<your client ID>

- client_secret:<your key>


> [AZURE.NOTE] Morate unijeti vrijednosti za **client_id** i parametar **client_secret** .

Ako ne uspije, to vraća token za provjeru autentičnosti.  
Da biste nazvali na API-JA, stvorite zaglavlja s parametrom sljedeće:

    `Authorization`=”<token_type> <access_token>"


Prilikom provjere autentičnosti, možete pronaći tokena vrstu i pristupni token u vraćenom token.

Pošaljite tog zaglavlja kao zahtjev sljedeći URL API-JA:`https://graph.microsoft.com/beta/identityRiskEvents`

Odgovor, ako ne uspije, je zbirka identiteta rizika događaja i povezane podatke u obliku OData JSON koji mogu raščlaniti i provode kao svojim potrebama.

Evo uzorak koda za provjere autentičnosti i pozivanje API pomoću komponente Powershell.  
Jednostavno dodajte svoj ID klijenta ključ i smjernice za domenu.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Daljnji koraci

Čestitamo, koju ste upravo načinili prvi poziv Microsoft Graph!  
Sada upit identiteta rizika događaja i koristiti podatke, ali svojim potrebama.

Da biste saznali više o web-mjesto Microsoft Graph i u okvir za sastavljanje aplikacije koje koriste API grafikonu, provjerite [dokumentaciju](https://graph.microsoft.io/docs) i mnogo više na [web-mjestu Microsoft Graph](https://graph.microsoft.io/). Osim toga, provjerite je li da biste označili stranici [Azure AD identiteta zaštita API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) svih API zaštite za identitet u grafikonu. Kao što smo dodali novi Načini rada s identiteta zaštita putem API-JA, vidjet ćete ih na toj stranici.


## <a name="additional-resources"></a>Dodatni resursi

- [Zaštita identiteta Azure Active Directory](active-directory-identityprotection.md)

- [Vrste događaja rizika otkrio Azure Active Directory identiteta zaštitu](active-directory-identityprotection-risk-events-types.md)

- [Microsoft Graph](https://graph.microsoft.io/)

- [Pregled programa Microsoft Graph](https://graph.microsoft.io/docs)

- [Azure AD identiteta zaštita servisa korijen](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)
