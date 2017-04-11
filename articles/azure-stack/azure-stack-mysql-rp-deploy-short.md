<properties
    pageTitle="Korištenje baze podataka MySQL kao PaaS na hrpu Azure | Microsoft Azure"
    description="Razumijevanje brzi koraci implementacije davatelja resursa MySQL i navodite MySQL kao servisa na hrpu Azure."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>Korištenje baze podataka MySQL kao PaaS na hrpu Azure

> [AZURE.NOTE] Sljedeće informacije odnosi se samo na Azure stogu TP1 implementacije.

Možete implementirati davatelja MySQL resursa na hrpu Azure. Nakon implementacije davatelja resursa možete stvoriti MySQL poslužitelja i baze podataka putem predlošci implementaciju Voditelj resursa Azure i dati baze podataka MySQL kao servis. Baza podataka MySQL, koji su uobičajeni na web-mjestima, podržava mnoge platforme web-mjesta. Nakon implementacije davatelja resursa WordPress web-mjesta možete stvoriti iz platforme Azure web-aplikacijama kao dodatak za servis (PaaS) za Azure stogu.

## <a name="quick-steps-to-deploy-the-resource-provider"></a>Brzi koraci za implementaciju davatelja resursa
Ako ste već upoznati s stogu Azure, poduzmite sljedeće korake. Ako želite više detalja, slijedite veze na svakoj sekciji ili idite izravno na [uvođenja MySQL resursa davatelja prilagodnik za bazu podataka na PNA snop Azure](azure-stack-mysql-rp-deploy-long.md).

1.  Provjerite je li taj [dovršili sve korake za postavljanja](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy) prije implementacije kod davatelja resursa:

    - .NET 3.5 framework već postavili osnovni slike u sustavu Windows Server. (Ako ste preuzeli bitova Azure stogu nakon veljača, 23, 2016, možete preskočiti ovaj korak.)
    - [Izdanje Azure PowerShell koji je kompatibilan s Azure stoga je instaliran](http://aka.ms/azStackPsh).
    - U pregledniku Internet Explorer sigurnosnih postavki na ClientVM, [omogućena je isključen bolja sigurnost u pregledniku Internet Explorer i kolačiće](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies).

2. [Preuzimanje datoteke binarne datoteke za davatelja MySQL resursa](http://aka.ms/masmysqlrp) i izdvojiti na ClientVM u Azure stogu probni od pojam (PNA).

3. [Pokrenite bootstrap.cmd i skripte](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment).

    Skup skripte koji su grupirani prema dvije glavne kartice otvorit će se u na Očisti integrirani skriptiranje okruženje (filtar). Pokretanje učitati skripti u nizu slijeva nadesno na svakoj kartici.

    1. Pokreću skripte na kartici **Priprema** slijeva nadesno da biste:

        - Stvaranje zamjenskih certifikata radi zaštite komunikaciju između davatelja resursa i upravljanja resursima Azure.
        - Prihvatite uvjete licencnog ugovora MySQL i preuzmite binarne datoteke MySQL.
        - Prenesite potvrde i druge artefakte s računom za pohranu Azure stogu.
        - Objavite Galerija paketa tako da možete implementirati MySQL resursi putem galerije.

        > [AZURE.IMPORTANT] Ako bilo koji od skripte "smrzavanje" bez razloga vidljivu nakon izvršavanja vaš klijent Azure Active Directory, postavke sigurnosti možda blokira DLL koji je potrebno za pokretanje. Da biste riješili taj problem, potražite Microsoft.AzureStack.Deployment.Telemetry.Dll u mapi davatelja resursa, desnom tipkom miša kliknite, kliknite **Svojstva**i zatim na kartici **Općenito** potvrdite **Unblock** .

    2. Pokreću skripte na kartici **uvođenja** slijeva nadesno da biste:

        - [Uvođenje virtualnog računala (VM)](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) koji hostira vašeg davatelja resursa, MySQL poslužitelji i bazama podataka koje ćete stvoriti instancu. Ova skripta reference datoteka parametar JSON koje je potrebno ažurirati neke vrijednosti prije nego što pokrenete skriptu.
        - [Registrirati lokalni DNS zapis](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS) koji će mapiranje davatelja resursa VM.
        - [Registrirajte se vaš davatelj usluga resursa](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider) s lokalnom Azure upravitelj resursa.

        > [AZURE.IMPORTANT] Sve skripte pretpostavlja da slike Temeljni operacijski sustav ispunjava preduvjeti (.NET 3.5 instaliran, Omogući JavaScript i kolačiće u ClientVM i najnoviju verziju programa Azure PowerShell instaliran). Ako primite pogreške prilikom pokretanja skripte, ponovno da ispunjena preduvjete.

5. Da biste [testirali novi davatelj MySQL resursa](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment), implementirati bazom podataka MySQL s portala za Azure stogu. Kliknite **Stvori** &gt; **Prilagođena** &gt; **MySQL poslužitelj i bazu podataka**.

Trebali biste dobiti davatelja resursa MySQL prema gore i pokretanje u oko 25 minuta.
