<properties
    pageTitle="Izvođenje upita grozd pomoću HDInsight .NET SDK | Microsoft Azure"
    description="Saznajte kako poslati Hadoop zadataka Azure HDInsight Hadoop pomoću HDInsight .NET SDK."
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

# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Izvođenje upita grozd pomoću HDInsight .NET SDK

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


Saznajte kako poslati grozd upita pomoću HDInsight .NET SDK.

> [AZURE.NOTE] Koraci u ovom članku moraju izvršiti iz klijenta za Windows. Informacije o korištenju Linux, OS X ili Unix klijenta za rad s grozd, koristite birač tabulatora koji se prikazuje pri vrhu stranice u članku.

##<a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **A Hadoop klaster u HDInsight**. Potražite u članku [Početak rada s operacijskim sustavom Linux Hadoop u HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Slanje upita grozd pomoću HDInsight .NET SDK

HDInsight .NET SDK nudi biblioteke klijenta za .NET, što olakšava rad s HDInsight klastere iz .NET. 

**Da biste poslali poslova**

1. Stvaranje konzole aplikacije C# u Visual Studio.
2. S konzole Upravitelj paketa Nuget pokrenite sljedeću naredbu.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

2. Koristite sljedeći kod:

        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
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

                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";

                static void Main(string[] args)
                {
                    System.Console.WriteLine("The application is running ...");

                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                    SubmitHiveJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "ravi" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the Hive job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);

                    System.Console.WriteLine("Waiting for the job completion ...");

                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }

                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure

                    System.Console.WriteLine("Job output is: ");

                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }

5. Pritisnite **F5** da biste pokrenuli aplikaciju.


## <a name="next-steps"></a>Daljnji koraci

U ovom se članku ste naučili nekoliko načina stvaranja programa klaster HDInsight. Dodatne informacije potražite u sljedećim člancima:

* [Početak rada sa servisom Azure HDInsight][hdinsight-get-started]
* [Stvaranje klastere Hadoop u HDInsight][hdinsight-provision]
* [Upravljanje Hadoop klastere u HDInsight pomoću portala za Azure](hdinsight-administer-use-management-portal.md)
* [Referenca HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx)
* [Korištenje Svinja s HDInsight](hdinsight-use-pig.md)
* [Korištenje Sqoop s HDInsight](hdinsight-use-sqoop-mac-linux.md)
* [Stvaranje aplikacije .NET HDInsight koji nije interaktivan provjere autentičnosti](hdinsight-create-non-interactive-authentication-dotnet-applications.md)


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


