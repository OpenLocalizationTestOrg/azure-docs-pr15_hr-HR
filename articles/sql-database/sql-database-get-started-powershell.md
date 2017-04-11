<properties
    pageTitle="Nove baze podataka SQL postavljanje sa servisom PowerShell | Microsoft Azure"
    description="Saznajte kako stvoriti bazu podataka za SQL sa servisom PowerShell. Uobičajeni zadaci postavljanja baze podataka upravlja se putem cmdleta ljuske PowerShell."
    keywords="Stvaranje nove sql baze podataka, postavljanje baze podataka"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="08/19/2016"
    ms.author="sstein"/>

# <a name="create-a-sql-database-and-perform-common-database-setup-tasks-with-powershell-cmdlets"></a>Stvaranje baze podataka SQL i izvoditi uobičajene zadatke Postavljanje baze podataka pomoću cmdleta ljuske PowerShell


> [AZURE.SELECTOR]
- [Portal za Azure](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Saznajte kako stvoriti SQL baze podataka pomoću cmdleta ljuske PowerShell. (Stvaranje elastic baza podataka, potražite u članku [Stvaranje nove grupe aplikacija elastic baze podataka sa servisom PowerShell](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## <a name="database-setup-create-a-resource-group-server-and-firewall-rule"></a>Postavljanje baze podataka: Stvaranje grupa resursa, poslužitelja i vatrozida pravila

Kada imate pristup pokrećete cmdleta za odabrani Azure pretplatu, na sljedeći korak je uspostavljanje resursa grupu koja sadrži poslužitelja na kojem će se stvoriti bazu podataka. Možete uređivati sljedeću naredbu da biste koristili lokaciju valjani koje odaberete. Pokrenite **(Get-AzureRmLocation | Gdje objekt {$_. Davatelji - eq "Microsoft.Sql"}). Mjesto** dobit ćete popis valjana mjesta.

Pokrenite sljedeću naredbu da biste stvorili grupu resursa:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### <a name="create-a-server"></a>Stvaranje poslužitelja

Baze podataka SQL stvaraju se unutar poslužitelja baze podataka SQL Azure. Pokretanje sustava [New-AzureRmSqlServer] (https://msdn.microsoft.com/library/azure/mt603715(v=azure.300\).aspx) da biste stvorili na poslužitelju. Naziv poslužitelja mora biti jedinstvena sa svim poslužiteljima baze podataka SQL Azure. Ako naziv poslužitelja već preuzeli, prikazat će se pogreška. Vrijednosti i sudjelovanje je ta naredba može potrajati nekoliko minuta. Naredba koristiti bilo koji valjani mjesto odaberete možete uređivati, ali koristite na isto mjesto na koje ste koristili za grupu resursa stvorili u prethodnom koraku.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Kada pokrenete tu naredbu, dobit ćete upit za korisničko ime i lozinku. Ne unesite vjerodajnice za Azure. Umjesto toga, unesite korisničko ime i lozinku da biste stvorili kao administrator poslužitelja. Skripta pri dnu ovog članka pokazuje kako postaviti vjerodajnice za poslužitelj u kod.

Detalji o poslužitelju pojavljuje poslužitelj uspješno stvorili.

### <a name="configure-a-server-firewall-rule-to-allow-access-to-the-server"></a>Konfiguriranje pravila vatrozida server da biste omogućili pristup poslužitelju

Da biste pristupili poslužitelj, morate uspostaviti pravila vatrozida. Pokretanje sustava [New-AzureRmSqlServerFirewallRule] (https://msdn.microsoft.com/library/azure/mt603860(v=azure.300\).aspx) naredba, IP adrese početka i završetka zamjenjuje valjane vrijednosti za vaše računalo.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

Detalji pravila vatrozida prikazuju se nakon što je pravilo stvoreno uspješno.

Da biste omogućili drugih Azure servisa za pristup poslužitelju, dodajte pravilo vatrozid i postavite StartIpAddress i EndIpAddress 0.0.0.0. Tim se pravilom omogućuje Azure promet s bilo kojeg Azure pretplate za pristup poslužitelju.

Dodatne informacije potražite u članku [Vatrozid za baze podataka SQL Azure](sql-database-firewall-configure.md).


## <a name="create-a-sql-database"></a>Stvaranje baze podataka SQL

Sada imate grupu resursa, na poslužitelju i pravila vatrozida konfigurirano tako da biste mogli pristupati s poslužitelja.

Sljedeće [novo AzureRmSqlDatabase] (https://msdn.microsoft.com/library/azure/mt619339(v=azure.300\).aspx) naredba stvara (prazno) SQL baze podataka u sloju standardnog servisa, s performansama razinu S1:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


Detalji baze podataka prikazuju se nakon uspješnog stvaranja baze podataka.

## <a name="create-a-sql-database-powershell-script"></a>Stvaranje baze podataka SQL skriptu komponente PowerShell

Sljedeću skriptu komponente PowerShell stvara SQL baze podataka i njegovih zavisne resursa. Zamijeni sve `{variables}` vrijednostima specifične za svoju pretplatu i resursi (ukloniti **{}** prilikom postavljanja vlastitih vrijednosti).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## <a name="next-steps"></a>Daljnji koraci
Nakon što stvorite SQL baze podataka i izvršavati zadatke postavljanja osnovni baze podataka, spremni ste za sljedeće:

- [Upravljanje bazom podataka SQL sa servisom PowerShell](sql-database-manage-powershell.md)
- [Povezivanje s bazom podataka SQL s SQL Server Management Studio i obaviti primjer T SQL upita](sql-database-connect-query-ssms.md)


## <a name="additional-resources"></a>Dodatni resursi

- [Cmdleti za baze podataka azure SQL] (https://msdn.microsoft.com/library/azure/mt574084(v=azure.300\).aspx)
- [Baze podataka Azure SQL](https://azure.microsoft.com/documentation/services/sql-database/)
