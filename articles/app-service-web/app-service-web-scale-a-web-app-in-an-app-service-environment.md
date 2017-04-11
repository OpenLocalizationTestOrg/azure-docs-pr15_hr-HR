<properties 
    pageTitle="Upute za promjenu veličine aplikacije u okruženju aplikacije servisa" 
    description="Promjena veličine aplikacije u okruženju aplikacije servisa" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>Promjena veličine aplikacije u okruženju aplikacije servisa #

U aplikaciji servisa Azure normalno tri stvari mogu mijenjati veličinu:

- cijene plan
- Veličina tempiranja 
- Broj instanci.

Nema potrebe da biste odabrali ili promijenili tarifu za cijene u programa elika i mala slova.  Pomoću mogućnosti je već na Premium cijene mogućnošću pretraživanja kroz razine.  

Vezana uz veličine tempiranja, administrator elika i mala slova možete dodijeliti veličinu računalnim resursa koje će se koristiti za svaki skup tempiranja.  To znači da imate radnih grupa aplikacija 1 s P4 računalnim resursima i radnih grupa aplikacija 2 s P1 izračun resursa, po želji.  Ne moraju biti ispravno veličina.  Za podatke o oko veličine i njihovih cijene potražite u članku u dokumentu [Cijene za Azure aplikacije servisa][AppServicePricing].  To ostavlja mogućnosti skaliranja za web-aplikacije i tarife servisa aplikacija u okruženju servisa aplikacija biti:

- Odabir radnih grupa aplikacija
- Broj instanci

Promjena ili stavku obavlja putem prikazanih za vaše elika i mala slova hostirane aplikacije servisa tarife odgovarajuće korisničkom Sučelju.  

![][1]

Nije moguće proširenja vaše ASP izvan broj dostupnih računalnim resursa u tempiranja koji se vaše ASP.  Ako je potrebno izračunati resursa u tu grupu radnih morate dobiti administrator elika i mala slova da biste ih dodali.  Za informacije oko ponovno konfiguriranje vaše elika i mala slova pročitajte informacije ovdje: [kako konfigurirati okruženje aplikacije servisa za][HowtoConfigureASE].  Možda želite iskoristiti značajke da biste dodali kapacitet temelju raspored ili metriku za automatsko skaliranje elika i mala slova.  Da biste dobili dodatne informacije o konfiguriranju automatsko skaliranje za elika i u mala slova okruženje sam potražite u članku [kako konfigurirati Automatsko skaliranje za servis okruženju aplikacije][ASEAutoscale].

Možete stvoriti aplikaciju za više servisa tarife pomoću računalnim resursa iz različitih radnih grupe ili možete koristiti isti skup tempiranja.  Ako, primjerice imate (10) dostupne računalnim resursa u radnih grupa aplikacija 1, možete odabrati da biste stvorili aplikacije servisa tarifa koristi (6) računalnim resurse i planiranje druge aplikacije servisa koji koristi (4) za izračun resursi.

### <a name="scaling-the-number-of-instances"></a>Promjena veličine broj instanci ###

Prilikom prvog stvaranja web-aplikaciju programa u okruženju aplikacije servisa započinje s 1 instance.  Možete se zatim Promijeni odgovor na dodatne instance nudi računalnim dodatne resurse za aplikacije.   

Ako je vaš elika i mala slova dovoljno kapaciteta, a zatim je vrlo jednostavne.  Idite na aplikaciju servisa planiranje koja sadrži web-mjesta želite proširenja, a zatim odaberite mjerilo.  Otvorit će se korisničko Sučelje gdje možete ručno postaviti skalu za vaše ASP ili konfigurirajte automatsko skaliranje pravila za vaše ASP.  Da biste ručno skaliranje aplikacije jednostavno postavite ***Skaliranje tako da*** ***se***broje instanci koji mogu Ručni unos.  Na tom mjestu povucite klizač na željenu količinu ili unesite u okvir pokraj klizača.  

![][2] 

Automatsko skaliranje pravila za programa ASP u programa elika i mala slova funkcionira na isti način kao i obično.  Možete odabrati ***Postotak procesora*** u odjeljku ***Skaliranje po*** i stvaranje pravila za automatsko skaliranje za vaše ASP na temelju postotka procesora ili možete stvoriti složenije pravila pomoću ***pravila raspored i performanse***.  Da biste vidjeli potpuniji detalje o konfiguriranju automatsko skaliranje koristiti vodič ovdje [Skaliranje aplikaciju u aplikacije servisa za Azure][AppScale]. 


### <a name="worker-pool-selection"></a>Odabir radnih grupa aplikacija ###

Kao što je naznačeno ranije, odabira radnih grupa aplikacija je pristupiti s ASP korisničkog Sučelja.  Otvorite na plohu ASP koju želite mjerilo, a zatim odaberite skup tempiranja.  Vidjet ćete sve tempiranja grupe koje ste konfigurirali u svom okruženju aplikacije servisa.  Ako imate samo jedan radni skup pa ćete vidjeti samo jednu grupu na popisu.  Da biste promijenili koje se vaše ASP grupe aplikacija radnih, jednostavno odaberite skup tempiranja želite da vaše aplikacije servisa Plan da biste premjestili.  

![][3]

Prije prelaska na ASP iz jednog radnih grupa aplikacija na drugi je važno da biste bili sigurni da imate odgovarajuće kapacitet za vaše ASP.  Na popisu grupe radnih ne samo nalazi naziv grupe aplikacija tempiranja, ali možete vidjeti i zaposlenici zaduženi za koliko su dostupne u tom tempiranja.  Provjerite da nema dovoljno instanci koje su dostupne za planiranje aplikacije servisa.  Ako je potrebno više izračunati resursa u tempiranja grupu koju želite premjestiti, zatražite administratora elika i mala slova da biste ih dodali.  

> [AZURE.NOTE] Premještanje na ASP iz jednog radnih grupa aplikacija uzrokovat će Hladno pokreće se aplikacija u tom ASP.  To može uzrokovati zahtjevi za pokretanje sporo kao što je aplikacija Hladna rada na nove računalnim resurse.  Hladna početka mogu se izbjegavati pomoću [aplikacije Toplo gore mogućnost] [ AppWarmup] u servisu Azure aplikacije.  Modul za pokretanje aplikacije opisane u članku funkcionira i za Hladna pokreće jer je postupak za inicijalizaciju se poziva kada su aplikacije Hladna rada na nove računalnim resurse. 

## <a name="getting-started"></a>Početak rada

Da biste započeli aplikacije servisa okruženja, potražite u članku [Kako da biste stvorili programa aplikacije servisa okruženje][HowtoCreateASE]

Dodatne informacije o platforme Azure aplikacije servisa potražite u članku [Aplikacije servisa za Azure][AzureAppService].

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
