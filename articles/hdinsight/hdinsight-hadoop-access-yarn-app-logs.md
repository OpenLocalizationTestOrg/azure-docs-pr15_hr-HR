<properties
    pageTitle="Aplikaciju programa Access Hadoop YARN zapisnike programski | Microsoft Azure"
    description="Aplikaciju programa Access programski prijavi klaster Hadoop u HDInsight."
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
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

# <a name="access-yarn-application-logs-on-windows-based-hdinsight"></a>Aplikaciju programa Access YARN prijavi HDInsight utemeljen na sustavu Windows

U ovoj se temi objašnjava kako pristupiti u zapisnicima YARN (još neki drugi resurs Negotiator) aplikacije koje ste završili na Hadoop klaster u Azure HDInsight

> [AZURE.NOTE] Informacije u ovom dokumentu odnosi samo na klastere HDInsight utemeljen na sustavu Windows. Informacije o pristupanju YARN prijavi sustavom Linux HDInsight klastere, potražite [aplikaciju programa Access YARN prijavi sustavom Linux Hadoop na HDInsight](hdinsight-hadoop-access-yarn-app-logs-linux.md)

### <a name="prerequisites"></a>Preduvjeti

- Klaster HDInsight utemeljen na sustavu Windows.  U odjeljku [klastere utemeljen na sustavu Windows za stvaranje Hadoop u HDInsight](hdinsight-provision-clusters.md).


## <a name="yarn-timeline-server"></a>Poslužitelj YARN vremenske trake

<a href="http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html" target="_blank">YARN vremenske trake Server</a> nudi generički informacije o dovršene aplikacija, kao i informacije o aplikaciji specifične za framework putem dvije različite sučelja. Konkretno:

* Prostor za pohranu i Dohvaćanje informacija generički aplikacije na klastere HDInsight je omogućen verzijom 3.1.1.374 ili noviji.
* Komponenta informacije specifične za framework aplikacije poslužitelja za vremenske trake nije dostupan na klastere HDInsight.


Generički informacije o aplikacijama obuhvaća sljedeće vrste podataka:

* ID aplikacije, Jedinstveni identifikator aplikacije
* Korisnik koji je pokrenuo aplikacije
* Informacije o pokušao isporučiti da biste dovršili aplikacije
* Spremnici koristi bilo koji pokušaj određene aplikacije

Na vašem klastere HDInsight ti podaci bit će spremljeni Azure resursa Upravitelj spremište povijest u spremnik zadano zadani račun za Azure prostora za pohranu. Ovaj generički podatke na dovršene aplikacije mogu se dohvaćati putem REST API-JA:

    GET on https://<cluster-dns-name>.azurehdinsight.net/ws/v1/applicationhistory/apps


## <a name="yarn-applications-and-logs"></a>Zapisnici programa YARN i

YARN podržava više programiranje modelima (MapReduce se jedna od njih) tako da decoupling Upravljanje resursima iz aplikacije planiranje i praćenje. To možete učiniti putem globalni *ResourceManager* (upravitelja Resursa), po – tempiranja-čvor *NodeManagers* (NMs) i po aplikacije *ApplicationMasters* (AMs). Prijepodne po aplikacije pregovara resursa (CPU-a, memorije, disk, mreže) za pokretanje aplikacije s na RM. Upravitelj Resursa funkcionira s NMs da biste dodijelili tih resursa koje odobravaju kao *spremnike*. Na AM je zadužen za praćenje tijeka spremnika dodijelila ga na RM. Aplikacije sustava može zahtijevati više spremnika ovisno o prirode aplikacije.

Osim toga, svaku aplikaciju možda se sastoje od više *aplikacija pokušava* da biste dovršili x. ruši ili zbog gubitak komunikaciju između programa Prijepodne i programa RM. Dakle, spremnika odobravaju određene pokušaj aplikacije. U smislu spremnik omogućuje kontekst za osnovna jedinica ono što ste napravili YARN aplikacija, a sve posla koji obavlja u kontekstu spremnik izvodi na čvor jednog suradnika na kojem je spremnik dodijeliti. Potražite u članku [YARN koncepata] [ YARN-concepts] za daljnje referencu.

Zapisnici aplikacije (i zapisnike pridružene spremnik) su ključnim za ispravljanje pogrešaka problematična Hadoop aplikacije. YARN daje bolje okvir za prikupljanje, Zbrajanje i spremanje zapisnika aplikacije pomoću [Prikupljanja zapisnika] [ log-aggregation] značajku. Značajka prikupljanja zapisnika omogućuje pristupanju zapisnika aplikacije više deterministic objedinjuje zapisnika preko svih spremnika na čvor tempiranja i sprema ih kao jedan Zbrojeno datoteku zapisnika po tempiranja čvor na zadani datotečni sustav završetku aplikacije. Aplikacije mogu koristiti stotine ili tisuće spremnika, ali zapisnicima sve spremnike izvoditi na jednom tempiranja čvor se uvijek pridružuje u jednu datoteku, rezultira jedan zapisnik po tempiranja čvor koristi aplikacija. Prikupljanja zapisnika je po zadanom omogućena na HDInsight klastere (verzije 3.0 i noviji), a Zbrojeno zapisnike moguće je pronaći u zadanom spremnik svoj klaster na sljedećem mjestu:

    wasbs:///app-logs/<user>/logs/<applicationId>

U tom mjesto, *korisničko* ime korisnika koja je pokrenula aplikacija, a *applicationId* Jedinstveni identifikator programa kao što je dodijelio YARN RM.

Zapisnike Zbrojeno nisu izravno čitljiv, kao što su zapisuju u [TFile][T-file], [binarnom obliku] [ binary-format] indeksirati kontejner. YARN nudi EŽA alate za ispis ti zapisnici u obliku običnog teksta za aplikacije ili spremnika koji vas zanimaju. Ti zapisnici možete prikazati kao običan tekst tako da pokrenete jednu od sljedećih YARN naredbe izravno na čvorove klaster (nakon povezivanja putem RDP):

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>


## <a name="yarn-resourcemanager-ui"></a>Korisničko Sučelje ResourceManager YARN

Korisničko Sučelje ResourceManager YARN pokreće na headnode klaster, a možete pristupiti putem Azure portala nadzorne ploče: 

1. Prijavite se na [portal za Azure](https://portal.azure.com/). 
2. Na izborniku lijevo kliknite **Pregledaj**, kliknite **HDInsight klastere**, zatim utemeljen na sustavu Windows klaster koju želite pristupiti zapisnike YARN aplikacije.
3. Na izborniku gornji kliknite **nadzorna ploča**. Prikazat će se stranica otvoriti u novom pregledniku kartice pod nazivom **HDInsight konzole za upit**.
4. **HDInsight konzole za upit**, kliknite **Yarn korisničkog Sučelja**.




[YARN-timeline-server]:http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html
[log-aggregation]:http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/
[T-file]:https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf
[binary-format]:https://issues.apache.org/jira/browse/HADOOP-3315
[YARN-concepts]:http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/
