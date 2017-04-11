<properties
   pageTitle="Stvaranje servisa Azure glavni sa servisom PowerShell | Microsoft Azure"
   description="U članku se opisuje kako pomoću ljuske PowerShell za Azure stvaranje aplikacije servisa Active Directory i usluge i dopustiti pristup resursima pomoću kontrola pristupa na temelju uloga. Prikazuje kako provjeriti autentičnost aplikacijom lozinke ili certifikata."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/12/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Stvaranje glavni servisa za pristup resursima pomoću Azure PowerShell

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure EŽA](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)

Ako imate aplikaciju ili skriptu koja treba imati pristup, vjerojatno ne želite da biste pokrenuli postupak u odjeljku svojih vjerodajnica. Imate različite dozvole za aplikaciju, a ne želite da se aplikacije da biste nastavili pomoću vjerodajnica Ako promijenite vaše obaveze. Umjesto toga, stvorite identitet za aplikaciju koja sadrži vjerodajnice za provjeru autentičnosti i dodjele uloga. Prilikom svakog pokretanja aplikacije je sam potvrđuje s te vjerodajnice. U ovoj se temi objašnjava korištenje [Azure PowerShell](powershell-install-configure.md) da biste postavili sve što vam treba za aplikaciju za rad u vlastitom vjerodajnice i identitet.

Sa servisom PowerShell, imate dvije mogućnosti za provjeru autentičnosti AD aplikacija:

 - lozinke
 - certifikat

