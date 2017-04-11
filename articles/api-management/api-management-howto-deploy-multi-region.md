<properties
    pageTitle="Kako implementirati instanca servisa Azure API upravljanje više područja Azure"
    description="Saznajte kako implementirati instanca servisa Azure API upravljanje više Azure područja." 
    services="api-management"
    documentationCenter=""
    authors="steved0x"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="how-to-deploy-an-azure-api-management-service-instance-to-multiple-azure-regions"></a>Kako implementirati instanca servisa Azure API upravljanje više područja Azure

Upravljanje API podržava implementaciju više područja koja omogućuje izdavači API-JA za jednu API servisa za upravljanje raspodijelite bilo koji broj željeni Azure područja. Omogućuje smanjivanje zahtjev Latencija izgledati po geografski distributed API korisnici te također poboljšava dostupnost usluge određenu regiju vodi izvan mreže. 

Kada servis za upravljanje API prethodno, sadrži samo jedna [jedinica][] i nalazi se u jedno područje Azure koji je označen kao primarni regija. Dodatni područja mogu se jednostavno dodati putem klasične portala za Azure. Poslužitelj za pristupnik za upravljanje API je implementiran na svakom području i promet poziv će biti proslijeđene najbliže pristupnika. Ako je područje vodi izvan mreže, promet je automatski ponovno usmjerenog sljedeći najbliže pristupnika. 

> [AZURE.IMPORTANT] Uvođenje više područja dostupna je samo u sloju **[Premium][]** .

## <a name="add-region"> </a>Implementacija upravljanja API servisa instance novo područje

Da biste započeli, kliknite **Upravljanje** Azure klasični portalu servisa za upravljanje API-JA. Tako ćete doći do portala za upravljanje API programa publisher.

![Portal za Publisher][api-management-management-console]

>Ako još niste stvorili instancu upravljanje API servisa, potražite u članku [Stvaranje instanca servisa API upravljanje][] praktičnog vodiča za [Početak rada s upravljanjem Azure API -JA][] .

Idite na karticu **Skaliranje** Azure klasični portalu za vaše instanca servisa za upravljanje API-JA. 

![Kartica skala][api-management-scale-service]

Za implementaciju novo područje, kliknite na padajućem popisu ispod primarnog regija, a zatim odaberite područje s popisa.

![Dodavanje područja][api-management-add-region]

Kada je odabrana područja, odaberite broj jedinica za novo područje s padajućeg popisa jedinica.

![Odredite jedinice][api-management-select-units]

Kada niste konfigurirali željeni područjima i jedinice, kliknite **Spremi**.

## <a name="remove-region"> </a>Brisanje instanca servisa API upravljanje iz područja

Da biste uklonili instanca servisa API upravljanje iz područja, otvorite karticu **Skaliranje** Azure klasični portalu za vaše instanca servisa za upravljanje API-JA. 

![Kartica skala][api-management-scale-service]

Kliknite **X** s desne strane željeno područje da biste uklonili.  

![Uklanjanje regija][api-management-remove-region]

Kada su uklonjene željeni područja, kliknite **Spremi**.


[api-management-management-console]: ./media/api-management-howto-deploy-multi-region/api-management-management-console.png

[api-management-scale-service]: ./media/api-management-howto-deploy-multi-region/api-management-scale-service.png
[api-management-add-region]: ./media/api-management-howto-deploy-multi-region/api-management-add-region.png
[api-management-select-units]: ./media/api-management-howto-deploy-multi-region/api-management-select-units.png
[api-management-remove-region]: ./media/api-management-howto-deploy-multi-region/api-management-remove-region.png

[Stvoriti instancu servisa za upravljanje API-JA]: api-management-get-started.md#create-service-instance
[Početak rada s upravljanjem API Azure]: api-management-get-started.md

[Deploy an API Management service instance to a new region]: #add-region
[Delete an API Management service instance from a region]: #remove-region

[jedinica]: http://azure.microsoft.com/pricing/details/api-management/
[Premium]: http://azure.microsoft.com/pricing/details/api-management/

