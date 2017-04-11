<properties
    pageTitle="Analiza podataka Twitter pomoću Hadoop u HDInsight | Microsoft Azure"
    description="Saznajte kako koristiti grozd radi analize podataka Twitter na Hadoop u HDInsight da biste pronašli željenu učestalost korištenje određene riječi."
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="jgao"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analiziranje podataka Twitter pomoću grozd u HDInsight

Društvene web-mjesta su jedna od glavnih vožnju prisilno za prihvaćanja velikih skupova podataka. Javni API-ji nudi web-mjesta kao što su Twitter su korisne izvora podataka za analizu i razumijevanje popularne trendova. U ovom ćete praktičnom vodiču će dobiti tweets pomoću Twitter strujanje API-JA, a zatim koristiti vrste Hive Apache na Azure HDInsight da biste dobili popis korisnika Twitter koja je poslala Većina tweets koji se nalaze određene riječi.

> [AZURE.NOTE] Koraci u ovom dokumentu potreban je utemeljen na sustavu Windows HDInsight klaster. Upute određene klaster sa sustavom Linux potražite u članku [Twitteru analiza podataka pomoću grozd u HDInsight (Linux)](hdinsight-analyze-twitter-data-linux.md).


##<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- **Na radne stanice** sa servisom PowerShell Azure instalacije i konfiguracije. 

    Za izvođenje skripte komponente Windows PowerShell, morate Azure PowerShell Pokreni kao administrator i postavite pravila izvođenja *RemoteSigned*. U odjeljku [skripte komponente Windows PowerShell pokrenite][powershell-script].

    Prije pokretanja skripte komponente Windows PowerShell, provjerite jeste li povezani u pretplatu za Azure pomoću cmdleta sustava sljedeće:

        Login-AzureRmAccount

    Ako imate više pretplata Azure, koristite sljedeći cmdlet da biste postavili trenutne pretplate:

        Select-AzureRmSubscription -SubscriptionID <Azure Subscription ID>

    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Klaster programa Azure HDInsight**. Upute za dodjelu resursa klaster, potražite u članku [Prvi koraci pri korištenju HDInsight] [ hdinsight-get-started] ili [HDInsight Dodjela resursa za klastere] [hdinsight-provision]. Trebat će vam naziv klaster kasnije u ovom praktičnom vodiču.

U sljedećoj su tablici navedeni datoteke koji se koriste u ovom ćete praktičnom vodiču:

Datoteka|Opis
---|---
/tutorials/twitter/Data/tweets.txt|Izvor podataka za posao grozd.
/tutorials/twitter/Output|Izlazna datoteka za posao grozd. Zadani grozd posao izlazni naziv datoteke je **000000_0**.
tutorials/twitter/twitter.hql|Datoteka skripte HiveQL.
/tutorials/twitter/jobstatus|Hadoop stanja zadatka.


##<a name="get-twitter-feed"></a>Dohvaćanje Twitter sažetka sadržaja

U ovom ćete praktičnom vodiču će koristiti [strujanje API na Twitteru][twitter-streaming-api]. Određenim na Twitteru strujanje API koristit će se [statusi/filtar][twitter-statuses-filter].

>[AZURE.NOTE] Datoteka koja sadrži 10 000 tweets i grozd skriptna datoteka (obrađen u sljedećem odjeljku) su preneseni u spremniku javno Blob. Ako želite koristiti prenesenih datoteka, preskočite ovaj odjeljak.

