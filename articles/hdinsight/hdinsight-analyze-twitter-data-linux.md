<properties
    pageTitle="Analiza podataka Twitter pomoću Apache vrste Hive na HDInsight | Microsoft Azure"
    description="Saznajte kako koristiti Python za spremanje Tweets koji sadrže određene ključne riječi, a zatim pretvaranje neobrađenog TWitter podataka u tablicu vrste Hive pretraživati pomoću grozd i Hadoop na HDInsight."
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="larryfr"/>

# <a name="analyze-twitter-data-using-hive-in-hdinsight"></a>Analiziranje podataka Twitter pomoću grozd u HDInsight

U ovom dokumentu, dobit ćete tweets pomoću Twitter strujanje API-JA, a zatim korištenje vrste Hive Apache na sustavom Linux HDInsight klaster u tijeku s JSON oblikovani podaci. Rezultat će biti popisa korisnika Twitter koja je poslala Većina tweets koji se nalaze određene riječi.

> [AZURE.NOTE] Tijekom korištenja pojedinačnih dijelova dokumenta s utemeljen na sustavu Windows HDInsight klastere (Python, primjerice), mnogi koraci temelje se na korištenje sustavom Linux HDInsight klaster. Upute određene na klaster utemeljen na sustavu Windows potražite u članku [Twitteru analiza podataka pomoću grozd u HDInsight](hdinsight-analyze-twitter-data.md).

###<a name="prerequisites"></a>Preduvjeti

Prije početka ovog praktičnog vodiča, morate imati sljedeće:

- __Klaster sustavom Linux Azure HDInsight__. Informacije o stvaranju klaster potražite u članku [Početak rada sa sustavom Linux HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md) upute o stvaranju klaster.

- Za __SSH klijenta__. Dodatne informacije o korištenju SSH s operacijskim sustavom Linux HDInsight potražite u sljedećim člancima:

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows.md)

