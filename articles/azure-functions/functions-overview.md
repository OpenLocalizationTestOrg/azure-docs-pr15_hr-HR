<properties
   pageTitle="Azure funkcije Pregled | Microsoft Azure"
   description="Objašnjenje kako pomoću funkcije Azure da biste optimizirali asinkronog radnih opterećenja u minutama."
   services="functions"
   documentationCenter="na"
   authors="mattchenderson"
   manager="erikre"
   editor=""
   tags=""
   keywords="Azure funkcije, obrada događaj, webhooks, dinamični računalnim, funkcije serverless arhitekture"/>

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="08/29/2016"
   ms.author="cfowler;mahender;glenga"/>
   
   
# <a name="azure-functions-overview"></a>Pregled Azure funkcija

Azure funkcije je rješenje jednostavno radi male dijelove koda ili "funkcije," u oblaku. Možete napisati kod vam je potrebna za dani problem konkretnom, bez brige o cijelu aplikaciju ili Infrastruktura da biste ga pokrenuti. To možete učiniti razvoj još produktivni, a možete koristiti na razvoj jeziku po izboru, kao što je C#, F #, Node.js, Python ili PHP. Plaćanje samo za vrijeme izvođenja kod i pouzdanost Azure za promjenu veličine prema potrebi.

Ova tema sadrži više razine pregled Azure funkcije. Ako želite odmah u i početak rada s funkcijama Azure, započnite [Stvaranje prve Azure (funkcija)](functions-create-first-azure-function.md). Ako tražite dodatne tehničke informacije o funkcijama potražite u članku [Referenca za razvojne inženjere](functions-reference.md).

## <a name="features"></a>Značajke

Slijede ključne značajke Azure funkcija:
    
