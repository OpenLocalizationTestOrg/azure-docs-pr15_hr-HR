<properties 
    pageTitle="Provjeru s mobilnim radnje REST API-ji"
    description="U članku se opisuje kako uspješnoj Azure Mobile radnje REST API-ji" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="10/05/2016"
    ms.author="wesmc;ricksal"/>

# <a name="authenticate-with-mobile-engagement-rest-apis"></a>Provjeru s mobilnim radnje REST API-ji

## <a name="overview"></a>Pregled

U ovom dokumentu opisuje način da biste dobili valjane AAD Oauth tokena za provjeru s REST API-ji radnje Mobile. 

Pretpostavlja se da imate valjan Azure pretplatu i stvorite radnje mobilne aplikacije pomoću jednog od naše [Vodiči za razvojne inženjere](mobile-engagement-windows-store-dotnet-get-started.md).

## <a name="authentication"></a>Provjera autentičnosti

U programu Microsoft Azure Active Directory temelji OAuth token služi za provjeru autentičnosti. 

Redoslijedom u zahtjev za provjeru autentičnosti API zaglavlje autorizacije mora biti dodan u svaki zahtjev za koji je obrasca za sljedeće:

    Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGmJlNmV2ZWJPamg2TTNXR1E...

>[AZURE.NOTE] Tokeni Azure Active Directory isteka u 1 sat.

Da biste dobili token na nekoliko načina. Budući da API-ji obično nazivaju se na neki servis u oblaku, koji želite koristiti ključ za API-JA. Ključa API-JA u Azure terminologija zove servisa glavni lozinku. U nastavku opisuju da biste ga ručno postavljanje.

### <a name="one-time-setup-using-script"></a>Jednokratno postavljanje (pomoću skripte)

Morate slijediti skup upute u nastavku da biste izvršili postavljanje pomoću skriptu PowerShell koji traje minimalne za postavljanje, ali koristi najviše dopuštenih zadane postavke. Po želji možete i slijedite upute u [ručno postavljanje](mobile-engagement-api-authentication-manual.md) za to s portala za Azure izravno i učinite precizniji konfiguracije. 

