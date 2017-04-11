<properties 
    pageTitle="Upravljanje web-aplikacijama u aplikacije servisa za Azure" 
    description="Veze na resurse za upravljanje web-aplikacijama u servisu Azure aplikacije." 
    services="app-service\web" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/24/2016" 
    ms.author="rachelap"/>

# <a name="manage-a-web-app-in-azure-app-service"></a>Upravljanje web-aplikacijama u aplikacije servisa za Azure

Ova tema sadrži veze na resurse za upravljanje web-aplikacijama u [Servisu Azure aplikacije](http://go.microsoft.com/fwlink/?LinkId=529714). Upravljanje obuhvaća sve zadatke koje su web-aplikaciju programa nesmetan. 

Tijekom života web-aplikacijama, će izvesti zadatke upravljanja različite pomičete s početnog uvođenja normalno, održavanje i ažuriranja.

Na portalu za Azure može izvršiti mnoge zadatke upravljanja aplikaciju za web.

## <a name="before-you-deploy-your-web-app-to-production"></a>Prije implementacije web-aplikaciju programa proizvodnje

### <a name="choose-a-tier"></a>Odaberite sloj

Aplikacije servisa za Azure se nude na pet razine: slobodno, zajedničko korištenje, Basic, standardna i Premium. Informacije o značajkama i cijene za svaki sloju, potražite u članku [cijene pojedinosti](/pricing/details/app-service/). 

- [Aplikacije servisa za tarife](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) omogućuju grupiranje više web-aplikacije u odjeljku isti sloju.
- Uvijek možete [prebaciti razine](web-sites-scale.md) nakon stvaranja web-aplikaciju programa.

### <a name="configuration"></a>Konfiguracija

Pomoću [Portala za Azure](https://portal.azure.com/) možete postaviti razne mogućnosti konfiguracije. Detalje potražite u članku [Konfiguriranje web-aplikacije u servisu Azure aplikacije](web-sites-configure.md). Evo brzog kontrolnog popisa:

- Odaberite **vrijeme izvođenja verzije** za .NET, PHP, Java ili Python, ako je potrebno.
- Omogućivanje **WebSockets** ako web-aplikaciju programa koristi protokol WebSocket. (To uključuje aplikacije koje koriste [ASP.NET SignalR](http://www.asp.net/signalr) ili [socket.io](web-sites-nodejs-chat-app-socketio.md).)
- Koristite neprekinuti web zadacima? Ako je tako, omogućiti **Uvijek na**.
- Postavljanje **zadanog dokumenta**, kao što su index.html.

Osim Osnovna konfiguracija postavki, preporučujemo vam da biste konfigurirali sljedeće:

- Šifriranje **Secure Socket Layer (SSL)** . Da biste koristili SSL s prilagođenim nazivom domene, morate dobiti SSL certifikata i konfiguriranje web-aplikaciju u programa za potvrdu da biste ga koristiti. Potražite u članku [Omogućivanje HTTPS za web-aplikaciju u aplikacije servisa za Azure](web-sites-configure-ssl-certificate.md).
- **Prilagođeni naziv domene.** Web-aplikaciju programa automatski dobiva poddomene u odjeljku azurewebsites.net. Možete povezati prilagođeni naziv domene, primjerice contoso.com. Potražite u članku [Konfiguriranje prilagođenog naziva domene na servisu Azure aplikacije](web-sites-custom-domain-name.md).

Konfiguracija jezično specifične:

- **PHP**: [Konfiguriranje i u web-aplikacijama aplikacije servisa za Azure](web-sites-php-configure.md).
- **Python**: [Konfiguriranje Python u aplikacije servisa za Azure Web Apps](web-sites-python-configure.md)


## <a name="while-your-web-app-is-running"></a>Dok se izvodi web-aplikaciju programa

Dok se izvodi web-aplikaciju programa, želite provjerite je li dostupna je i da mijenja veličinu da bi odgovarao promet korisnika. Možda ćete morati otklanjanje pogrešaka.

### <a name="monitoring"></a>Nadzor

- Putem portala za Azure možete [dodati performanse mjernih podataka](web-sites-monitor.md) kao što su procesora i broj zahtjeva za klijenta.
- [Promjena veličine web-aplikaciju programa](web-sites-scale.md) kao odgovor na promet. Ovisno o vašem sloju skalirali broj VMs i/ili veličina VM instanci. U razine standardnu i Premium možete i postaviti autoscaling, tako da se web-aplikaciju programa mijenja veličinu automatski fixed rasporedu ili kao odgovor na učitati.  
 
### <a name="backups"></a>Sigurnosne kopije

- Postaviti [Automatsko sigurnosno kopiranje](web-sites-backup.md) web-aplikacije. Saznajte više o sigurnosnih kopija u [ovom ćete videozapisu](https://azure.microsoft.com/documentation/videos/azure-websites-automatic-and-easy-backup/).
- Dodatne informacije o mogućnostima za [oporavak baze podataka](../sql-database/sql-database-business-continuity.md) u bazi podataka SQL Azure.

### <a name="troubleshooting"></a>Otklanjanje poteškoća

- Ako nešto pošlo po redu, možete ga [Otklanjanje poteškoća s u Visual Studio](web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug), pomoću dijagnostičkog zapisnika i ispravljanje pogrešaka uživo u oblaku. 
- Izvan Visual Studio postoje razni načini prikupljanje dijagnostičke zapisnike. Potražite u članku [Omogućivanje Dijagnostika bilježenje u zapisnik web-aplikacijama u servisu Azure aplikacije](web-sites-enable-diagnostic-log.md).
- Node.js aplikacije, potražite [u](web-sites-nodejs-debug.md)članku Node.js web app u Azure aplikacije servisa za ispravljanje pogrešaka.

### <a name="restoring-data"></a>Vraćanje podataka

- [Vraćanje](web-sites-restore.md) web-aplikacije koje se prethodno sigurnosne kopije.


## <a name="when-you-update-your-web-app"></a>Kada ažurirate web-aplikaciju programa

Ako niste omogućili automatsko sigurnosno kopiranje, možete stvoriti [ručno sigurnosno kopiranje](web-sites-backup.md).

Razmislite o korištenju [kopirana bez postavljanja implementacije](web-sites-staged-publishing.md). Ta mogućnost omogućuje objavljivanje ažuriranja pripremna implementaciju koja se pokreće jedno uz drugo u svojoj implementaciji proizvodnje. 

Ako koristite Visual Studio Team Services, možete postaviti neprekinuti implementacije iz izvora kontrole:

- [Korištenje timskog Foundation verzije kontrole (TFVC)](../cloud-services/cloud-services-continuous-delivery-use-vso.md) 
- [Korištenje brojka](../cloud-services/cloud-services-continuous-delivery-use-vso-git.md)
 
<!-- Anchors. -->

[Before you deploy your site to production]: #before-you-deploy-your-site-to-production
[While your website is running]: #while-your-website-is-running
[When you update your website]: #when-you-update-your-website

  
