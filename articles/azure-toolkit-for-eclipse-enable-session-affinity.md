<properties
    pageTitle="Omogućivanje sesiju afinitet korištenje kompleta alata za Azure za Eclipse"
    description="Saznajte kako omogućiti sesiju afinitet pomoću komplet alata za Azure Eclipse."
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
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->

# <a name="enable-session-affinity"></a>Omogućivanje afinitet sesiju #

Unutar Azure alata za Eclipse, možete omogućiti HTTP sesiju afinitet ili "ljepljive sesije", za svoje uloge. Na sljedećoj je slici prikazan dijaloški okvir koristi da bi omogućio značajku afinitet sesiju svojstva **Opterećenja** :

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Da biste omogućili afinitet sesije za svoje uloge ##

1. Uloga u Eclipse na Project Explorer desnom tipkom miša, kliknite **Azure**, a zatim **Opterećenja**.
1. U dijaloškom okviru **Svojstva WorkerRole1 opterećenja** :
    1. Provjera **omogućiti HTTP sesiju afinitet (ljepljive sesije) za ta uloga.**
    1. Za **krajnje točke unosa za korištenje**, odaberite krajnje točke unosa da biste koristili, na primjer, **http (javno: 80, Privatno: 8080)**. Aplikacija morate koristiti ovaj krajnjoj točki njezin krajnju točku HTTP-a. Možete omogućiti višestruke krajnje točke za vašu ulogu, ali možete odabrati samo jednu od njih da podržava ljepljive sesije.
    1. Ponovno stvaranje aplikacija.

Kada je omogućen, ako imate više od jedne instance uloge, HTTP zahtjeva koji dolaze iz određenog klijenta će i dalje se obrađuje na istoj instanci uloge.

Komplet alata za Eclipse omogućuje to instalacijom modula IIS posebno naziva aplikacije zahtjev za usmjeravanje (ARR) u svaku instanci uloge. ARR reroutes HTTP zahtjevima za instancu odgovarajuću ulogu. Kompleta alata za automatski reconfigures odabrane krajnjoj točki tako da se dolazni promet HTTP najprije usmjeravanja ARR softvera. Kompleta alata za također se stvara krajnju točku Interna koji Java poslužitelj konfiguriran da biste poslušali. To je krajnja točka koristi ARR za preusmjeravanje prometa HTTP instanci odgovarajuću ulogu. Na taj način svaku instancu uloga u implementaciji sustava više instanci služi kao obrnutim proxy poslužitelj za sve druge instance, omogućivanje ljepljive sesije.

## <a name="notes-about-session-affinity"></a>Napomene o sesiji afinitet ##

* Sesije afinitet ne funkcionira u emulator računalnim. Postavke se mogu primijeniti u emulator računalnim bez ometaju proces Sastavi ili izračunati emulator izvođenja, ali značajka sam neće funkcionirati unutar emulator računalnim.
* Omogućivanje sesiju afinitet rezultirat će povećava mogu prostora na disku zauzima uvođenja u Azure, kao što je dodatnim softverom će preuzeti i instalirati u vaša uloga instance nakon pokretanja na servisu u oblaku Azure.
* Vrijeme pokretanje ulogama će trajati dulje.
* Interna krajnje točke, funkcionirati kao promet rerouter kao što je rečeno, bit će added.ss

Primjer održavati podatke sesiju kada je omogućen sesiju afinitet potražite [u][]članku održavati podatke sesije s afinitet sesiju.

## <a name="see-also"></a>Vidi također ##

[Azure komplet alata za Eclipse][]

[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse][]

[Instalacija alata za Azure za Eclipse][] 

[Kako održavati podatke sesije s afinitet sesiju][]

Dodatne informacije o korištenju Azure s Java potražite u članku [Razvojni centar za Azure Java][].

<!-- URL List -->

[Razvojni centar za Azure Java]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure komplet alata za Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Stvaranje aplikacije svijeta pozdrav za Azure u Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Kako održavati podatke sesije s afinitet sesiju]: http://go.microsoft.com/fwlink/?LinkID=699539
[Instalacija alata za Azure za Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png