1. Najnoviju verziju programa Azure PowerShell zatražite od [ovdje](http://aka.ms/webpi-azps). Dodatne informacije o upute za preuzimanje, vidjet ćete sljedeću [vezu](../powershell-install-configure.md).  

2. Nakon instalacije Azure PowerShell da biste bili sigurni da imate **Modul Azure** instaliran pomoću sljedeće naredbe:

    na. Provjerite je li modul Azure PowerShell dostupna na popisu dostupnih module. 
    
        Get-Module –ListAvailable 

    ![Dostupne modula Azure][1]
        
    b. Ako ne pronađete modul Azure PowerShell na gornjem popisu morate pokrenite sljedeću naredbu:
        
        Import-Module Azure 
        
3. Prijava na upravitelj resursa Azure s PowerShell pokretanjem sljedeće naredbe i koja omogućuje korisničko ime i lozinku za račun za Azure: 
        
        Login-AzureRmAccount

4. Ako imate više pretplata, a zatim pokrećite na sljedeći način:

    na. Dobit ćete popis svih pretplata i kopirajte SubscriptionId pretplatu koju želite koristiti. Provjerite je li ove pretplate isto ono koja ima mobilnu aplikaciju radnje koje namjeravate interakciju putem API-ji. 

        Get-AzureRmSubscription

    b. Pokrenite sljedeću naredbu pružanje u SubscriptionId da biste konfigurirali pretplata će se koristiti.

        Select-AzureRmSubscription –SubscriptionId <subscriptionId>

5. Kopiranje teksta za skripte [Novo AzureRmServicePrincipalOwner.ps1](https://raw.githubusercontent.com/matt-gibbs/azbits/master/src/New-AzureRmServicePrincipalOwner.ps1) na lokalno računalo i spremite je kao cmdlet ljuske PowerShell (npr. `APIAuth.ps1`) i ona izvršenje `.\APIAuth.ps1`. 
    
6. Skripta će zatražiti ulazni za **principalName**. Unesite odgovarajuću naziv koji želite koristiti za stvaranje aplikacije servisa Active Directory (npr. APIAuth). 

7. Po završetku skripta će prikazati sljedeće četiri vrijednosti koje ćete morati programski uspješnoj AD pa provjerite da biste ih kopirali. 
        
    **TenantId**, **SubscriptionId**, **ApplicationId**i **tajna**.

    Koristite TenantId kao `{TENANT_ID}`, ApplicationId kao `{CLIENT_ID}` i tajnu kao `{CLIENT_SECRET}`.

    > [AZURE.NOTE] Zadani Pravilnik za sigurnost mogu blokirati izvodi skripte komponente PowerShell. Ako je tako, privremeno konfiguriranje izvođenja pravilo da biste skripte izvršavanje pomoću sljedeće naredbe:

        > Set-ExecutionPolicy RemoteSigned

8. Evo kako skup PS cmdleta izgledati. 

    ![][3]

9. Provjerite na portalu za upravljanje Azure novu aplikaciju AD stvorena s nazivom koje ste naveli u skriptu pod nazivom **principalName** u odjeljku **Pokaži aplikacije Moja tvrtka vlasništvu**.

    ![][4]

#### <a name="steps-to-get-a-valid-token"></a>Korake da biste dobili valjane tokena

1. Poziva API pomoću sljedećih parametara i provjerite je li da biste zamijenili klijentu\_ID, KLIJENT\_ID i KLIJENT\_TAJNA:

    - **Zahtjev za URL** kao *https://login.microsoftonline.com/ {klijentu\_ID} / oauth2/token*
    - **HTTP vrsta sadržaja zaglavlja** kao *aplikacija/x-www-form-urlencoded*
    - **HTTP zahtjev tijelo** kao *dodijeliti\_vrsta = klijent\_vjerodajnice i client_id = {KLIJENT\_ID} & client_secret = {KLIJENT\_TAJNA} & resource=https%3A%2F%2Fmanagement.core.windows.net%2F*

    Ovo je zahtjev za primjer:

        POST /{TENANT_ID}/oauth2/token HTTP/1.1
        Host: login.microsoftonline.com
        Content-Type: application/x-www-form-urlencoded
        grant_type=client_credentials&client_id={CLIENT_ID}&client_secret={CLIENT_SECRET}&reso
        urce=https%3A%2F%2Fmanagement.core.windows.net%2F

    Evo je odgovor na primjer:

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=utf-8
        Content-Length: 1234
    
        {"token_type":"Bearer","expires_in":"3599","expires_on":"1445395811","not_before":"144
        5391911","resource":"https://management.core.windows.net/","access_token":{ACCESS_TOKEN}}

    U ovom se primjeru dio URL-a kodiranje parametara objavu `resource` zapravo je vrijednost `https://management.core.windows.net/`. Pripazite da i URL kodiranje `{CLIENT_SECRET}` jer možda sadrži posebne znakove.

    > [AZURE.NOTE] Za testiranje, možete koristiti alat za klijent HTTP kao što su [Fiddler](http://www.telerik.com/fiddler) ili [proširenje Postman vizualnog okvira](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop) 

2. Sada u svakoj poziv API zaglavlju zahtjev za autorizaciju obuhvaćaju sljedeće:

        Authorization: Bearer {ACCESS_TOKEN}

    Ako se pojavi vraćena kod 401 stanja, provjerite tijelo odgovor je možda vam reći token je istekao. U tom slučaju dobiti novi token.

##<a name="using-the-apis"></a>Korištenje API-ji

Sad kad imate valjan tokena, spremni ste za pozive API-JA.

1. U svaki zahtjev za API morate proći valjana, koji tokena koje ste dobili u prethodnom odjeljku.

2. Morat ćete priključite neke parametara u zahtjev za URI koji služi za identifikaciju aplikacije. Zahtjev URI izgleda ovako

        https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/
        providers/Microsoft.MobileEngagement/appcollections/{app-collection}/apps/{app-resource-name}/

    Da biste dobili parametre, kliknite naziv aplikacije i kliknite nadzorna ploča, a će posjetite stranicu ovako s 3 parametra.

    - **1**`{subscription-id}`
    - **2**`{app-collection}`
    - **3**`{app-resource-name}`
    - **4** naziv grupe resursa za vaše će se **MobileEngagement** , osim ako ste stvorili novi. 

    ![Mobilni radnje API URI parametara][2]

>[AZURE.NOTE] <br/>
>1. Zanemari adresu korijenske API kao što je za prethodni API-ji.<br/>
>2. Ako ste stvorili aplikaciju pomoću portala za Azure klasični morate koristiti naziv aplikacije resursa koji se razlikuje od sam naziv aplikacije. Ako ste aplikaciju stvorili na portalu za Azure trebate koristiti naziv aplikacije sam (nema bez razlikovanja između aplikacija za resursa i naziv aplikacije za aplikacije stvorene na novom portalu).  

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication/azure-module.png
[2]: ./media/mobile-engagement-api-authentication/mobile-engagement-api-uri-params.png
[3]: ./media/mobile-engagement-api-authentication/ps-cmdlets.png
[4]: ./media/mobile-engagement-api-authentication/ad-app-creation.png



