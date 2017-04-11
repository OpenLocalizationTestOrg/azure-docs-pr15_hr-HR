<properties
   pageTitle="Stvaranje servisa glavni s Azure EŽA | Microsoft Azure"
   description="U članku se opisuje kako pomoću Azure EŽA stvaranje aplikacije servisa Active Directory i usluge i dopustiti pristup resursima pomoću kontrola pristupa na temelju uloga. Prikazuje kako provjeriti autentičnost aplikacijom lozinke ili certifikata."
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
   ms.date="09/30/2016"
   ms.author="tomfitz"/>

# <a name="use-azure-cli-to-create-a-service-principal-to-access-resources"></a>Stvaranje glavni servisa za pristup resursima pomoću EŽA Azure

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure EŽA](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Ako imate aplikaciju ili skriptu koja treba imati pristup, vjerojatno ne želite da biste pokrenuli postupak u odjeljku svojih vjerodajnica. Imate različite dozvole za aplikaciju, a ne želite da se aplikacije da biste nastavili pomoću vjerodajnica Ako promijenite vaše obaveze. Umjesto toga, stvorite identitet za aplikaciju koja sadrži vjerodajnice za provjeru autentičnosti i dodjele uloga. Prilikom svakog pokretanja aplikacije je sam potvrđuje s te vjerodajnice. U ovoj se temi objašnjava postavljanje programa za rad u vlastitom vjerodajnice i identitet pomoću [Azure EŽA za Mac, Linux i Windows](xplat-cli-install.md) .

S EŽA Azure, imate dvije mogućnosti za provjeru autentičnosti AD aplikacija:

 - lozinke
 - certifikat

U ovoj se temi objašnjava korištenje obje mogućnosti u EŽA Azure. Ako namjeravate li se prijaviti u Azure s programiranje framework (takve Python, Ruby ili Node.js), provjeru autentičnosti lozinke možda je najbolja mogućnost. Prije nego što odlučite hoćete li koristiti lozinke ili certifikata, u odjeljku [aplikacije ogledne](#sample-applications) primjere provjere autentičnosti u drugi okviri.

## <a name="active-directory-concepts"></a>Active Directory koncepti

U ovom se članku stvorite dva objekta – aplikacije Active Directory (AD) i glavni servisa. AD aplikacija je globalni predstavljanje aplikacije. Sadrži vjerodajnice (id aplikacije i ili lozinke ili certifikata). Glavni servis je lokalni prikaz aplikacija u servisu Active Directory. Sadrži Dodjela uloge. U ovoj se temi usredotočuje se na kojima je namijenjena aplikacije da biste pokrenuli u tvrtki ili ustanovi samo jedan aplikaciji jednog klijenta. Obično koristi aplikacije jednog klijenta za redak tvrtke programa koji se pokreću unutar tvrtke ili ustanove. U aplikaciji jednog klijenta, imate jedan AD aplikacije i jedan usluge.

Koje možda se pitate li se - Zašto mi je potreban oba objekti? Taj se način više smisla kada razmotrite više klijentske aplikacije. Obično koristi više klijentske aplikacije za softver kao-na-usluga aplikacije (SaaS), gdje aplikacija izvodi na mnogo različitih pretplate. Za više klijentske aplikacije, imate jedan AD aplikacije i više servisa upravitelji (jedan u svakom servisu Active Directory koja omogućuje pristup aplikaciji). Da biste postavili više klijentske aplikacije, potražite u članku [Vodič za programere za autorizaciju s resursima API Azure](resource-manager-api-authentication.md).

## <a name="required-permissions"></a>Potrebne dozvole

Da biste dovršili ove teme, morate imati potrebne dozvole u Azure Active Directory i Azure pretplatu. Konkretno, moraju imati mogućnost da biste stvorili aplikacije u servisu Active Directory i glavnicu servisa dodijeliti uloge. 

Servisa Active Directory, vaš račun mora biti administrator (kao što je **Globalni administrator** ili **Administrator korisnicima**). Ako vaš račun je dodijeljena uloga **korisnika** , morate koristiti administrator pretvoriti dozvole.

U vašoj pretplati se vaš račun mora imati `Microsoft.Authorization/*/Write` programa access koja je dodijeljena uloga [vlasnika](./active-directory/role-based-access-built-in-roles.md#owner) ili [Korisnički pristup administratorsku](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) ulogu. Ako je vaš račun dodijeljena uloga **suradnika** , primite poruku o pogrešci prilikom pokušaja glavnicu servisa dodijeliti uloge. Ponovno pretplate administrator mora vam dodijeliti dovoljna prava pristupa.

Sada, prijeđite na odjeljak za provjeru autentičnosti [lozinke](#create-service-principal-with-password) ili [certifikata](#create-service-principal-with-certificate) .

## <a name="create-service-principal-with-password"></a>Stvaranje servisa glavni pomoću lozinke

U ovom se odjeljku poduzeti korake za stvaranje aplikacije servisa Active Directory pomoću lozinke i dodijeliti ulogu čitač glavni servisa.

Pogledajmo korake u nastavku.

1. Prijavite se na račun.

        azure login

1. Imate dvije mogućnosti za stvaranje aplikacije servisa Active Directory. Možete stvoriti glavni u jednom koraku AD aplikacija i servisa sustava ili ih stvorite zasebno. Stvorite ih u jednom koraku ako ne trebate navedite Početna stranica i identifikator ji za aplikacije. Stvorite ih zasebno ako vam je potrebna da biste postavili te vrijednosti za web-aplikacije. U ovom ćete koraku prikazane su obje mogućnosti.

     - Da biste stvorili AD aplikacija i servisa glavni u jednom koraku, navedite naziv aplikacije i lozinkom, kao što je prikazano u sljedeću naredbu:
     
            azure ad sp create -n exampleapp -p {your-password}     
     
     - Da biste stvorili aplikacije AD zasebno, navedite naziv aplikacije, Početna stranica URI, identifikator ji i lozinkom, kao što je prikazano u sljedeću naredbu:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example -p <Your_Password>

         Prethodni naredba vraća vrijednost ID programa. Da biste stvorili glavni usluga, unesite vrijednost kao parametar u sljedeću naredbu:
     
            azure ad sp create -a <AppId>
     
     Ako vaš račun nema [potrebne dozvole](#required-permissions) na servisu Active Directory, pogledajte poruku o pogrešci u kojoj stoji da se "Authentication_Unauthorized" ili "nije pronađena pretplata u kontekstu".
    
     Za obje mogućnosti, vraća se novi Upravitelj servisa. **Id objekta** nije potrebna kada se dodjelu dozvola. Prilikom prijave u nije potrebno guid navedena **Imena usluge** . Guid je iste vrijednosti kao id aplikacije. U aplikacijama uzorka tu vrijednost je se nazivaju **ID klijenta**. 
    
        info:    Executing command ad sp create
        + Creating application exampleapp
        / Creating service principal for application 7132aca4-1bdb-4238-ad81-996ff91d8db+
        data:    Object Id:               ff863613-e5e2-4a6b-af07-fff6f2de3f4e
        data:    Display Name:            exampleapp
        data:    Service Principal Names:
        data:                             7132aca4-1bdb-4238-ad81-996ff91d8db4
        data:                             https://www.contoso.org/example
        info:    ad sp create command OK

2. Dodijelite dozvole glavni servisa pretplate na. U ovom primjeru dodajte glavnicu servisa ulozi **čitač** koji daje dozvolu za čitanje svih resursa u pretplate. Druge uloge potražite u članku [RBAC: ugrađene uloge](./active-directory/role-based-access-built-in-roles.md). Za parametar **ServicePrincipalName** navedite **ID objekta** koji ste koristili prilikom stvaranja aplikacije. 

        azure role assignment create --objectId ff863613-e5e2-4a6b-af07-fff6f2de3f4e -o Reader -c /subscriptions/{subscriptionId}/

     Ako je vaš račun nema potrebne dozvole za dodjeljivanje uloge, vidjet ćete poruku o pogrešci. Poruka označuje vaš račun **nema autorizacije za izvođenje akcija Microsoft.Authorization/roleAssignments/write putem opseg "/ pretplate / {guid}"**. 

To je to! AD aplikacija i servisa glavnicu postavljaju. U sljedećem odjeljku objašnjava da biste se prijavili pomoću vjerodajnica kroz Azure EŽA. Ako želite koristiti vjerodajnicu u aplikaciji kod nije potrebno da biste nastavili s ove teme. Možete se prebaciti na [aplikacijama ogledne](#sample-applications) primjere prijave pomoću aplikacije id i lozinku. 

### <a name="provide-credentials-through-azure-cli"></a>Unesite vjerodajnice pomoću EŽA Azure

Sada ćete morati prijaviti kao program za izvođenje operacija.

1. Kad god se prijaviti kao glavni usluga, morate unesite id klijenta direktorija za AD aplikacije. Klijent je instanca servisa Active Directory. Da biste dohvatili id klijenta za pretplatu trenutno čija je autentičnost provjerena, koristite:

        azure account show

     Koja vraća:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Ako vam je potrebna da biste dobili id klijenta drugu pretplatu, koristite sljedeću naredbu:

        azure account show -s {subscription-id}

2. Ako je potrebno dohvatiti id klijenta za zapisivanje u koristite:

        azure ad sp show -c exampleapp --json

     Vrijednosti koju želite koristiti za zapisivanje u je guid naveden u glavni nazivi servisa.

        [
          {
            "objectId": "ff863613-e5e2-4a6b-af07-fff6f2de3f4e",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "7132aca4-1bdb-4238-ad81-996ff91d8db4",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "7132aca4-1bdb-4238-ad81-996ff91d8db4"
            ]
          }
        ]

3. Prijavite se kao Upravitelj servisa.

        azure login -u 7132aca4-1bdb-4238-ad81-996ff91d8db4 --service-principal --tenant {tenant-id}

    Zatraži lozinku. Unesite lozinku koju ste naveli prilikom stvaranja aplikacije za AD.

        info:    Executing command login
        Password: ********

Sada provjereno kao glavnicu servisa glavnicu servis koji ste stvorili.

## <a name="create-service-principal-with-certificate"></a>Stvaranje servisa glavni certifikatom

U ovom odjeljku, provedite korake da biste:

- stvaranje samopotpisane potvrde
- Stvaranje aplikacije AD pomoću certifikata i glavnicu servisa
- Dodijeli ulogu čitač glavnicu servisa

Da biste dovršili ove korake, morate imati instaliran [OpenSSL](http://www.openssl.org/) .

1. Stvaranje samopotpisane potvrde.

        openssl req -x509 -days 3650 -newkey rsa:2048 -out cert.pem -nodes -subj '/CN=exampleapp'

2. Kombiniranje javni i privatni ključevi.

        cat privkey.pem cert.pem > examplecert.pem

3. Otvorite datoteku **examplecert.pem** i potražite dugo niz znakova između **---Početak certifikat---** i **---ZAVRŠETKA certifikat---**. Kopirajte podatke o certifikatu. Prenesite ove podatke kao parametar prilikom stvaranja servis glavni.

1. Prijavite se na račun.

        azure login

1. Imate dvije mogućnosti za stvaranje aplikacije servisa Active Directory. Možete stvoriti glavni u jednom koraku AD aplikacija i servisa sustava ili ih stvorite zasebno. Stvorite ih u jednom koraku ako ne trebate navedite Početna stranica i identifikator ji za aplikacije. Stvorite ih zasebno ako vam je potrebna da biste postavili te vrijednosti za web-aplikacije. U ovom ćete koraku prikazane su obje mogućnosti.

     - Da biste stvorili AD aplikacija i servisa glavni u jednom koraku, navedite naziv aplikacije i podatke o certifikatu kao što je prikazano u sljedeću naredbu:
     
            azure ad sp create -n exampleapp --cert-value <certificate data>
     
     - Da biste stvorili aplikacije AD zasebno, navedite naziv aplikacije, Početna stranica URI, identifikator ji i podatke o certifikatu kao što je prikazano u sljedeću naredbu:
     
            azure ad app create -n exampleapp --home-page http://www.contoso.org --identifier-uris https://www.contoso.org/example --cert-value <certificate data>

         Prethodni naredba vraća vrijednost ID programa. Da biste stvorili glavni usluga, unesite vrijednost kao parametar u sljedeću naredbu:
     
            azure ad sp create -a <AppId>
  
     Ako vaš račun nema [potrebne dozvole](#required-permissions) na servisu Active Directory, pogledajte poruku o pogrešci u kojoj stoji da se "Authentication_Unauthorized" ili "nije pronađena pretplata u kontekstu".
    
     Za obje mogućnosti, vraća se novi Upravitelj servisa. Kada dodjelu dozvola, potrebno je identifikator objekta. Prilikom prijave u nije potrebno guid navedena **Imena usluge** . Guid je iste vrijednosti kao id aplikacije. U aplikacijama uzorka tu vrijednost je se nazivaju **ID klijenta**. 
    
        info:    Executing command ad sp create
        - Creating service principal for application 4fd39843-c338-417d-b549-a545f584a74+
        data:    Object Id:        7dbc8265-51ed-4038-8e13-31948c7f4ce7
        data:    Display Name:     exampleapp
        data:    Service Principal Names:
        data:                      4fd39843-c338-417d-b549-a545f584a745
        data:                      https://www.contoso.org/example
        info:    ad sp create command OK
        
2. Dodijelite dozvole glavni servisa pretplate na. U ovom primjeru dodajte glavnicu servisa ulozi **čitač** koji daje dozvolu za čitanje svih resursa u pretplate. Druge uloge potražite u članku [RBAC: ugrađene uloge](./active-directory/role-based-access-built-in-roles.md). Za parametar **ServicePrincipalName** navedite **ID objekta** koji ste koristili prilikom stvaranja aplikacije. 

        azure role assignment create --objectId 7dbc8265-51ed-4038-8e13-31948c7f4ce7 -o Reader -c /subscriptions/{subscriptionId}/

     Ako je vaš račun nema potrebne dozvole za dodjeljivanje uloge, vidjet ćete poruku o pogrešci. Poruka označuje vaš račun **nema autorizacije za izvođenje akcija Microsoft.Authorization/roleAssignments/write putem opseg "/ pretplate / {guid}"**. 

### <a name="provide-certificate-through-automated-azure-cli-script"></a>Navedite certifikata pomoću automatiziranog Azure EŽA skripte

Sada ćete morati prijaviti kao program za izvođenje operacija.

1. Kad god se prijaviti kao glavni usluga, morate unesite id klijenta direktorija za AD aplikacije. Klijent je instanca servisa Active Directory. Da biste dohvatili id klijenta za pretplatu trenutno čija je autentičnost provjerena, koristite:

        azure account show

     Koja vraća:

        info:    Executing command account show
        data:    Name                        : Windows Azure MSDN - Visual Studio Ultimate
        data:    ID                          : {guid}
        data:    State                       : Enabled
        data:    Tenant ID                   : {guid}
        data:    Is Default                  : true
        ...

     Ako vam je potrebna da biste dobili id klijenta drugu pretplatu, koristite sljedeću naredbu:

        azure account show -s {subscription-id}

1. Da biste dohvatiti otisak prsta certifikat i uklonite nepotrebne znakove, koristite:

        openssl x509 -in "C:\certificates\examplecert.pem" -fingerprint -noout | sed 's/SHA1 Fingerprint=//g'  | sed 's/://g'
    
     Koja vraća otisak prsta vrijednost slično:

        30996D9CE48A0B6E0CD49DBB9A48059BF9355851

2. Ako je potrebno dohvatiti id klijenta za zapisivanje u koristite:

        azure ad sp show -c exampleapp

     Vrijednosti koju želite koristiti za zapisivanje u je guid naveden u glavni nazivi servisa.

        [
          {
            "objectId": "7dbc8265-51ed-4038-8e13-31948c7f4ce7",
            "objectType": "ServicePrincipal",
            "displayName": "exampleapp",
            "appId": "4fd39843-c338-417d-b549-a545f584a745",
            "servicePrincipalNames": [
              "https://www.contoso.org/example",
              "4fd39843-c338-417d-b549-a545f584a745"
            ]
          }
        ]

1. Prijavite se kao Upravitelj servisa.

        azure login --service-principal --tenant {tenant-id} -u 4fd39843-c338-417d-b549-a545f584a745 --certificate-file C:\certificates\examplecert.pem --thumbprint {thumbprint}

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
- Dodatne informacije o korištenju potvrde i Azure EŽA, pročitajte članak [provjere autentičnosti utemeljene na certifikat s upravitelji servisa Azure iz naredbenog retka Linux](http://blogs.msdn.com/b/arsen/archive/2015/09/18/certificate-based-auth-with-azure-service-principals-from-linux-command-line.aspx). 
