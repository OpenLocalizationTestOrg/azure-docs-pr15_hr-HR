<properties
 pageTitle="Razumijevanje i otklanjanje pogrešaka WebHCat na HDInsight"
 description="Saznajte kako da biste o uobičajenih pogrešaka vratio WebHCat na HDInsight te kako biste ih riješili."
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"
 tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="09/27/2016"
 ms.author="larryfr"/>

#<a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Razumijevanje i rješavanju pogrešaka koje ste primili iz WebHCat (Templeton), na HDInsight

Prilikom korištenja WebHCat (prijašnjeg Templeton,) da biste radili s HDInsight, možda ćete primiti pogreške. Ovaj dokument sadrži smjernice na uobičajene pogreške – Zašto nastaju i što možete učiniti da biste ih riješili.

##<a name="what-is-webhcat"></a>Što je WebHCat?

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) je REST API-JA za [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tablice i pohranu sloju upravljanje Hadoop. WebHCat je po zadanom omogućena na HDInsight klastere, a koriste razne alate za poslati zadacima, bez prijave u klaster dobiti stanja zadatka, itd..

##<a name="modifying-configuration"></a>Izmjena konfiguracija

> [AZURE.IMPORTANT] Nekoliko pogreške navedene u ovom dokumentu pojaviti jer je premašena konfigurirani maksimum. Kada korak razlučivost spominjanja izmjene vrijednosti, poslužite se nešto od sljedećeg da biste izvršili promjene:

* Za klastere **Windows** : akcijom skriptu da biste konfigurirali vrijednost tijekom stvaranja klaster. Dodatne informacije potražite u članku [razvoju skripte akcije](hdinsight-hadoop-script-actions.md).

* Za klastere **Linux** : korištenje Ambari (web ili REST API-JA) da biste izmijenili vrijednost. Dodatne informacije potražite u članku [Upravljanje HDInsight pomoću Ambari](hdinsight-hadoop-manage-ambari.md)

###<a name="default-configuration"></a>Konfiguriranje zadanog

Slijede zadane konfiguracije vrijednosti koje se mogu utjecati na performanse WebHCat ili uzrokuju pogreške ako se premaše ove vrijednosti:

| Postavka | Funkcija | Zadana vrijednost |
| ------- | ------------ | ------------- |
| [yarn.Scheduler.capacity.Maximum aplikacije][maximum-applications] | Maksimalni broj zadataka koji se može biti aktivna istovremeno (na čekanju ili ne radi) | 10 000 |
| [templeton.Exec.Max procs][max-procs] | Maksimalan broj zahtjeva koja se istovremeno služila | 20 |
| [mapreduce.jobhistory.Max dob ms][max-age-ms] | Broj dana koji će biti zadržani povijesti zadatka | sedam dana |

##<a name="too-many-requests"></a>Previše zahtjeva

**Šifra stanja HTTP**: 429

| Uzrok | Razlučivost |
| ----- | ---------- |
| Ako ste premašili maksimalni istovremeni zahtjevi poslužena po WebHCat minuti (zadano 20) | Svoje radno opterećenje da biste bili sigurni da ne šaljete na više maksimalni broj istovremeni zahtjevi povećali ili smanjili ograničenja Istodobni zahtjev za promjenom `templeton.exec.max-procs`. Dodatne informacije potražite u članku [Konfiguracija Modifying](#modifying-configuration) |

##<a name="server-unavailable"></a>Poslužitelj nije dostupan

**Šifra stanja HTTP**: 503

| Uzrok | Razlučivost |
| ---------------- | ------------------- |
| To se obično događa tijekom prebacivanje između primarnih i sekundarnih HeadNode za klaster | Pričekajte dvije minute, a zatim ponovite postupak |

##<a name="bad-request-content-could-not-find-job"></a>Neispravan zahtjev sadržaja: nije moguće pronaći posla

**Šifra stanja HTTP**: 400

| Uzrok | Razlučivost |
| ---------------- | ------------------- |
| Pojedinosti posla nisu očistili po dosadašnje iskustvo program za čišćenje | Zadano razdoblje zadržavanja za dosadašnje iskustvo je sedam dana. To možete promijeniti promjenom `mapreduce.jobhistory.max-age-ms`. Dodatne informacije potražite u članku [Konfiguracija Modifying](#modifying-configuration) |
| Zadatak je omogućeno drugom procesu zbog s pomoćnim | Ponovite predavanje posla za do dvije minute |
| Id zadatka koji nisu valjani koristila | Provjera je li točan id zadatka |

##<a name="bad-gateway"></a>Neispravni pristupnika

**Šifra stanja HTTP**: 502

| Uzrok | Razlučivost |
| ---------------- | ------------------- |
| Interna smeća se pojavljuje unutar WebHCat procesa | Pričekajte smeća dovrši ili ponovno pokrenite servis za WebHCat |
| Prekoračenje vremena čeka se odgovor ResourceManager servisa. To se može dogoditi kada broj aktivne aplikacije dolazi konfigurirani najviše (zadano 10 000) | Pričekajte da se trenutno izvode zadatke za dovršetak ili povećajte ograničenje Istodobni posao izmjenom `yarn.scheduler.capacity.maximum-applications`. Dodatne informacije potražite u članku [Konfiguracija Modifying](#modifying-configuration)  |
| Prilikom pokušaja dohvatiti sve zadatke putem poziva [se /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) tijekom `Fields` postavljen na`*` | Dohvaćanje detalja *sve* posla, umjesto toga koristite `jobid` dohvaćanje detalja za poslove samo veća od određene id zadatka. Ili pomoću`Fields` |
| Servis WebHCat isključen tijekom HeadNode prebacivanje | Pričekajte dvije minute, a zatim ponovite postupak |
| Nema više od 500 na čekanju poslove šalje putem WebHCat | Pričekajte da se trenutno čeka poslove dovršite prije slanja više zadataka |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
 
