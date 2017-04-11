<properties 
    pageTitle="Pomoću konfiguracije PM2 za NodeJS u web-aplikacijama na Linux | Microsoft Azure" 
    description="Pomoću konfiguracije PM2 za NodeJS u web-aplikacijama na Linux" 
    keywords="Azure aplikacije servisa za web-aplikacije, nodejs, pm2, linux, oss"
    services="app-service" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/10/2016" 
    ms.author="naziml"/>

# <a name="using-pm2-configuration-for-nodejs-in-web-apps-on-linux"></a>Pomoću konfiguracije PM2 za Node.js u web-aplikacijama na Linux

Ako ste postavili stog aplikacije da biste Node.js web-aplikacijama na Linux, dobit ćete mogućnost da biste postavili Node.js pokretanje datoteke kao što je prikazano na slici u nastavku.

![][1]

To možete koristiti da biste

-   Odredite skriptu za pokretanje aplikacije Node.js (na primjer: /bin/server.js)
-   Odredite PM2 konfiguracijska datoteka da biste koristili aplikaciju Node.js (na primjer: /foo/process.json)

 >[AZURE.NOTE] Ako želite procese čvor automatski ponovno pokrenuti kada se mijenjaju određene datoteke, morat ćete PM2 konfiguracija. U suprotnom aplikacija ne će pokrenuti kada ga prima obavijesti o promjeni iz elemente kao što su neprekinuti implementacije promjenama kodu aplikacije.

Možete provjeriti Node.js [postupak datoteke dokumentaciju](http://pm2.keymetrics.io/docs/usage/application-declaration/) za sve mogućnosti, ali u nastavku je uzorak što želite koristiti kao process.json datoteka

        {
          "name"        : "worker",
          "script"      : "/bin/server.js",
          "instances"   : 1,
          "merge_logs"  : true,
          "log_date_format" : "YYYY-MM-DD HH:mm Z",
          "watch": ["/bin/server.js", "foo.txt"],
          "watch_options": {
            "followSymlinks": true,
            "usePolling"   : true,
            "interval"    : 5
          }
        }

Imajte na umu u ovoj konfiguraciji važnih stvari koje su 

-   Svojstvo "skripte" određuje skriptu aplikaciju sustava start.
-   Svojstvo "instance" određuje koliko je instanci čvor postupak da biste pokrenuli. Ako koristite aplikacije na veće VM koji imaju više jezgri želite Maksimiziranje resurse tako da Ovdje postavite veću vrijednost.
-   Polje "pogledajte" navodi sve datoteke čije promjene želite ponovno pokrenuti procese čvor.
-   "Watch_options," trenutno potrebno da biste odredili "usePolling" kao true zbog način na koji je postavljen vaš sadržaj aplikacije.


## <a name="next-steps"></a>Daljnji koraci ##

* [Što je aplikacije servisa za na Linux?](./app-service-linux-intro.md)

<!--Image references-->
[1]: ./media/app-service-linux-using-nodejs-pm2/nodejs-startup-file.png