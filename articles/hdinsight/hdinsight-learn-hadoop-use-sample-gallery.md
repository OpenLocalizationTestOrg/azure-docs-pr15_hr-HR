<properties
   pageTitle="Naučite Hadoop u HDInsight pomoću galerije uzorka | Microsoft Azure"
   description="Brzo Saznajte Hadoop tako da pokrenete probne aplikacije iz galerije HDInsight početak rada. Korištenje oglednih podataka ili navesti vlastite."
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
   ms.date="10/21/2016"
   ms.author="jgao"/>

# <a name="learn-hadoop-by-using-the-azure-hdinsight-getting-started-gallery"></a>Naučite Hadoop pomoću galerije Azure HDInsight početak rada

Početak rada galerije dostupna je samo za klastere HDInsight utemeljenom na sustavu Windows. Galerije pruža lako i brzo saznati Hadoop tako da pokrenete probne aplikacije u HDInsight. Neke od primjera isporučuje se s oglednim podacima. Možete navesti vlastite podatke za preostale uzorka. Trenutno postoje sljedeće šest uzoraka (s više nadolazeći):

- Rješenja Azure podacima
    - Analiza zapisnika za web-mjesto Microsoft Azure
    - Microsoft Azure prostora za pohranu analytics
- Rješenja s oglednim podacima
    - Senzor analiza podataka
    - Analiza trend twitter
    - Analiza zapisnik web-mjesta
    - Mahout film preporuka

![HDInsight Hadoop, oluja i HBase početak rada galerije rješenja uključujući ogledne podatke.][hdinsight.sample.gallery]

Sljedeće se videozapisu prikazuje kako pokrenuti analizu uzorka trend Twitter:

<center><a href="https://www.youtube.com/embed/7ePbHot1SN4">https://www.Youtube.com/EMBED/7ePbHot1SN4></a></center>

Nadzorna ploča koje se može pristupiti pregledavanjem http://<YourHDInsightClusterName>.azurehdinsight.net/ ili s portala za Azure.

**Da biste pokrenuli uzorka iz galerije početak rada**

1. Prijava na [portal za Azure][azure.portal].
2. Na lijevom izborniku kliknite **Pregledaj** , kliknite **HDInsight klastere**, a zatim naziv klaster.
3. Na gornjoj izborniku kliknite **nadzorna ploča** .
4. Unesite korisničko ime i lozinku za korisnika HTTP (naziva se i klaster korisnika).
6. Kliknite **Početak rada Galerija** pri vrhu stranice.
7. Kliknite jednu od primjera. Svaki uzorak daje detaljne upute za izvođenje ga. Sljedeća slika prikazuje uzorak za analizu Twitter trend:

    ![HDInsight Twitter trend analizu uzorka][hdinsight.twitter.sample]

## <a name="next-steps"></a>Daljnji koraci
Drugi načini dodatne informacije o HDInsight obuhvaćaju sljedeće:

- [HDInsight učenje karte][hdinsight.learn.map]
- [HDInsight infographic][hdinsight.infographic]

<!--Image references-->
[hdinsight.sample.gallery]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Getting-Started-Gallery.png
[hdinsight.twitter.sample]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Twitter-Trend-Analysis-sample.png

<!--Link references-->
[hdinsight.learn.map]: https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/
[hdinsight.infographic]: http://go.microsoft.com/fwlink/?linkid=523960
[azure.portal]:https://portal.azure.com