- __Python__ i [točaka](https://pypi.python.org/pypi/pip)

##<a name="get-a-twitter-feed"></a>Početak Twitter sažetka sadržaja

Twitter omogućuje vam da dohvatite [podatke za svaku tweet](https://dev.twitter.com/docs/platform-objects/tweets) kao dokumenta JavaScript objekt notaciju (JSON) putem REST API-JA. [OAuth](http://oauth.net) je potreban za provjeru autentičnosti da biste na API-JA. Morate stvoriti i _Na Twitteru aplikacije_ koja sadrži postavke koje se koriste za pristup s API-JA.

###<a name="create-a-twitter-application"></a>Stvaranje aplikacije Twitter

1. U web-pregledniku prijavite [https://apps.twitter.com/](https://apps.twitter.com/). Ako nemate račun za Twitter, kliknite vezu **prijavite se odmah** .
2. Kliknite **Stvori novu aplikaciju**.
3. Unesite **naziv**, **Opis** **web-mjesta**. Možete čine URL-a za polje **web-mjesto** . Sljedeća tablica prikazuje neke ogledne vrijednosti za korištenje:

  	| Polje | Vrijednost |
  	|:----- |:----- |
  	| Ime  | MyHDInsightApp |
  	| Opis | MyHDInsightApp |
  	| Web-mjesta | http://www.myhdinsightapp.com |
    
4. Provjerite **Da, se slažete**, a zatim **Stvori aplikacija Twitter**.
5. Kliknite karticu **dozvole** . Dozvola za zadane je **samo za čitanje**. To je dovoljno za ovog praktičnog vodiča.
6. Kliknite karticu **ključeva i tokeni programa Access** .
7. Kliknite **Stvori token za pristup**.
8. U gornjem desnom kutu stranice kliknite **Test OAuth** .
9. Zapišite **potrošača ključ**, **tajna potrošača**, **token za pristup**i **pristup tokena tajna**. Vrijednosti će zatrebate kasnije.

>[AZURE.NOTE] Kada koristite naredbu zakretanja u sustavu Windows, koristite dvostrukih navodnika umjesto jednostruke navodnike za mogućnost vrijednosti.

###<a name="download-tweets"></a>Preuzmite tweets

Sljedeći kod Python će preuzmite 10 000 tweets iz Twitter i spremite ih u datoteku pod nazivom __tweets.txt__.

> [AZURE.NOTE] Sljedeći koraci se izvode na klaster HDInsight jer Python već instaliran.

1. Povezivanje s klaster HDInsight pomoću SSH:

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
        
    Ako ste koristili lozinku radi zaštite SSH korisnički račun, morat ćete unijeti. Ako ste koristili javni ključ, možda ćete morati koristiti u `-i` parametar da biste odredili koji se podudaraju privatni ključ. Na primjer, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.
        
    Dodatne informacije o korištenju SSH s operacijskim sustavom Linux HDInsight potražite u sljedećim člancima:
    
    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight Linux, Unix ili OS X](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [Korištenje SSH sa sustavom Linux Hadoop na HDInsight iz sustava Windows](hdinsight-hadoop-linux-use-ssh-windows)
    
2. Prema zadanim postavkama uslužni __točaka__ nije instaliran na glavni čvor HDInsight. Koristite sljedeće da biste instalirali, a zatim ažurirajte uslužni:

        sudo apt-get install python-pip
        sudo pip install --upgrade pip

3. Kod da biste preuzeli tweets ovisi o [Tweepy](http://www.tweepy.org/) i [Progressbar](https://pypi.python.org/pypi/progressbar/2.2). Da biste ih instalirali, koristite sljedeću naredbu:

        sudo apt-get install python-dev libffi-dev libssl-dev
        sudo apt-get remove python-openssl
        sudo pip install tweepy progressbar pyOpenSSL requests[security]
        
    > [AZURE.NOTE] Bitova o uklanjanju python-openssl, instalacije python razvojni, libffi razvojni, libssl razvojni, pyOpenSSL i zahtjevi za [sigurnost] je da biste izbjegli programa InsecurePlatform Upozorenje pri povezivanju s Twitter putem SSL iz Python.
    >
    > Da biste izbjegli [pogreške](https://github.com/tweepy/tweepy/issues/576) koje se mogu pojaviti prilikom obrade tweets koristi se Tweepy v3.2.0.

4. Da biste stvorili novu datoteku pod nazivom __gettweets.py__, koristite sljedeću naredbu:

        nano gettweets.py

5. Koristite sljedeće kao sadržaj datoteke __gettweets.py__ . Zamjena podatke rezerviranog mjesta za __potrošača\_tajna__, __potrošača\_ključ__, __pristup /\_tokena__, i __pristup\_tokena\_tajna__ s podacima iz aplikacije Twitter.

        #!/usr/bin/python

        from tweepy import Stream, OAuthHandler
        from tweepy.streaming import StreamListener
        from progressbar import ProgressBar, Percentage, Bar
        import json
        import sys

        #Twitter app information
        consumer_secret='Your consumer secret'
        consumer_key='Your consumer key'
        access_token='Your access token'
        access_token_secret='Your access token secret'

        #The number of tweets we want to get
        max_tweets=10000

        #Create the listener class that will receive and save tweets
        class listener(StreamListener):
            #On init, set the counter to zero and create a progress bar
            def __init__(self, api=None):
                self.num_tweets = 0
                self.pbar = ProgressBar(widgets=[Percentage(), Bar()], maxval=max_tweets).start()

            #When data is received, do this
            def on_data(self, data):
                #Append the tweet to the 'tweets.txt' file
                with open('tweets.txt', 'a') as tweet_file:
                    tweet_file.write(data)
                    #Increment the number of tweets
                    self.num_tweets += 1
                    #Check to see if we have hit max_tweets and exit if so
                    if self.num_tweets >= max_tweets:
                        self.pbar.finish()
                        sys.exit(0)
                    else:
                        #increment the progress bar
                        self.pbar.update(self.num_tweets)
                return True

            #Handle any errors that may occur
            def on_error(self, status):
                print status

        #Get the OAuth token
        auth = OAuthHandler(consumer_key, consumer_secret)
        auth.set_access_token(access_token, access_token_secret)
        #Use the listener class for stream processing
        twitterStream = Stream(auth, listener())
        #Filter for these topics
        twitterStream.filter(track=["azure","cloud","hdinsight"])

6. Da biste spremili datoteku pomoću __Ctrl + X__, a zatim __Y__ .

7. Da biste pokrenuli datoteku i preuzeli tweets, koristite sljedeću naredbu:

        python gettweets.py

    Pokazatelj tijeka moraju se, a Brojanje do 100%, kao što je na tweets preuzimanja i spremiti datoteku.

    > [AZURE.NOTE] Ako traje vrlo dugo trake tijeku pomicanje, trebali biste promijeniti filtar da biste pratili trendova teme; Kada je mnogo tweets o temi filtrirate na, vrlo brzo možete dobiti 10000 tweets potrebne.

###<a name="upload-the-data"></a>Prijenos podataka

Da biste podatke u WASB (raspodijeljeno datotečni sustav koristi HDInsight), koristite sljedeće naredbe:

    hdfs dfs -mkdir -p /tutorials/twitter/data
    hdfs dfs -put tweets.txt /tutorials/twitter/data/tweets.txt

Spremaju se podaci na na mjesto kojemu možete pristupiti sve čvorove u klasteru.

##<a name="run-the-hiveql-job"></a>Pokreni HiveQL


1. Da biste stvorili datoteku koja sadrži naredbe HiveQL, koristite sljedeću naredbu:

        nano twitter.hql
    
    Koristite sljedeće kao sadržaj datoteke:

        set hive.exec.dynamic.partition = true;
        set hive.exec.dynamic.partition.mode = nonstrict;
        -- Drop table, if it exists
        DROP TABLE tweets_raw;
        -- Create it, pointing toward the tweets logged from Twitter
        CREATE EXTERNAL TABLE tweets_raw (
            json_response STRING
        )
        STORED AS TEXTFILE LOCATION '/tutorials/twitter/data';
        -- Drop and recreate the destination table
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
        -- Select tweets from the imported data, parse the JSON,
        -- and insert into the tweets table
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
        
        
3. Pritisnite __Ctrl + X__, a zatim pritisnite __Y__ da biste spremili datoteku.

4. Da biste pokrenuli HiveQL koje se nalaze u datoteci, koristite sljedeću naredbu:

        beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin -i twitter.hql
        
    To će učitavanje ljuske grozd, pokretanje u HiveQL u datoteci __twitter.hql__ i na kraju vratili na `jdbc:hive2//localhost:10001/>` upit.
    
5. Iz upita beeline da biste provjerili možete odabrati podatke iz tablice __tweets__ koji je stvorio HiveQL u datoteci __twitter.hql__ koristite sljedeće:
        
        SELECT name, screen_name, count(1) as cc
            FROM tweets
            WHERE text like "%Azure%"
            GROUP BY name,screen_name
            ORDER BY cc DESC LIMIT 10;

    Vratit će maksimalno 10 tweets koje sadrže riječ __Azure__ u tekst poruke.

##<a name="next-steps"></a>Daljnji koraci

Pomoću ovog praktičnog vodiča smo ste vidjeti kako pretvaranje nestrukturirane JSON skup podataka u strukturiranim tablicu vrste Hive upita, istraživanje i analiza podataka iz Twitter pomoću HDInsight na Azure. Da biste saznali više, pogledajte:

- [Početak rada s HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md)
- [Analizirati podatke o kašnjenju leta pomoću HDInsight](hdinsight-analyze-flight-delay-data-linux.md)

[curl]: http://curl.haxx.se
[curl-download]: http://curl.haxx.se/download.html

[apache-hive-tutorial]: https://cwiki.apache.org/confluence/display/Hive/Tutorial

[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter
