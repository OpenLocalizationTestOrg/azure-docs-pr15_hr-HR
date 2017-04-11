<properties 
    pageTitle="Stvaranje aplikacije servisa za okruženja" 
    description="Opis tijek stvaranja okruženja aplikacije servisa" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>Stvaranje aplikacije servisa za okruženja #

### <a name="overview"></a>Pregled ###

Aplikacije servisa za okruženja elika (i mala slova) su Premium servis servisa Azure aplikacije koje nudi poboljšane konfiguracije mogućnost povezivanja koja nije dostupna u žigovima više klijenta.  Značajka elika i mala slova zapravo uvodi aplikacije servisa Azure u klijentu virtualne mreže.  Da bi se dobio veći razumijevanja mogućnosti koje nudi aplikacije servisa okruženja Saznajte [što je okruženju aplikacije servisa] [ WhatisASE] dokumentacije.

### <a name="before-you-create-your-ase"></a>Prije stvaranja vaše elika i mala slova ###

Nije važno je znati što ne možete promijeniti.  Aspekti ne možete promijeniti o vašem elika i mala slova nakon stvaranja su:

- Mjesto
- Pretplate
- Grupa resursa
- VNet koristi
- Podmreže koristi 
- Veličina podmreže

Prilikom odabira u VNet i navođenjem podmreži, provjerite je li je dovoljna za sve budućeg rasta.  

### <a name="creating-an-app-service-environment"></a>Stvaranje aplikacije servisa za okruženja ###

Da biste pristupili elika i mala slova stvaranje korisničkog Sučelja na dva načina.  Možda ćete pronaći pretraživanjem trgovine Windows Azure ***aplikacije servisa*** okruženju ili putem New -> Web + Mobile -> aplikacije servisa okruženju.  Da biste stvorili programa elika i mala slova:

1. Navedite naziv vaše elika i mala slova.  Naziv koji je naveden za elika i u mala slova će se koristiti za aplikacije stvorene u elika i u mala slova.  Ako je naziv u elika i mala slova appsvcenvdemo biti bi poddomene naziv. *appsvcenvdemo.p.azurewebsites.net*.  Ako tako stvoreni aplikacije s nazivom *mytestapp* , tada će biti moguće adresirati na *mytestapp.appsvcenvdemo.p.azurewebsites.net*.  Ne možete koristiti praznina naziv vaše elika i mala slova.  Ako koristite pisani velikim slovima znakove u nazivu, naziv domene bit će ukupni mala verzija taj naziv.  Ako koristite programa ILB zatim naziv elika i mala slova se koristi u vašem poddomenu, ali umjesto izričito navedeno tijekom stvaranja elika i mala slova

    ![][1]

2. Odaberite pretplatu.  Pretplate za vaše elika i mala slova je onaj koji će biti stvoren sve aplikacije u tom elika i mala slova.  Ne možete smjestiti vaše elika i mala slova u VNet koji se nalazi u drugu pretplatu

3. Odaberite ili navedite novu grupu resursa.  Grupa resursa koji se koriste za vaše elika i mala slova mora biti isti koji se koristi za vaše VNet.  Ako odaberete postojećih VNet Grupiraj odabir resursa za vaše elika i mala slova će ažurirati u skladu s vizualnim koji vaše VNet.

    ![][2]

4. Odaberite željeno virtualne mreže i mjesto.  Možete odabrati da biste stvorili novi VNet ili odaberite postojećih VNet.  Ako odaberete novi VNet možete navesti naziv i mjesto. Novi VNet će imati raspon 192.268.250.0/23 adresa i podmreže pod nazivom **zadani** koja je definirana kao 192.168.250.0/24.  Možete odabrati i jednostavno postojećih Classic ili VNet upravitelj resursa.  Odabir vrste VIP određuje ako vaš elika i mala slova možete izravno pristupiti putem Interneta (vanjsko) ili koristi se raspoređivača opterećenja Interna (ILB).  Da biste saznali više o njima pročitajte [pomoću internog raspoređivača opterećenja s okruženjem aplikacije servisa][ILBASE].  Ako odaberete VIP vrste vanjskih možete odabrati koliko vanjske IP adrese sustava stvara se pomoću svrhe IPSSL.  Ako odaberete internog morate navesti poddomena koju ćete koristiti na elika i mala slova.  ASEs može uvesti u virtualne mreže koje koriste *neki* adresa javnog raspona, *ili* RFC1918 adresa razmaka (odnosno Privatna adresa).  Da biste koristili virtualne mreže s rasponom javnu adresu, morat ćete stvoriti VNet na vrijeme.  Kad odaberete postojećih VNet morat ćete stvoriti novi podmreže tijekom stvaranja elika i mala slova.  **Unaprijed stvoreni podmreže ne možete koristiti na portalu.  Ako stvarate vaše elika i mala slova pomoću predloška za upravitelj resursa možete stvoriti na elika i mala slova s postojećih podmreže.**  Da biste stvorili programa elika i mala slova iz predloška korištenja informacije ovdje, [Stvaranje okruženju servisa aplikacije iz predloška] [ ILBAseTemplate] , a Saznajte i [Stvaranje okruženju servisa ILB aplikacije iz predloška][ASEfromTemplate].

### <a name="details"></a>Pojedinosti ###

Na elika i mala slova stvara se pomoću 2 sučeljima i zaposlenici zaduženi za 2.  U sučeljima poslužiti kao krajnje točke HTTP/HTTPS web-mjesta i pošaljite promet zaposlenici zaduženi za koje su uloge koje hostira aplikacija.   Možete prilagoditi količinu nakon stvaranja elika i mala slova, a možete čak i postavljanje pravila za automatsko skaliranje na te grupe resursa.  Više pojedinosti oko ručne promjene veličine, upravljanje i nadzor od servisa okruženju aplikacije izvor informacija: [kako konfigurirati okruženje servisa aplikacija][ASEConfig] 

Samo na jednom elika i mala slova može postojati u podmreže koji se koriste u elika i mala slova.  Podmreži ne može se koristiti za sve osim na elika i mala slova

### <a name="after-app-service-environment-creation"></a>Nakon stvaranja okruženja aplikacije servisa ###

Nakon stvaranja elika i mala slova možete prilagoditi:

- Količina sučeljima (minimalne: 2)
- Količina zaposlenici zaduženi za (minimalne: 2)
- Količina IP adresa za IP SSL
- Veličina resursa koristi sučeljima ili zaposlenici zaduženi za izračun (sučeljima Minimalna veličina je P2)


Nema više detalja oko ručno i promjene veličine, upravljanje i nadzor aplikacija servisa okruženja ovdje: [kako konfigurirati okruženje servisa aplikacija][ASEConfig] 

Dodatne informacije o autoscaling je vodič za ovdje: [kako konfigurirati Automatsko skaliranje za servis okruženju aplikacije][ASEAutoscale]

Postoje dodatni ovisnosti koje nisu dostupne za prilagodbu kao što su baze podataka i prostora za pohranu.  Te su obrađuje Azure i isporučuju uz sustav.  Prostor za pohranu sustava podržava do 500 GB za cijelo okruženje aplikacije servisa i baze podataka prilagoditi tako da Azure prema potrebi po skale sustava.


## <a name="getting-started"></a>Početak rada
Svih članaka i kako – da biste je za aplikaciju servisa okruženja dostupne u [PROČITAJME za aplikaciju servisa okruženja](../app-service/app-service-app-service-environments-readme.md).

Za početak rada s aplikaciju servisa okruženja, potražite u članku [Uvod u aplikaciju servisa okruženja][WhatisASE]

Dodatne informacije o platforme Azure aplikacije servisa potražite u članku [Aplikacije servisa za Azure][AzureAppService].

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
