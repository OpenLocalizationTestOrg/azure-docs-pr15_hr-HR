<properties
    pageTitle="Stvaranje applciations .NET HDInsight koji nije interaktivan provjere autentičnosti | Microsoft Azure"
    description="Saznajte kako da biste stvorili .NET HDInsight aplikacije koje nisu interaktivne provjere autentičnosti."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Stvaranje aplikacije .NET HDInsight koji nije interaktivan provjere autentičnosti

Možete izvršiti .NET Azure HDInsight aplikacije u odjeljku aplikacije vlastite identiteta (koji nije interaktivan) ili u odjeljku identitet korisnika prijavili u aplikaciju (interaktivne). Uzorak interaktivnih aplikacija potražite u članku [Slanje grozd/Svinja/Sqoop zadatke pomoću HDInsight .NET SDK](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). U ovom se članku objašnjava stvoriti aplikaciju za .NET koje nisu interaktivne provjere autentičnosti za povezivanje sa servisom Azure HDInsight i slanje grozd posla.

Iz aplikacije .NET, morate:

- Azure pretplate klijentu ID-a
- ID klijenta za Azure Directory aplikacije
- Azure Directory tajnu tipku za aplikacije.  

Glavni postupak obuhvaća sljedeće korake:

2. Stvorite aplikaciju Azure Directory.
2. Dodijelite uloge AD aplikaciju.
3. Razvoj klijentska aplikacija.


##<a name="prerequisites"></a>Preduvjeti

- Klaster HDInsight. Stvorili prema uputama u [uvodni vodič](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster). 




## <a name="create-azure-directory-application"></a>Stvaranje aplikacije za Azure direktorija 
Kada stvorite aplikaciju servisa Active Directory, zapravo stvara aplikacija i servisa glavni. Možete izvršiti aplikacije u odjeljku aplikacije identitet.

Trenutno, morate koristiti portala za Azure klasični da biste stvorili novu aplikaciju servisa Active Directory. Ova mogućnost dodat će se na portal Azure novije verzije. Možete izvesti i korake kroz Azure PowerShell ili Azure EŽA. Dodatne informacije o korištenju programa PowerShell ili EŽA sa servisom glavni potražite u članku [servis za provjeru autentičnosti glavni s Azure Voditelj resursa](../resource-group-authenticate-service-principal.md).

**Da biste stvorili Azure Directory aplikacije**

1.  Prijavite se na [portal za Azure klasični]( https://manage.windowsazure.com/).
2.  U lijevom oknu odaberite **Servisa Active Directory** .

    ![Azure klasični portala servisa active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Odaberite direktorija koji želite koristiti za stvaranje nove aplikacije. Ne moraju biti postojeće.
4.  Od početka do popisa postojeće aplikacije kliknite **aplikacije** .
5.  Kliknite **Dodaj** od dna da biste dodali nove aplikacije.
6.  Unesite **naziv**, odaberite **web-aplikacije i/ili Web API**i zatim kliknite **Dalje**.

    ![nove aplikacije servisa azure active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Unesite **URL za prijavu** i **Aplikacije ID URI**. Za **Prijavu na URL**, navedite URI za web-mjesto koji opisuje aplikacije. Ne provjerava postojanje parametra web-mjesta. Za Aplikacije ID URI pružaju URI koja služi za identifikaciju aplikacije. A zatim **Dovršeno**.
Potrebno stvoriti aplikaciju trenutak.  Nakon stvaranja aplikacija na portalu prikazuje stranici brzi Glace nove aplikacije. Nemojte zatvoriti portalu. 

    ![Nova svojstva aplikacije servisa azure active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**Da biste dobili klijentskog računala ID i tajnu ključ**

1.  Novostvoreni AD aplikacije stranici kliknite **Konfiguriraj** gornji izborniku.
2.  Napravite njezinu kopiju **ID klijenta**. Potrebno je u aplikaciji za .NET.
3.  U odjeljku **tipke**, kliknite padajući **Odaberite trajanje** pa odaberite **godina 1** ili **2 godina**. Vrijednost ključa će se prikazati sve dok ne spremite konfiguracije.
4.  Kliknite **Spremi** na dno stranice. Kada se pojavi tajnu ključ, napravite njezinu kopiju tipku. Potrebno je u aplikaciji za .NET.

##<a name="assign-ad-application-to-role"></a>Dodjeljivanje AD aplikacije uloga

Morate dodijeliti aplikacije u [ulogu](../active-directory/role-based-access-built-in-roles.md) da biste dodijelili dozvole za izvođenje akcija. Opseg možete postaviti na razini pretplate, grupa resursa ili resurs. Dozvole se naslijeđuju niže razine opsega (Ako, na primjer, dodavanje aplikacije ulozi čitač za grupu resursa znači da je pročitati grupa resursa i sve resursi sadrži). U ovom ćete praktičnom vodiču postavit opsega na razini grupu resursa.  Budući Azure klasični portal ne podržava grupe resursa, taj dio sadrži koje je potrebno izvršiti na portalu Azure. 

**Da biste dodali uloga vlasnika AD aplikacije**

1.  Prijavite se na [portal za Azure](https://portal.azure.com).
2.  U lijevom oknu kliknite **Grupu resursa** .
3.  Kliknite grupu resursa koji sadrži klaster HDInsight gdje će pokrenite upit grozd u nastavku ovog praktičnog vodiča. Ako ima previše grupa resursa, možete koristiti filtar.
4.  Kliknite **Access** iz plohu klaster.

    ![Ikona oblaka i thunderbolt = brzi početak rada](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  Kliknite **Dodavanje** iz plohu **korisnika** .
6.  Slijedite upute da biste dodali uloga **vlasnika** AD aplikaciju koju ste stvorili u zadnji postupak. Kada dovršite je uspješno, moraju potražite aplikaciju na popisu plohu korisnika s ulogom vlasnik.


##<a name="develop-hdinsight-client-application"></a>Razvoj HDInsight klijentska aplikacija

Stvaranje aplikacije konzole C# .net slijedeći upute u [Slanje Hadoop zadataka u HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Zatim zamijenite metodu GetTokenCloudCredentials sljedeće:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

Dohvaćanje ID klijenta putem komponente PowerShell:

    Get-AzureRmSubscription

Ili Azure EŽA:

    azure account show --json

      
## <a name="see-also"></a>Vidi također

- [Slanje Hadoop zadataka u HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Stvaranje aplikacije servisa Active Directory i glavnicu servis pomoću portala](../resource-group-create-service-principal-portal.md)
- [Autentičnost glavnicu servisa s Azure Voditelj resursa](../resource-group-authenticate-service-principal.md)
- [Kontrola Azure pristupa na temelju uloga](../active-directory/role-based-access-control-configure.md)
