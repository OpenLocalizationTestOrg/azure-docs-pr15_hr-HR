<properties
    pageTitle="Korištenje Hadoop Sqoop u HDInsight | Microsoft Azure"
    description="Saznajte kako koristiti HDInsight .NET SDK za pokretanje Sqoop uvoz i izvoz između programa Hadoop klaster i baze podataka Azure SQL."
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
   ms.date="09/14/2016"
    ms.author="jgao"/>

#<a name="run-sqoop-jobs-using-net-sdk-for-hadoop-in-hdinsight"></a>Pokrenite Sqoop zadatke pomoću .NET SDK za Hadoop u HDInsight

[AZURE.INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Saznajte kako pomoću servisa HDInsight .NET SDK da biste pokrenuli Sqoop zadatke u HDInsight za uvoz i izvoz između HDInsight klaster i baze podataka Azure SQL ili baze podataka SQL Server.

> [AZURE.NOTE] Koraci u ovom članku može se koristiti s ili Windows ili Linux HDInsight klaster; Međutim, ove korake će raditi samo iz klijenta za Windows. Birač tabulatora pri vrhu stranice u ovom se članku koristite da biste odabrali druge načine.

###<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **A Hadoop klaster u HDInsight**. Potražite u članku [Stvaranje klaster i SQL baze podataka](hdinsight-use-sqoop.md#create-cluster-and-sql-database).

## <a name="run-sqoop-using-net-sdk"></a>Pokrenite Sqoop pomoću .NET SDK-a

HDInsight .NET SDK nudi biblioteke klijenta za .NET, što olakšava rad s HDInsight klastere iz .NET. U ovom ćete odjeljku stvarate C# program konzole za izvoz u hivesampletable baze podataka SQL tablicu koju ste stvorili ranije u ovom vodičima.

**Da biste poslali Sqoop posla**

1. Stvaranje konzole aplikacije C# u Visual Studio.
2. Konzoli sustava Upravitelj paketa za Visual Studio, pokrenite sljedeću naredbu Nuget da biste uvezli paketa.

        Install-Package Microsoft.Azure.Management.HDInsight.Job
        
3. U datoteci Program.cs, koristite sljedeći kod:

        using System.Collections.Generic;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        
        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
        
                private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
                private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
                private const string ExistingClusterUsername = "<Cluster Username>";
                private const string ExistingClusterPassword = "<Cluster User Password>";
        
                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");
        
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
        
                    SubmitSqoopJob();
        
                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }
        
                private static void SubmitSqoopJob()
                {
                    var sqlDatabaseServerName = "<SQLDatabaseServerName>";
                    var sqlDatabaseLogin = "<SQLDatabaseLogin>";
                    var sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>";
                    var sqlDatabaseDatabaseName = "<DatabaseName>";
        
                    var tableName = "<TableName>";
                    var exportDir = "/tutorials/usesqoop/data";
        
                    // Connection string for using Azure SQL Database.
                    // Comment if using SQL Server
                    var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ".database.windows.net;user=" + sqlDatabaseLogin + "@" + sqlDatabaseServerName + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
                    // Connection string for using SQL Server.
                    // Uncomment if using SQL Server
                    //var connectionString = "jdbc:sqlserver://" + sqlDatabaseServerName + ";user=" + sqlDatabaseLogin + ";password=" + sqlDatabaseLoginPassword + ";database=" + sqlDatabaseDatabaseName;
        
                    var parameters = new SqoopJobSubmissionParameters
                    {
                        Files = new List<string> { "/user/oozie/share/lib/sqoop/sqljdbc41.jar" }, // This line is required for Linux-based cluster.
                        Command = "export --connect " + connectionString + " --table " + tableName + "_mobile --export-dir " + exportDir + "_mobile --fields-terminated-by \\t -m 1"
                    };
        
                    System.Console.WriteLine("Submitting the Sqoop job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitSqoopJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }
        
4. Pritisnite **F5** da biste pokrenuli program. 

##<a name="limitations"></a>Ograničenja

* Skupno izvoz - Linux s operacijskim sustavom HDInsight, Sqoop poveznik za izvoz podataka u Microsoft SQL Server ili baze podataka SQL Azure trenutno ne podržava umeće skupno.

* Grupno slanje promjena - sa sustavom Linux HDInsight kada se koristi u `-batch` prebacivanje prilikom izvršavanja umeće, Sqoop će obavljanje više umeće umjesto grupnog slanja promjena operacije Umetanje.

##<a name="next-steps"></a>Daljnji koraci

Sada ste naučili kako koristiti Sqoop. Da biste saznali više, pogledajte:

- [Korištenje Oozie s HDInsight](hdinsight-use-oozie.md): korištenje Sqoop akcije u tijeku rada Oozie.
- [Analiza leta podatke o kašnjenju pomoću HDInsight](hdinsight-analyze-flight-delay-data.md): korištenje vrste Hive da biste analizirali leta odgađanje podataka, a zatim pomoću Sqoop za izvoz podataka u bazu podataka Azure SQL.
- [Prijenos podataka HDInsight](hdinsight-upload-data.md): pronalaženje drugih načina za prijenos podataka u spremište blobova platforme HDInsight/Azure.


