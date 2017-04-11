<properties
   pageTitle="Korištenje Hadoop Svinja s .NET u HDInsight | Microsoft Azure"
   description="Saznajte kako koristiti .NET SDK Hadoop slanje zadataka Svinja Hadoop na HDInsight."
   services="hdinsight"
   documentationCenter=".net"
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
   tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="larryfr"/>

#<a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a>Pokrenite Svinja zadatke pomoću .NET SDK za Hadoop u HDInsight

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Ovaj dokument sadrži primjer korištenja .NET SDK Hadoop slanje zadataka Svinja Hadoop na klasteru HDInsight.

HDInsight .NET SDK nudi biblioteke klijent .NET koji olakšava rad s HDInsight klastere iz .NET. Svinja omogućuje vam stvaranje operacije MapReduce po Modeliranje niz transformacije podataka. Ćete saznati kako koristiti osnovne C# aplikacije sustava za slanje Svinja posla programa klaster HDInsight.

## <a name="prerequisites"></a>Preduvjeti

Da biste dovršili korake u ovom članku, morate sljedeće.

* Klaster programa Azure HDInsight (Hadoop na HDInsight) (ili Windows sustavom Linux ili).
* Visual Studio 2012 ili 2013 ili 2015.

## <a name="create-the-application"></a>Stvaranje aplikacije

HDInsight .NET SDK nudi biblioteke klijenta za .NET, što olakšava rad s HDInsight klastere iz .NET. 


1. Otvorite Visual Studio 2012 ili 2013
2. Na izborniku **datoteka** , odaberite **Novo** , a zatim odaberite **projekta**.
3. Novi projekt, unesite ili odaberite sljedeće vrijednosti.

    <table>
    <tr>
    <th>Svojstvo</th>
    <th>Vrijednost</th>
    </tr>
    <tr>
    <th>Kategorija</th>
    <th>Predlošci/Visual C# / sustava Windows</th>
    </tr>
    <tr>
    <th>Predložak</th>
    <th>Aplikacije konzole</th>
    </tr>
    <tr>
    <th>Ime</th>
    <th>SubmitPigJob</th>
    </tr>
    </table>
4. Kliknite **u redu** da biste stvorili projekta.
5. Na izborniku **Alati** odaberite **Upravitelj paketa biblioteke** ili **Upravitelj Nuget paketa**, a zatim **Konzole za Upravitelj paketa**.
6. Pokrenite sljedeću naredbu na konzoli za instalaciju paketa .NET SDK.

        Install-Package Microsoft.Azure.Management.HDInsight.Job

7. Iz programa Explorer rješenja, dvokliknite **Program.cs** da biste ga otvorili. Zamijenite postojeće kod sljedeće.

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

                    SubmitPigJob();

                    System.Console.WriteLine("Press ENTER to continue ...");
                    System.Console.ReadLine();
                }

                private static void SubmitPigJob()
                {
                    var parameters = new PigJobSubmissionParameters
                    {
                        Query = @"LOGS = LOAD 'wasbs:///example/data/sample.log';
                                    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                    RESULT = order FREQUENCIES by COUNT desc;
                                    DUMP RESULT;"
                    };

                    System.Console.WriteLine("Submitting the Pig job to the cluster...");
                    var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                    System.Console.WriteLine("Validating that the response is as expected...");
                    System.Console.WriteLine("Response status code is " + response.StatusCode);
                    System.Console.WriteLine("Validating the response object...");
                    System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
                }
            }
        }


7. Pritisnite **F5** da biste pokrenuli aplikaciju.
8. Pritisnite **ENTER** da biste izašli iz aplikacije.

## <a name="summary"></a>Sažetak

Kao što vidite, .NET SDK Hadoop omogućuje stvaranje .NET aplikacijama slanje Svinja zadatke programa HDInsight klaster i praćenje stanja zadatka.

## <a name="next-steps"></a>Daljnji koraci

Općenite informacije o Svinja u HDInsight.

* [Korištenje Svinja s Hadoop na HDInsight](hdinsight-use-pig.md)

Dodatne informacije o drugim načinima možete raditi s Hadoop na HDInsight.

* [Korištenje grozd s Hadoop na HDInsight](hdinsight-use-hive.md)

* [Korištenje MapReduce s Hadoop na HDInsight](hdinsight-use-mapreduce.md) [portal pretpregled]: https://portal.azure.com/