[Tweets podaci](https://dev.twitter.com/docs/platform-objects/tweets) se pohranjuju u obliku JavaScript objekt notacije (JSON) koji sadrži složenoj strukturi ugniježđene. Umjesto više redaka koda za pisanje pomoću običan programski jezik, možete transformirati ove ugniježđene strukture u tablicu vrste Hive, tako da ga možete mu tako da na Structured Query Language (SQL) – kao što su jezika koji se naziva HiveQL.

Twitter koristi OAuth radi olakšavanja ovlašteni pristup njegov API-JA. OAuth je protokolu provjere autentičnosti koje korisnicima omogućuje odobriti aplikacije djelovanje u njihovo ime bez omogućivanja zajedničkog korištenja svoju lozinku. Dodatne informacije možete pronaći u [oauth.net](http://oauth.net/) ili izvrstan [Vodič za početnike za OAuth](http://hueniverse.com/oauth/) Hueniverse.

Prvi korak da biste koristili OAuth je da biste stvorili novu aplikaciju na web-mjestu na Twitteru za razvojne inženjere.

**Da biste stvorili Twitter aplikacije**

1. Prijavite se u [https://apps.twitter.com/](https://apps.twitter.com/). Ako nemate račun za Twitter, kliknite vezu **prijavite se odmah** .
2. Kliknite **Stvori novu aplikaciju**.
3. Unesite **naziv**, **Opis** **web-mjesta**. Možete čine URL-a za polje **web-mjesto** . Sljedeća tablica prikazuje neke ogledne vrijednosti za korištenje:

    Polje|Vrijednost
    ---|---
    Ime|MyHDInsightApp
    Opis|MyHDInsightApp
    Web-mjesta|http://www.myhdinsightapp.com

4. Provjerite **Da, se slažete**, a zatim **Stvori aplikacija Twitter**.
5. Kliknite karticu **dozvole** . Dozvola za zadane je **samo za čitanje**. To je dovoljno za ovog praktičnog vodiča.
6. Kliknite karticu **ključeva i tokeni programa Access** .
7. Kliknite **Stvori token za pristup**.
8. U gornjem desnom kutu stranice kliknite **Test OAuth** .
9. Zapišite **potrošača ključ**, **tajna potrošača**, **token za pristup**i **pristup tokena tajna**. Trebat će vam vrijednosti kasnije u ovom praktičnom vodiču.

U ovom ćete praktičnom vodiču da biste web-servisa poziva koristit će komponente Windows PowerShell. .NET C# uzorka, potražite u članku [Analiza u stvarnom vremenu Twitter šalju s HBase u HDInsight][hdinsight-hbase-twitter-sentiment]. Drugi popularne alat za pozive servisa web je [*Curl*][curl]. Zakretanja mogu se preuzeti sa [ovdje][curl-download].

>[AZURE.NOTE] Kada koristite naredbu zakretanja u sustavu Windows, koristite dvostrukih navodnika umjesto jednostruke navodnike za mogućnost vrijednosti.

**Da biste dobili tweets**

1. Otvorite Windows PowerShell integrirani skriptiranja okruženje (filtar). (Na zaslonu Start u sustavu Windows 8 upišite **PowerShell_ISE** i zatim kliknite **Očisti filtar za Windows**. Potražite u članku [Pokretanje komponente Windows PowerShell u sustavu Windows 8 i Windows][powershell-start].)

2. Kopirajte sljedeću skriptu u oknu skripte:

        #region - variables and constants
        $clusterName = "<HDInsightClusterName>" # Enter the HDInsight cluster name

        # Enter the OAuth information for your Twitter application
        $oauth_consumer_key = "<TwitterAppConsumerKey>";
        $oauth_consumer_secret = "<TwitterAppConsumerSecret>";
        $oauth_token = "<TwitterAppAccessToken>";
        $oauth_token_secret = "<TwitterAppAccessTokenSecret>";

        $destBlobName = "tutorials/twitter/data/tweets.txt" # This script saves the tweets into this blob.

        $trackString = "Azure, Cloud, HDInsight" # This script gets the tweets containing these keywords.
        $track = [System.Uri]::EscapeDataString($trackString);
        $lineMax = 10000  # The script will get this number of tweets. It is about 3 minutes every 100 lines.
        #endregion

        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        Login-AzureRmAccount
        #endregion

        #region - Create a block blob object for writing tweets into Blob storage
        Write-Host "Get the default storage account name and Blob container name using the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -Name $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $storageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $containerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $storageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $containerName." -ForegroundColor Yellow

        Write-Host "Define the Azure storage connection string ..." -ForegroundColor Green
        $storageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$storageAccountName;AccountKey=$storageAccountKey"
        Write-Host "`tThe connection string is $storageConnectionString." -ForegroundColor Yellow

        Write-Host "Create block blob object ..." -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($containerName)
        $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)
        #end region

        # region - Format OAuth strings
        Write-Host "Format oauth strings ..." -ForegroundColor Green
        $oauth_nonce = [System.Convert]::ToBase64String([System.Text.Encoding]::ASCII.GetBytes([System.DateTime]::Now.Ticks.ToString()));
        $ts = [System.DateTime]::UtcNow - [System.DateTime]::ParseExact("01/01/1970", "dd/MM/yyyy", $null)
        $oauth_timestamp = [System.Convert]::ToInt64($ts.TotalSeconds).ToString();

        $signature = "POST&";
        $signature += [System.Uri]::EscapeDataString("https://stream.twitter.com/1.1/statuses/filter.json") + "&";
        $signature += [System.Uri]::EscapeDataString("oauth_consumer_key=" + $oauth_consumer_key + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_nonce=" + $oauth_nonce + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_signature_method=HMAC-SHA1&");
        $signature += [System.Uri]::EscapeDataString("oauth_timestamp=" + $oauth_timestamp + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_token=" + $oauth_token + "&");
        $signature += [System.Uri]::EscapeDataString("oauth_version=1.0&");
        $signature += [System.Uri]::EscapeDataString("track=" + $track);

        $signature_key = [System.Uri]::EscapeDataString($oauth_consumer_secret) + "&" + [System.Uri]::EscapeDataString($oauth_token_secret);

        $hmacsha1 = new-object System.Security.Cryptography.HMACSHA1;
        $hmacsha1.Key = [System.Text.Encoding]::ASCII.GetBytes($signature_key);
        $oauth_signature = [System.Convert]::ToBase64String($hmacsha1.ComputeHash([System.Text.Encoding]::ASCII.GetBytes($signature)));

        $oauth_authorization = 'OAuth ';
        $oauth_authorization += 'oauth_consumer_key="' + [System.Uri]::EscapeDataString($oauth_consumer_key) + '",';
        $oauth_authorization += 'oauth_nonce="' + [System.Uri]::EscapeDataString($oauth_nonce) + '",';
        $oauth_authorization += 'oauth_signature="' + [System.Uri]::EscapeDataString($oauth_signature) + '",';
        $oauth_authorization += 'oauth_signature_method="HMAC-SHA1",'
        $oauth_authorization += 'oauth_timestamp="' + [System.Uri]::EscapeDataString($oauth_timestamp) + '",'
        $oauth_authorization += 'oauth_token="' + [System.Uri]::EscapeDataString($oauth_token) + '",';
        $oauth_authorization += 'oauth_version="1.0"';

        $post_body = [System.Text.Encoding]::ASCII.GetBytes("track=" + $track);
        #endregion

        #region - Read tweets
        Write-Host "Create HTTP web request ..." -ForegroundColor Green
        [System.Net.HttpWebRequest] $request = [System.Net.WebRequest]::Create("https://stream.twitter.com/1.1/statuses/filter.json");
        $request.Method = "POST";
        $request.Headers.Add("Authorization", $oauth_authorization);
        $request.ContentType = "application/x-www-form-urlencoded";
        $body = $request.GetRequestStream();

        $body.write($post_body, 0, $post_body.length);
        $body.flush();
        $body.close();
        $response = $request.GetResponse() ;

        Write-Host "Start stream reading ..." -ForegroundColor Green

        Write-Host "Define a MemoryStream and a StreamWriter for writing ..." -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream

        $sReader = New-Object System.IO.StreamReader($response.GetResponseStream())

        $inrec = $sReader.ReadLine()
        $count = 0
        while (($inrec -ne $null) -and ($count -le $lineMax))
        {
            if ($inrec -ne "")
            {
                Write-Host "`n`t $count tweets received." -ForegroundColor Yellow

                $writeStream.WriteLine($inrec)
                $count ++
            }

            $inrec=$sReader.ReadLine()
        }
        #endregion

        #region - Write tweets to Blob storage
        Write-Host "Write to the destination blob ..." -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $destBlob.UploadFromStream($memStream)

        $sReader.close()
        #endregion

        Write-Host "Completed!" -ForegroundColor Green

3. Postavljanje prvog pet do osam varijable u skripti:


    Varijabla|Opis
    ---|---
    $clusterName|To je naziv klaster HDInsight mjesto na koje želite li pokrenuti aplikaciju.
    $oauth_consumer_key|Ovo je Twitter aplikacije **potrošača ključ** koji zapisali ranije prilikom stvaranja aplikacije Twitter.
    $oauth_consumer_secret|Ovo je Twitter aplikaciji **tajna potrošača** ste zapisali neke starije verzije.
    $oauth_token|Ovo je Twitter aplikacije **token pristup** ste zapisali neke starije verzije.
    $oauth_token_secret|Ovo je Twitter aplikacije **programa access tokena tajna** ste zapisali neke starije verzije.
    $destBlobName|To je naziv blob izlaz. Zadana vrijednost je **tutorials/twitter/data/tweets.txt**. Ako promijenite zadane vrijednosti, morat ćete ažurirati skripte komponente Windows PowerShell sukladno tome.
    $trackString|Web-servisa koji će vratiti tweets koji se odnose na te ključne riječi. Zadana vrijednost je **Azure, oblaka, HDInsight**. Ako promijenite zadane vrijednosti koje će se ažurirati skripte komponente Windows PowerShell sukladno tome.
    $lineMax|Vrijednost određuje koliko tweets skripta će se čitati. Da biste pročitali 100 tweets traje tri minute. Možete postaviti veći broj, ali će trebati više vremena da biste preuzeli.

5. Pritisnite **F5** da biste pokrenuli skriptu. Ako naiđete na probleme, kao zaobilazno rješenje, odaberite sve retke, a zatim pritisnite tipku **F8**.
6. Prikazat će "Dovršeno!" na kraju izlaz. Prikazat će se poruka o pogrešci u crvenoj boji.

Kao postupak provjere valjanosti, Izlazna datoteka **/tutorials/twitter/data/tweets.txt**, možete provjeriti na vašem Azure blobova pomoću programa explorer Azure prostora za pohranu ili Azure PowerShell. Ogledne skripte komponente Windows PowerShell za stvaranje popisa datoteka, potražite u članku [Korištenje blobova s HDInsight][hdinsight-storage-powershell].



##<a name="create-hiveql-script"></a>Stvaranje HiveQL skripte

Pomoću komponente PowerShell Azure, možete pokrenuti višestruke izjave o HiveQL jedan po jedan ili pakiranje izjava HiveQL u skriptna datoteka. U ovom ćete praktičnom vodiču će stvoriti skriptu HiveQL. Datoteka skripte morate prenijeti spremište blobova platforme Azure. U sljedećem odjeljku će pokrenuti skriptna datoteka pomoću Azure PowerShell.

>[AZURE.NOTE] Datoteka skripte grozd i datoteku koja sadrži 10 000 tweets su preneseni u spremniku javno Blob. Ako želite koristiti prenesenih datoteka, preskočite ovaj odjeljak.

Skripta HiveQL će učinite sljedeće:

1. **Odbacivanje tweets_raw tablice** u slučaju tablice već postoji.
2. **Stvorite tablicu vrste Hive tweets_raw**. Privremena tablica strukturirane grozd sadrži podatke za daljnje Izdvoji, pretvaranje i učitavanje obrada (ETL). Informacije o particije potražite u članku [vrste Hive vodič][apache-hive-tutorial].  
3. **Učitavanja podataka** iz izvora mape /tutorials/twitter/data. Veliki tweets skupu podataka u obliku ugniježđene JSON sada su pretvoreni u privremene grozd strukturu tablice.
3. **Odbacivanje tweets tablice** u slučaju tablice već postoji.
4. **Stvaranje tablice tweets**. Prije nego što upit možete poslati protiv tweets dataset pomoću grozd, morate pokrenuti drugi proces ETL. Ovaj postupak ETL definira shemu detaljnije tablice podataka pohranjenih u tablici "twitter_raw".  
5. **Umetanje tablice Prebriši**. Ova složene grozd skripta će izbaciti skup dugo poslovi MapReduce po Hadoop klaster. Ovisno o vašem skupu podataka i veličinu svoj klaster, to može potrajati 10 minuta.
6. **Umetanje prebrisati direktorija**. Izvođenje upita i izlazni skup podataka u datoteku. Ovaj upit će vratiti popis korisnika Twitter koja je poslala Većina tweets koja sadrži riječ "Azure".

**Da biste stvorili grozd skripte i prijenos Azure**

1. Otvorite Windows Očisti filtar.
2. Kopirajte sljedeću skriptu u oknu skripte:

        #region - variables and constants
        $clusterName = "<Existing HDInsight Cluster Name>" # Enter your HDInsight cluster name
        $subscriptionID = "<Azure Subscription ID>"
        
        $sourceDataPath = "/tutorials/twitter/data"
        $outputPath = "/tutorials/twitter/output"
        $hqlScriptFile = "tutorials/twitter/twitter.hql"
        
        $hqlStatements = @"
        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        
        DROP TABLE tweets_raw;
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '$sourceDataPath';
        
        DROP TABLE tweets;
        CREATE TABLE tweets
        (
            id BIGINT,
            created_at STRING,
            created_at_date STRING,
            created_at_year STRING,
            created_at_month STRING,
            created_at_day STRING,
            created_at_time STRING,
            in_reply_to_user_id_str STRING,
            text STRING,
            contributors STRING,
            retweeted STRING,
            truncated STRING,
            coordinates STRING,
            source STRING,
            retweet_count INT,
            url STRING,
            hashtags array<STRING>,
            user_mentions array<STRING>,
            first_hashtag STRING,
            first_user_mention STRING,
            screen_name STRING,
            name STRING,
            followers_count INT,
            listed_count INT,
            friends_count INT,
            lang STRING,
            user_location STRING,
            time_zone STRING,
            profile_image_url STRING,
            json_response STRING
        );
        
        FROM tweets_raw
        INSERT OVERWRITE TABLE tweets
        SELECT
            cast(get_json_object(json_response, '$.id_str') as BIGINT),
            get_json_object(json_response, '$.created_at'),
            concat(substr (get_json_object(json_response, '$.created_at'),1,10),' ',
            substr (get_json_object(json_response, '$.created_at'),27,4)),
            substr (get_json_object(json_response, '$.created_at'),27,4),
            case substr (get_json_object(json_response, '$.created_at'),5,3)
                when "Jan" then "01"
                when "Feb" then "02"
                when "Mar" then "03"
                when "Apr" then "04"
                when "May" then "05"
                when "Jun" then "06"
                when "Jul" then "07"
                when "Aug" then "08"
                when "Sep" then "09"
                when "Oct" then "10"
                when "Nov" then "11"
                when "Dec" then "12" end,
            substr (get_json_object(json_response, '$.created_at'),9,2),
            substr (get_json_object(json_response, '$.created_at'),12,8),
            get_json_object(json_response, '$.in_reply_to_user_id_str'),
            get_json_object(json_response, '$.text'),
            get_json_object(json_response, '$.contributors'),
            get_json_object(json_response, '$.retweeted'),
            get_json_object(json_response, '$.truncated'),
            get_json_object(json_response, '$.coordinates'),
            get_json_object(json_response, '$.source'),
            cast (get_json_object(json_response, '$.retweet_count') as INT),
            get_json_object(json_response, '$.entities.display_url'),
            array(
                trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[1].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[2].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[3].text'))),
                trim(lower(get_json_object(json_response, '$.entities.hashtags[4].text')))),
            array(
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[1].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[2].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[3].screen_name'))),
                trim(lower(get_json_object(json_response, '$.entities.user_mentions[4].screen_name')))),
            trim(lower(get_json_object(json_response, '$.entities.hashtags[0].text'))),
            trim(lower(get_json_object(json_response, '$.entities.user_mentions[0].screen_name'))),
            get_json_object(json_response, '$.user.screen_name'),
            get_json_object(json_response, '$.user.name'),
            cast (get_json_object(json_response, '$.user.followers_count') as INT),
            cast (get_json_object(json_response, '$.user.listed_count') as INT),
            cast (get_json_object(json_response, '$.user.friends_count') as INT),
            get_json_object(json_response, '$.user.lang'),
            get_json_object(json_response, '$.user.location'),
            get_json_object(json_response, '$.user.time_zone'),
            get_json_object(json_response, '$.user.profile_image_url'),
            json_response
        WHERE (length(json_response) > 500);
        
        INSERT OVERWRITE DIRECTORY '$outputPath'
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;
        "@
        #endregion
        
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        
        Try{
            Get-AzureRmSubscription
        }
        Catch{
            Login-AzureRmAccount
        }
        
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
        
        #endregion
        
        #region - Create a block blob object for writing the Hive script file
        Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
        $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroupName = $myCluster.ResourceGroup
        $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
        $defaultBlobContainerName = $myCluster.DefaultStorageContainer
        Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
        Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
        
        Write-Host "Define the connection string ..." -ForegroundColor Green
        $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
        $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"
        
        Write-Host "Create block blob objects referencing the hql script file" -ForegroundColor Green
        $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
        $storageClient = $storageAccount.CreateCloudBlobClient();
        $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
        $hqlScriptBlob = $storageContainer.GetBlockBlobReference($hqlScriptFile)
        
        Write-Host "Define a MemoryStream and a StreamWriter for writing ... " -ForegroundColor Green
        $memStream = New-Object System.IO.MemoryStream
        $writeStream = New-Object System.IO.StreamWriter $memStream
        $writeStream.Writeline($hqlStatements)
        #endregion
        
        #region - Write the Hive script file to Blob storage
        Write-Host "Write to the destination blob ... " -ForegroundColor Green
        $writeStream.Flush()
        $memStream.Seek(0, "Begin")
        $hqlScriptBlob.UploadFromStream($memStream)
        #endregion
        
        Write-Host "Completed!" -ForegroundColor Green

        

4. Postavljanje prve dvije varijable u skripti:

    Varijabla|Opis
    ---|---
    $clusterName|Unesite naziv klaster HDInsight na mjesto na koje želite li pokrenuti aplikaciju.
    $subscriptionID|Unesite svoj ID Azure pretplate.
    $sourceDataPath|Spremište blobova platforme Azure mjesto gdje će se upiti grozd čitati podatke iz. Ne morate promijeniti ovu varijablu.
    $outputPath|Prostor za pohranu blobova platforme Azure mjesto gdje grozd upiti će izlaz rezultate. Ne morate promijeniti ovu varijablu.
    $hqlScriptFile|Mjesto i naziv datoteke HiveQL skriptna datoteka. Ne morate promijeniti ovu varijablu.

5. Pritisnite **F5** da biste pokrenuli skriptu. Ako naiđete na probleme, kao zaobilazno rješenje, odaberite sve retke, a zatim pritisnite tipku **F8**.
6. Prikazat će "Dovršeno!" na kraju izlaz. Prikazat će se poruka o pogrešci u crvenoj boji.

Kao postupak provjere valjanosti, Izlazna datoteka **/tutorials/twitter/twitter.hql**, možete provjeriti na vašem Azure blobova pomoću programa explorer Azure prostora za pohranu ili Azure PowerShell. Ogledne skripte komponente Windows PowerShell za stvaranje popisa datoteka, potražite u članku [Korištenje blobova s HDInsight][hdinsight-storage-powershell].  


##<a name="process-twitter-data-by-using-hive"></a>Obrada podataka Twitter pomoću grozd

Dovršite sve Priprema napravili. Sada možete pozivanje grozd skripte i provjerili rezultate.

### <a name="submit-a-hive-job"></a>Slanje grozd posla

Da biste pokrenuli skriptu grozd, koristite sljedeću skriptu komponente Windows PowerShell. Morat ćete postaviti prvi varijabla.

>[AZURE.NOTE] Da biste koristili u tweets i skripte HiveQL koji ste prenijeli u zadnje dvije sekcije, postavite $hqlScriptFile "/ tutorials/twitter/twitter.hql". Da biste koristili one koji su preneseni na javno blob umjesto vas, postavite $hqlScriptFile "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql".

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    $httpUserName = "admin"
    $httpUserPassword = "<HDInsight Cluster HTTP User Password>"
    
    #use one of the following
    $hqlScriptFile = "wasbs://twittertrend@hditutorialdata.blob.core.windows.net/twitter.hql"
    $hqlScriptFile = "/tutorials/twitter/twitter.hql"
    
    $statusFolder = "/tutorials/twitter/jobstatus"
    #endregion
    
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    
    #region - Invoke Hive
    Write-Host "Invoke Hive ... " -ForegroundColor Green
    
    # Create the HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpUserPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)
    
    Use-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName -HttpCredential $httpCredential 
    $response = Invoke-AzureRmHDInsightHiveJob -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -file $hqlScriptFile -StatusFolder $statusFolder #-OutVariable $outVariable
    
    Write-Host "Display the standard error log ... " -ForegroundColor Green
    $jobID = ($response | Select-String job_ | Select-Object -First 1) -replace ‘\s*$’ -replace ‘.*\s’
    Get-AzureRmHDInsightJobOutput -ClusterName $clusterName -JobId $jobID -DefaultContainer $defaultBlobContainerName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -HttpCredential $httpCredential
    #endregion

### <a name="check-the-results"></a>Provjerite rezultate

Da biste provjerili izlaz posao grozd, koristite sljedeću skriptu komponente Windows PowerShell. Morat ćete postaviti prvo dvije varijable.

    #region variables and constants
    $clusterName = "<Existing Azure HDInsight Cluster Name>"
    
    $blob = "tutorials/twitter/output/000000_0" # The name of the blob to be downloaded.
    #engregion
    
    #region - Create an Azure storage context object
    Write-Host "Get the default storage account name and container name based on the cluster name ..." -ForegroundColor Green
    $myCluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $myCluster.ResourceGroup
    $defaultStorageAccountName = $myCluster.DefaultStorageAccount.Replace(".blob.core.windows.net", "")
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultBlobContainerName = $myCluster.DefaultStorageContainer
    
    Write-Host "`tThe storage account name is $defaultStorageAccountName." -ForegroundColor Yellow
    Write-Host "`tThe blob container name is $defaultBlobContainerName." -ForegroundColor Yellow
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey  
    #endregion
    
    #region - Download blob and display blob
    Write-Host "Download the blob ..." -ForegroundColor Green
    cd $HOME
    Get-AzureStorageBlobContent -Container $defaultBlobContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "Display the output ..." -ForegroundColor Green
    Write-Host "==================================" -ForegroundColor Green
    cat "./$blob"
    Write-Host "==================================" -ForegroundColor Green
    #end region

> [AZURE.NOTE] Tablicu vrste Hive koristi \001 graničnik polja. Graničnik koji nisu vidljivi u izlaz.

Kada rezultate analize ste potvrdili spremište blobova platforme Azure, možete izvesti podatke s poslužiteljem baze podataka/SQL Azure SQL, izvezite podatke u Excel pomoću dodatka Power Query ili povezati aplikacije da biste podatke pomoću ODBC upravljački program vrste Hive. Dodatne informacije potražite u članku [Korištenje Sqoop s HDInsight][hdinsight-use-sqoop], [Analiza leta podatke o kašnjenju pomoću HDInsight][hdinsight-analyze-flight-delay-data], [Povezivanje programa Excel sa servisa HDInsight pomoću značajke Power Query][hdinsight-power-query], i [Povezivanje programa Excel sa servisom HDInsight pomoću Microsoft vrste Hive ODBC upravljački program][hdinsight-hive-odbc].

##<a name="next-steps"></a>Daljnji koraci

Pomoću ovog praktičnog vodiča smo ste vidjeti kako pretvaranje nestrukturirane JSON skup podataka u strukturiranim tablicu vrste Hive upita, istraživanje i analiza podataka iz Twitter pomoću HDInsight na Azure. Da biste saznali više, pogledajte:

- [Početak rada s HDInsight][hdinsight-get-started]
- [Analiza u stvarnom vremenu Twitter šalju s HBase u HDInsight][hdinsight-hbase-twitter-sentiment]
- [Analizirati podatke o kašnjenju leta pomoću HDInsight][hdinsight-analyze-flight-delay-data]
- [Povezivanje programa Excel sa servisom HDInsight pomoću značajke Power Query][hdinsight-power-query]
- [Povezivanje programa Excel sa servisom HDInsight pomoću ODBC upravljački program Microsoft grozd][hdinsight-hive-odbc]
- [Korištenje Sqoop s HDInsight][hdinsight-use-sqoop]

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: powershell-install-configure.md
[powershell-script]: http://technet.microsoft.com/library/ee176961.aspx


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-hadoop-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-odbc-driver.md
[hdinsight-hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md
