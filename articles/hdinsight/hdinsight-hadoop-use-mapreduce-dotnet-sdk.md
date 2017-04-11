<properties
    pageTitle="Slanje MapReduce zadatke pomoću servisa HDInsight .NET SDK | Microsoft Azure"
    description="Saznajte kako poslati MapReduce zadataka Azure HDInsight Hadoop pomoću HDInsight .NET SDK."
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
   ms.date="10/28/2016"
    ms.author="jgao"/>

# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Pokrenite MapReduce zadatke pomoću servisa HDInsight .NET SDK

[AZURE.INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Saznajte kako poslati MapReduce zadatke pomoću HDInsight .NET SDK. HDInsight klastere isporučuje posudu datoteku s nekim MapReduce uzorka. Datoteka posudu je */example/jars/hadoop-mapreduce-examples.jar*.  Jedna od primjera jest *wordcount*. Razvijajte C# program konzole za slanje wordcount posla.  Posao čita */example/data/gutenberg/davinci.txt* datoteku, a proizvodi rezultate u */example/data/davinciwordcount*.  Ako želite ponovno pokrenite aplikaciju, morate očistiti Izlazna datoteka.

> [AZURE.NOTE] Koraci u ovom članku moraju izvršiti iz klijenta za Windows. Informacije o korištenju Linux, OS X ili Unix klijenta za rad s grozd, koristite birač tabulatora koji se prikazuje pri vrhu stranice u članku.

##<a name="prerequisites"></a>Preduvjeti

Prije nego počnete u ovom se članku, morate imati sljedeće:

- **A Hadoop klaster u HDInsight**. Potražite u članku [Početak rada s operacijskim sustavom Linux Hadoop u HDInsight](hdinsight-use-sqoop.md#create-cluster-and-sql-database).
- **Visual Studio 2012/2013/2015**.

##<a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Slanje MapReduce zadatke pomoću servisa HDInsight .NET SDK

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

                    SubmitMRJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitHiveJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };

                    var paras = new MapReduceJobSubmissionParameters
                    {
                        //Content = "wordcount  ",
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };

                    System.Console.WriteLine("Submitting the MR job to the cluster...");
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

- Stvaranje klaster i predavanje posla grozd, potražite u članku [Početak rada sa servisom Azure HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).
- Stvaranje klastere HDInsight potražite u članku [Stvaranje Linux sustavom Hadoop klastere u HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
- Upravljanje klastere servisa HDInsight potražite u članku [Upravljanje Hadoop klastere u HDInsight](hdinsight-administer-use-management-portal.md).
- Za učenje HDInsight .NET SDK potražite u članku [Referenca HDInsight .NET SDK](https://msdn.microsoft.com/library/mt271028.aspx).
- Za koji nije interaktivan autentičnost Azure potražite u članku [Stvaranje aplikacija za .NET HDInsight koji nije interaktivan provjere autentičnosti](hdinsight-create-non-interactive-authentication-dotnet-applications.md).




