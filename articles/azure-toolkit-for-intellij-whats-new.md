<properties
    pageTitle="Što je novo u Azure komplet alata za IntelliJ | Microsoft Azure"
    description="Informirajte se o najnovijim značajkama u komplet alata za Azure za IntelliJ."
    services=""
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="multiple"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/26/2016" 
    ms.author="robmcm;asirveda;martinsawicki"/>

# <a name="whats-new-in-the-azure-toolkit-for-intellij"></a>Što je novo u Azure komplet alata za IntelliJ

## <a name="azure-toolkit-for-intellij-releases"></a>Azure komplet alata za IntelliJ izdanja

Ovaj članak sadrži informacije o raznim izdanja i najnovija ažuriranja za komplet alata za Azure za IntelliJ.

> [AZURE.NOTE] Postoji Azure komplet alata za za Eclipse IDE. Dodatne informacije potražite u članku [Azure komplet alata za Eclipse].

### <a name="august-26-2016"></a>Kolovoz 26, 2016

Komplet alata za Azure za IntelliJ - 2016 kolovoz izdanje obuhvaća sljedeća poboljšanja:

* **Prilagođeni JDK distribucije**. Komplet alata za Azure za IntelliJ sada podržava određivanje i implementaciji sustava proizvoljne JDK verzije sustava spremniku Azure WebApp:
  - Osim JDKs nudi Azure, možete odabrati iz široke odabira verzija Zulu OpenJDK učinio dostupnima na Azure Azul sustave.
  - Možete odrediti i vlastite JDK raspodjelu Ako prenesete ga kao ZIP datoteke na račun servisa za pohranu.
* **Poboljšanja prikaz pretraživača Azure**:
  - Podrška za upravljanje virtualnog računala pomoću novog modela resursima Azure korisnika: popis, stvaranje i na brisanje resursa utemeljen na Upravitelj virtualnim strojevima bez pomicanja na IDE.
  - Podrška za računa spremišta blobova platforme upravljanja pomoću upravitelja resursa Azure korisnika koji dopuna postojeće funkcionalnost za upravljanje računima "klasični" prostora za pohranu.
* **Upravljački program za Microsoft JDBC 6.0 za SQL Server**. To ažuriranje sadrži najnoviji JDBC upravljački program za Microsoft SQL Server (v6.0), što je sada uključeni kao biblioteku u kojoj možete jednostavno dodati Java projekte, čime ćete zamjene starije verzije.

### <a name="june-29-2016"></a>Lipnja 29, 2016

Komplet alata za Azure za IntelliJ - 2016 lipnja izdanje obuhvaća sljedeća poboljšanja:

* **Preduvjet Java 8**. Komplet alata za Azure za IntelliJ sada zahtijeva Java 8, iako se taj zahtjev je samo za kompleta alata za - aplikacija možete nastaviti koristiti sve verzije Java koje podržava Azure.
* **Podrška za najnovije Java JDKs**. Najnoviju verziju sustava Java JDKs sada podržava komplet alata za Azure za IntelliJ.
* **Podrška za Azure SDK v2.9.1**. Najnoviju verziju Azure SDK sada je minimalne stara obavezna za Azure alata za IntelliJ.
* **Integrirano uzorka**. Komplet alata za Azure za IntelliJ sada nudi nekoliko oglednih aplikacija da razvojnim inženjerima početak rada.
* **Integracija alat za HDInsight**. Alati na Azure HDInsight su sada vezanoj instalaciji s komplet alata za Azure za IntelliJ. Dodatne informacije potražite u članku [HDInsight dodatak Alati za IntelliJ].
* **Udaljena ispravljanje pogrešaka web-aplikacija Java Web Apps**. Komplet alata za Azure za IntelliJ sada podržava udaljene pogrešaka Java web apps na aplikacije servisa za Azure.

### <a name="april-12-2016"></a>Travanj 12, 2016

Komplet alata za Azure za IntelliJ - 2016 u travnju izdanje obuhvaća sljedeća poboljšanja:

* **Podrška za Azure SDK v2.9.0**. Najnoviju verziju Azure SDK sada je minimalne stara obavezna za Azure alata za IntelliJ.
* **Različiti upotrebljivosti, odziv i performanse poboljšanja vezane uz podrške za Azure Web App**. Broj optimizacije performanse u kako komunicira kompleta alata za Azure rezultatom u više odgovara korisničkog Sučelja.
* **Mogućnost brisanja postojeće web-aplikacije spremnika u Azure s unutar IntelliJ**. Komplet alata za Azure za IntelliJ sada omogućuje da biste izbrisali postojeću spremnik za Azure Web ne napuštajući IntelliJ.

## <a name="see-also"></a>Vidi također ##

Dodatne informacije o Kompleti alata Azure za Java IDEs potražite na sljedećim vezama:

- [Azure komplet alata za Eclipse]
  - [Instalacija alata za Azure za Eclipse]
  - [Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse]
  - [Što je novo u Azure komplet alata za Eclipse]
- [Azure komplet alata za IntelliJ]
  - [Instalacija alata za Azure za IntelliJ]
  - [Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ]
  - *Što je novo u Azure komplet alata za IntelliJ (Ovaj članak)*

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java].

<!-- URL List -->

[Azure komplet alata za Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure komplet alata za IntelliJ]: ./azure-toolkit-for-intellij.md
[Stvaranje web-aplikacijama svijeta pozdrav za Azure u Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Stvaranje web-aplikacijama svijeta pozdrav za Azure u IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Instalacija alata za Azure za Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Instalacija alata za Azure za IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Što je novo u Azure komplet alata za Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547

[HDInsight dodatak Alati za IntelliJ]: ./hdinsight/hdinsight-apache-spark-intellij-tool-plugin.md