* **Odabir jezika** - funkcije za pisanje pomoću C#, F #, Node.js, Python, PHP, grupe, tulumu, jezika Java ili sve izvršnu datoteku.
* **Plaćanje po korištenju cijene model** – plaćanje samo za potrošeno vrijeme pokretanje koda. Dinamični servisa aplikacija planiranje mogućnost prikaže u [cijene sekcije](#pricing) ispod.  
* **Premjesti vlastite ovisnosti** - funkcije podržava NuGet i NPM, da biste mogli koristiti Omiljene biblioteke.  
* **Integrirano sigurnost** – zaštita HTTP pokrene funkcije s drugim davateljima OAuth kao što su Azure Active Directory, Facebook, Google, Twitter i Microsoftov Account.  
* **Integracija pojednostavljenog** - jednostavno pod utjecajem Azure servise i softver – kao-na-(SaaS) ponude servisa. Potražite u [odjeljku integracije](#integrations) ispod primjere.  
* **Prilagodljivo razvojno** - kod svojih funkcije na portalu ili postavljanje neprekinuti integracije i implementaciju kod putem GitHub, Visual Studio Team Services i druge [podržane alate za razvoj](../app-service-web/web-sites-deploy.md#deploy-using-an-ide).  
* **Otvori izvora** – u funkcije runtime je Otvori izvorišno i [dostupne na GitHub](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Što učiniti s funkcijama?

Azure funkcije je odličan rješenje za obradu podataka, Integracija sustave, rad s Interneta – od-stvari (IoT) te izgradnju jednostavne API-ji i microservices. Razmislite o funkcijama za zadatke poput sliku ili redoslijed obrada, održavanje datoteka, a zatim dugoročnih zadatke koji želite pokrenuti u pozadini niti ili za sve zadatke za koje želite pokrenuti na raspored. 

Funkcija nudi predloške za početak rada u ključni scenariji, uključujući sljedeće:

* **BlobTrigger** - blob-ova za pohranu Azure postupak kada se dodaju spremnika. To možete koristiti za promjenu veličine slike.
* **EventHubTrigger** - odgovaranje na događaje isporučena programa Azure koncentrator za događaj. Osobito korisni u aplikaciju instrumentation, obrada korisničko sučelje ili tijeka rada i scenariji Internet stvari (IoT).
* **Generički webhook** - postupak webhook HTTP zahtjeve iz bilo koji servis koji podržava webhooks.
* **GitHub webhook** - odgovaranje na događaje koji se pojavljuju u vašem spremištima GitHub. Na primjer, pročitajte članak [Stvaranje na webhook ili funkcija API-JA](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** - okidača izvođenja koda pomoću HTTP zahtjev.
* **QueueTrigger** - odgovaranje na poruke koje stižu u redu čekanja za Azure prostora za pohranu. Primjer potražite u članku [Stvaranje funkciji Azure koja povezuje sa servisom Azure](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** - povezati kod s drugih servisa za Azure ili na lokaciji usluge po slušanje redovima poruka. 
* **ServiceBusTopicTrigger** - povezati kod s drugih servisa za Azure ili na lokaciji servisa putem pretplate na temu. 
* **TimerTrigger** – izvršavanje čišćenje ili druge zadatke za obradu unaprijed definirane rasporedu. Na primjer, potražite u članku [Stvaranje događaja processing (opis funkcije)](functions-create-an-event-processing-function.md).

Azure funkcije podržava *okidača*, koje su načina pokretanja izvođenja koda i *povezivanja*, koji su načini pojednostavljivanje kodiranje za ulazni i izlazni podatke. Detaljan opis okidača i poveznici koji nudi funkcije Azure potražite u članku [funkcije Azure okidača i reference za razvojne inženjere povezivanja](functions-triggers-bindings.md).


## <a name="integrations"></a>Integracije

Azure funkcije integrira u raznim Azure i 3 davatelja usluga. Te možete koristiti za pokretanje funkcija i počnite izvođenja ili za služiti kao unos, a zatim izlaz za kod. Funkcija Azure podržava sljedeće integracije servisa. 

* Azure DocumentDB
* Koncentratorima Azure događaja 
* Azure mobilne aplikacije (tablice)
* Azure obavijesti koncentratora
* Azure Bus servisa (redovima i teme)
* Azure prostora za pohranu (blob, redovima i tablice) 
* GitHub (webhooks)
* Lokalni (pomoću servisa Bus)

## <a name="pricing"></a>Koliko stoji funkcije trošak?

Azure funkcije sadrži dvije vrste cijene tarife, odabrati neki koji najbolje odgovara vašim potrebama: 

* **Dinamični hostinga plan** – kada se pokrene funkcija, Azure sadrži sve potrebne računalne resurse. Ne morate brinuti o upravljanju resursa, a samo platiti vremena koja se pokreće kod. Sve pojedinosti o cijenama dostupne su na [funkcije cijene stranice](/pricing/details/functions). 

* **Aplikacije servisa za planiranje** – pokretanje sustava funkcija baš kao na web, mobile i aplikacije API-JA. Ako već koristite aplikacije servisa za sve aplikacije, možete pokrenuti na funkcije na istu tarifu pri bez dodatnih troškova. Sve pojedinosti možete pronaći na [aplikaciju rezervnih stranice](/pricing/details/app-service/).

Dodatne informacije o skaliranje funkcije, potražite [u](functions-scale.md)članku promjena veličine Azure funkcije.

##<a name="next-steps"></a>Daljnji koraci

+ [Stvaranje prve Azure (funkcija)](functions-create-first-azure-function.md)  
Odmah u i stvaranje prve funkcija pomoću funkcije Azure brzi početak rada. 
+ [Azure funkcije reference za razvojne inženjere](functions-reference.md)  
Pruža dodatne tehničke informacije o izvođenja funkcije Azure i reference za kodiranje funkcije i definiranje okidača i povezivanja.
+ [Testiranje Azure funkcije](functions-test-a-function.md)  
U članku se opisuje različite Alati i tehnike za testiranje sustava funkcije.
+ [Upute za promjenu veličine Azure funkcije](functions-scale.md)  
U članku se opisuje servisa tarife dostupno u sklopu Azure funkcija, uključujući tarifa za dinamičku servis te kako odabrati desnom plan. 
+ [Dodatne informacije o aplikacije servisa za Azure](../app-service/app-service-value-prop-what-is.md)  
Azure funkcije upravlja platforme Azure aplikacije servisa za osnovne funkcije kao što su implementacije, varijable okruženja i Dijagnostika. 