U ovoj se temi objašnjava korištenje obje mogućnosti u ljusci PowerShell. Ako namjeravate li se prijaviti u Azure s programiranje framework (takve Python, Ruby ili Node.js), provjeru autentičnosti lozinke možda je najbolja mogućnost. Prije nego što odlučite hoćete li koristiti lozinke ili certifikata, u odjeljku [aplikacije ogledne](#sample-applications) primjere provjere autentičnosti u drugi okviri.

## <a name="active-directory-concepts"></a>Active Directory koncepti

U ovom se članku stvorite dva objekta – aplikacije Active Directory (AD) i glavni servisa. AD aplikacija je globalni predstavljanje aplikacije. Sadrži vjerodajnice (id aplikacije i ili lozinke ili certifikata). Glavni servis je lokalni prikaz aplikacija u servisu Active Directory. Sadrži Dodjela uloge. U ovoj se temi usredotočuje se na kojima je namijenjena aplikacije da biste pokrenuli u tvrtki ili ustanovi samo jedan aplikaciji jednog klijenta. Obično koristi aplikacije jednog klijenta za redak tvrtke programa koji se pokreću unutar tvrtke ili ustanove. U aplikaciji jednog klijenta, imate jedan AD aplikacije i jedan usluge.

Koje možda se pitate li se - Zašto mi je potreban oba objekti? Taj se način više smisla kada razmotrite više klijentske aplikacije. Obično koristi više klijentske aplikacije za softver kao-na-usluga aplikacije (SaaS), gdje aplikacija izvodi na mnogo različitih pretplate. Za više klijentske aplikacije, imate jedan AD aplikacije i više servisa upravitelji (jedan u svakom servisu Active Directory koja omogućuje pristup aplikaciji). Da biste postavili više klijentske aplikacije, potražite u članku [Vodič za programere za autorizaciju s resursima API Azure](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Potrebne dozvole

Da biste dovršili ove teme, morate imati potrebne dozvole u Azure Active Directory i Azure pretplatu. Konkretno, moraju imati mogućnost da biste stvorili aplikacije u servisu Active Directory i glavnicu servisa dodijeliti uloge. 

Servisa Active Directory, vaš račun mora biti administrator (kao što je **Globalni administrator** ili **Administrator korisnicima**). Ako vaš račun je dodijeljena uloga **korisnika** , morate koristiti administrator pretvoriti dozvole.

U vašoj pretplati se vaš račun mora imati `Microsoft.Authorization/*/Write` programa access koja je dodijeljena uloga [vlasnika](./active-directory/role-based-access-built-in-roles.md#owner) ili [Korisnički pristup administratorsku](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) ulogu. Ako je vaš račun dodijeljena uloga **suradnika** , primite poruku o pogrešci prilikom pokušaja glavnicu servisa dodijeliti uloge. Ponovno pretplate administrator mora vam dodijeliti dovoljna prava pristupa.

Sada, prijeđite na odjeljak za provjeru autentičnosti [lozinke](#create-service-principal-with-password) ili [certifikata](#create-service-principal-with-certificate) .

## <a name="create-service-principal-with-password"></a>Stvaranje servisa glavni pomoću lozinke

U ovom odjeljku, provedite korake da biste:

- Stvaranje aplikacije servisa Active Directory pomoću lozinke
- Stvaranje glavnicu servisa
- Dodijeli ulogu čitač glavnicu servisa

Da biste brzo izveli taj postupak, pogledajte sljedeće tri Cmdlete. 

     $app = New-AzureRmADApplication -DisplayName "{app-name}" -HomePage "https://{your-domain}/{app-name}" -IdentifierUris "https://{your-domain}/{app-name}" -Password "{your-password}"
     New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
     New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Pogledajmo korake pobliže da biste provjerili postupka.

1. Prijavite se na račun.

        Add-AzureRmAccount

1. Stvaranje nove aplikacije servisa Active Directory unosom zaslonsko ime, URI koji opisuje aplikacije, ji koje označavaju aplikacije i lozinku za identitet aplikacije.

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org/exampleapp" -IdentifierUris "https://www.contoso.org/exampleapp" -Password "<Your_Password>"

     Za aplikacije jednog klijenta na ji su nije provjeriti valjanost.
     
     Ako vaš račun nema [potrebne dozvole](#required-permissions) na servisu Active Directory, pogledajte poruku o pogrešci u kojoj stoji da se "Authentication_Unauthorized" ili "nije pronađena pretplata u kontekstu".

1. Pregledajte novi objekt aplikacije. 

        $app
        
     Imajte na umu posebno svojstvo **ApplicationId** , što je potrebno za stvaranje servisa upravitelji, dodjele uloga i Pribavljanje token za pristup.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}

2. Stvorite glavni usluge za svoju aplikaciju tako da Prenos u id aplikacije aplikacije servisa Active Directory.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

3. Dodijelite dozvole glavni servisa pretplate na. U ovom primjeru dodajte glavnicu servisa ulozi **čitač** koji daje dozvolu za čitanje svih resursa u pretplate. Druge uloge potražite u članku [RBAC: ugrađene uloge](./active-directory/role-based-access-built-in-roles.md). Za parametar **ServicePrincipalName** pružaju **ApplicationId** koje ste koristili prilikom stvaranja aplikacije. 

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Ako je vaš račun nema potrebne dozvole za dodjeljivanje uloge, vidjet ćete poruku o pogrešci. Poruka označuje vaš račun **nema autorizacije za izvođenje akcija Microsoft.Authorization/roleAssignments/write putem opseg "/ pretplate / {guid}"**. 

To je to! AD aplikacija i servisa glavnicu postavljaju. U sljedećem odjeljku pokazuje kako se prijaviti pomoću vjerodajnica putem komponente PowerShell. Ako želite koristiti vjerodajnicu u aplikaciji kod, možete se prebaciti na [probne aplikacije](#sample-applications). 

### <a name="provide-credentials-through-powershell"></a>Unesite vjerodajnice putem komponente PowerShell

Sada ćete morati prijaviti kao program za izvođenje operacija.

1. Stvaranje objekta **PSCredential** koja sadrži vaše vjerodajnice tako da pokrenete naredbu **Dohvati vjerodajnica** . Potreban vam je **ApplicationId** prije pokretanja sljedeću naredbu pa provjerite imate li koji je dostupan za lijepljenje.

        $creds = Get-Credential

2. Se od vas zatraži da unesete vjerodajnice. Koristite **ApplicationId** koje ste koristili prilikom stvaranja aplikacije za korisničko ime. Da biste postigli lozinku, koristite onaj koji ste naveli prilikom stvaranja računa.

     ![Unesite vjerodajnice](./media/resource-group-authenticate-service-principal/arm-get-credential.png)

2. Kad god se prijaviti kao glavni usluga, morate unesite id klijenta direktorija za AD aplikacije. Klijent je instanca servisa Active Directory. Ako imate samo jedan pretplatu, možete koristiti:

        $tenant = (Get-AzureRmSubscription).TenantId
    
     Ako imate više pretplata, navedite pretplate na kojoj se nalazi servisa Active Directory. Dodatne informacije potražite u članku [kako Azure pretplate pridružuju Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

        $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

4. Prijavite se kao glavni servis navođenjem da je taj račun servisa glavni i unošenjem vjerodajnica objekt. 

        Add-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId $tenant
        
     Sada provjereno kao glavni servisa Active Directory aplikacije koje ste stvorili.

### <a name="save-access-token-to-simplify-log-in"></a>Spremanje token za pristup da biste pojednostavnili prijava

Da biste izbjegli pruža servis glavni vjerodajnice svaki put kada je potrebno da biste se prijavili, uštedjet ćete token za pristup.

1. Da biste koristili token za trenutni pristup u sesiji kasnije, spremite profil.

        Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
     Otvorite profil i pregledali njegov sadržaj. Obratite pozornost na to da sadrži token za pristup. 
        
2. Umjesto da ručno ponovo prijaviti, jednostavno učitavanje profila.

        Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
        
> [AZURE.NOTE] Pristupni token istekne, tako da se pomoću spremljenu profila radi samo dok god je valjan token.
        
## <a name="create-service-principal-with-certificate"></a>Stvaranje servisa glavni certifikatom

U ovom odjeljku, provedite korake da biste:

- stvaranje samopotpisane potvrde
- Stvaranje aplikacije servisa Active Directory pomoću certifikata
- Stvaranje glavnicu servisa
- Dodijeli ulogu čitač glavnicu servisa

Da biste brzo izveli taj postupak s Azure PowerShell 2.0 na Windows 10 ili Windows Server 2016 Tehnički pretpregled, pogledajte sljedeće Cmdlete. 

    $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
    $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())
    $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

Pogledajmo korake pobliže da biste provjerili postupka. U ovom se članku prikazuje i kako obaviti zadatke u starijim verzijama Azure PowerShell ili operacijske sustave.

### <a name="create-the-self-signed-certificate"></a>Stvaranje samopotpisane potvrde

Verzija sustava PowerShell dostupnih u sustavima Windows 10 i Windows Server 2016 Tehnički pretpregled ima ažurirane cmdleta **New SelfSignedCertificate** za stvaranje samopotpisane potvrde. Starijim operacijskim sustavima imati cmdleta New-SelfSignedCertificate, ali ne nudi parametre potrebne za ovu temu. Umjesto toga, morate uvesti modula za generiranje certifikata. Ova tema prikazuje oba pristupa za generiranje certifikat koji se temelji na operacijskog sustava imate. 

- Ako imate **Windows 10 ili Windows Server 2016 Tehnički pretpregled**, pokrenite sljedeću naredbu za stvaranje samopotpisane potvrde: 

        $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleapp" -KeySpec KeyExchange
       
- Ako **nemate Windows 10 ili Windows Server 2016 Tehnički pretpregled**, morate preuzeti [samopotpisane potvrde generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) Microsoft Script Center. Izdvajanje njegov sadržaj i uvezite cmdlet vam je potrebna.
     
        # Only run if you could not use New-SelfSignedCertificate
        Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
    
     Nakon toga stvaranje potvrde.
    
        $cert = New-SelfSignedCertificateEx -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"

Imate certifikat i možete nastaviti sa stvaranjem aplikacije servisa Active Directory.

### <a name="create-the-active-directory-app-and-service-principal"></a>Stvorite glavni aplikacije servisa Active Directory i usluga

1. Dohvaćanje vrijednost ključa iz certifikata.

        $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

2. Prijavite se na račun za Azure.

        Add-AzureRmAccount

3. Stvaranje nove aplikacije servisa Active Directory unosom zaslonsko ime, URI koji opisuje aplikacije, ji koje označavaju aplikacije i lozinku za identitet aplikacije.

     Ako imate Azure PowerShell 2.0 (kolovoz 2016 ili noviji), koristite sljedeći cmdlet:

        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Ako imate Azure PowerShell 1.0, koristite sljedeći cmdlet:
    
        $app = New-AzureRmADApplication -DisplayName "exampleapp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.contoso.org/example" -KeyValue $keyValue -KeyType AsymmetricX509Cert  -EndDate $cert.NotAfter -StartDate $cert.NotBefore      
    
    Za aplikacije jednog klijenta na ji su nije provjeriti valjanost.
    
    Ako vaš račun nema [potrebne dozvole](#required-permissions) na servisu Active Directory, pogledajte poruku o pogrešci u kojoj stoji da se "Authentication_Unauthorized" ili "nije pronađena pretplata u kontekstu".
        
    Pregledajte novi objekt aplikacije. 

        $app

    Obratite pozornost na to svojstvo **ApplicationId** , što je potrebno za stvaranje servisa upravitelji, dodjele uloga i Pribavljanje tokeni programa access.

        DisplayName             : exampleapp
        ObjectId                : c95e67a3-403c-40ac-9377-115fa48f8f39
        IdentifierUris          : {https://www.contoso.org/example}
        HomePage                : https://www.contoso.org
        Type                    : Application
        ApplicationId           : 8bc80782-a916-47c8-a47e-4d76ed755275
        AvailableToOtherTenants : False
        AppPermissions          : 
        ReplyUrls               : {}


5. Stvorite glavni usluge za svoju aplikaciju tako da Prenos u id aplikacije aplikacije servisa Active Directory.

        New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId

6. Dodijelite dozvole glavni servisa pretplate na. U ovom primjeru dodajte glavnicu servisa ulozi **čitač** koji daje dozvolu za čitanje svih resursa u pretplate. Druge uloge potražite u članku [RBAC: ugrađene uloge](./active-directory/role-based-access-built-in-roles.md). Za parametar **ServicePrincipalName** pružaju **ApplicationId** koje ste koristili prilikom stvaranja aplikacije.

        New-AzureRmRoleAssignment -RoleDefinitionName Reader -ServicePrincipalName $app.ApplicationId.Guid

    Ako je vaš račun nema potrebne dozvole za dodjeljivanje uloge, vidjet ćete poruku o pogrešci. Poruka označuje vaš račun **nema autorizacije za izvođenje akcija Microsoft.Authorization/roleAssignments/write putem opseg "/ pretplate / {guid}"**.

To je to! AD aplikacija i servisa glavnicu postavljaju. U sljedećem odjeljku objašnjava da biste se prijavili pomoću certifikata putem komponente PowerShell.

### <a name="provide-certificate-through-automated-powershell-script"></a>Navedite certifikata pomoću automatiziranog skriptu komponente PowerShell

Kad god se prijaviti kao glavni usluga, morate unesite id klijenta direktorija za AD aplikacije. Klijent je instanca servisa Active Directory. Ako imate samo jedan pretplatu, možete koristiti:

    $tenant = (Get-AzureRmSubscription).TenantId
    
Ako imate više pretplata, navedite pretplate na kojoj se nalazi servisa Active Directory. Dodatne informacije potražite u članku [administriranje Azure AD direktorija](./active-directory/active-directory-administer.md).

    $tenant = (Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId

Za provjeru autentičnosti u skriptu, navedite račun je servis glavnog i navedite otisak prsta certifikata, id aplikacije i id klijenta. Da biste automatizirali skriptu, možete pohraniti ove vrijednosti kao varijable okruženja te njihovo dohvaćanje tijekom izvođenja ili ih možete obuhvatiti skriptu.

    Add-AzureRmAccount -ServicePrincipal -CertificateThumbprint $cert.Thumbprint -ApplicationId $app.ApplicationId -TenantId $tenant

Sada provjereno kao glavni servisa Active Directory aplikacije koje ste stvorili.

## <a name="sample-applications"></a>Probne aplikacije

Sljedeće ogledne aplikacije pokazalo kako se prijaviti kao Upravitelj servisa.

**.NET**

- [Implementacija programa SSH omogućeno VM s predloškom s .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [Upravljanje Azure resursa i grupa resursa s .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [Početak rada s resursima - implementacija pomoću predloška Azure resursima - u Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [Početak rada s resursima - upravljanje grupa resursa – u Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [Implementacija programa SSH omogućeno VM s predloškom u Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [Upravljanje Azure resursa i grupa resursa s Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [Implementacija programa SSH omogućeno VM s predloškom u Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [Upravljanje Azure resursa i grupa resursa s Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**Ruby**

- [Implementacija programa SSH omogućeno VM s predloškom u Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [Upravljanje Azure resursa i grupa resursa s Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Daljnji koraci
  
- Detaljne upute Integracija aplikacije u Azure upravljanja resursima potražite u članku [Vodič za programere za autorizaciju s resursima API Azure](resource-manager-api-authentication.md).
- Detaljnije objašnjenje aplikacija i servisa upravitelji potražite u članku [aplikacija i servisa glavnicu objekte](./active-directory/active-directory-application-objects.md). 
- Dodatne informacije o provjeri autentičnosti servisa Active Directory potražite u članku [Provjera autentičnosti scenariji za Azure AD](./active-directory/active-directory-authentication-scenarios.md).



