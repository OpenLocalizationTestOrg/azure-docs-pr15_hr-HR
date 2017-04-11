<properties 
    pageTitle="Kako prilagoditi portala za razvojne inženjere upravljanje API Azure pomoću predložaka | Microsoft Azure" 
    description="Saznajte kako prilagoditi portala za razvojne inženjere upravljanje API Azure pomoću predložaka." 
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


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Kako prilagoditi portala za razvojne inženjere upravljanje API Azure pomoću predložaka

Azure upravljanja API nudi nekoliko značajki prilagodbe koje omogućuju administratorima da biste [prilagodili izgled i dojam portala za razvojne inženjere](api-management-customize-portal.md), kao što možete prilagoditi sadržaj stranice portala za razvojne inženjere pomoću čitav niz predložaka koja konfigurirati sadržaj stranice sami. Imate pomoću [DotLiquid](http://dotliquidmarkup.org/) sintaksa i navedeni skup resursa lokaliziranim niz, ikone i kontrole stranice, odlične fleksibilnost konfigurirajte sadržaj stranice prema svojim potrebama pomoću ove predložaka.

## <a name="developer-portal-templates-overview"></a>Pregled predložaka portala za razvojne inženjere

Predlošci za portala za razvojne inženjere upravlja se portala za razvojne inženjere administratori instanca servisa za upravljanje API-JA. Upravljanje predlošcima za razvojne inženjere, do vaše instancu upravljanje API servisa na portalu klasični Azure, a zatim **Pregledaj**.

![Portala za razvojne inženjere][api-management-browse]

Ako se već nalaze na portalu za publisher, portala za razvojne inženjere možete pristupiti tako da kliknete **portala za razvojne inženjere**.

![Izbornik portala za razvojne inženjere][api-management-developer-portal-menu]

Da biste pristupili predložaka portala za razvojne inženjere, kliknite ikonu Prilagodi na lijevoj strani da biste prikazali izbornik prilagodbe pa kliknite **Predlošci**.

![Predlošci za portala za razvojne inženjere][api-management-customize-menu]

Popis predložaka prikazuje nekoliko kategorija predložaka koji prekriva druge stranice portala za razvojne inženjere. Svaki predložak razlikuje se, ali su koraci za njihovo uređivanje i objavljivanje promjene isti. Da biste uredili predložak, kliknite naziv predloška.

![Predlošci za portala za razvojne inženjere][api-management-templates-menu]

Kliknite predložak vodi vas na stranici portala za razvojne inženjere moguće je prilagoditi tako da taj predložak. U ovom primjeru **Popis proizvoda** prikazuje se predložak. Predložak **popisa proizvoda** određuje područje zaslona označenu crvena pravokutnik. 

![Predložak popisa proizvoda][api-management-developer-portal-templates-overview]

Neke predloške, kao što su predlošci **Korisničkog profila** , prilagodite različite dijelove na istoj stranici. 

![Predlošci korisničkih profila][api-management-user-profile-templates]

Uređivač za svaki predložak portala za razvojne inženjere sadrži dvije sekcije koje se prikazuju pri dnu stranice. S lijeve strane prikazuje okno Uređivanje predloška i desnoj strani prikazuje podatkovnog modela za predložak. 

Predložak uređivanje okno sadrži oznake koje određuje izgled i ponašanje odgovarajuće stranice portala za razvojne inženjere. Oznake u predlošku koristi sintaksu [DotLiquid](http://dotliquidmarkup.org/) . Jedan popularne uređivač za DotLiquid je [DotLiquid za dizajnera](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Promjene koje ste napravili predložak tijekom uređivanja prikazuju se u stvarnom vremenu u web-pregledniku, ali nisu vidljive klijentima dok ste [Spremanje](#to-save-a-template) i [Objavljivanje](#to-publish-a-template) predloška.

![Predložak oznake][api-management-template]

U oknu **podataka predložak** omogućuje vodič u podatkovni model za entiteti koji su dostupni za upotrebu u određeni predložak. Ovaj vodič nudi prikazom podataka uživo koje su trenutno prikazane na portalu za razvojne inženjere. Predložak okna možete proširiti tako da kliknete pravokutnik u gornjem desnom kutu okna **podataka predloška** .

![Predložak podatkovnog modela][api-management-template-data]

U prethodnom primjeru postoje dva proizvoda prikazana u portala za razvojne inženjere koji nisu dohvaćeni iz podataka koji se prikazuju u oknu **podataka predloška** , kao što je prikazano u sljedećem primjeru.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

Oznake u predlošku **Popis proizvoda** obrađuje podatke možete unijeti željeni izlazni rezultat po iterating kroz zbirke proizvoda da biste prikazali informacije i veze za svaki pojedinačni proizvod. Napomena u `<search-control>` i `<page-control>` elemenata u oznake. Te upravljati prikazom za pretraživanje i označavanje stranica kontrole na stranici. `ProductsStrings|PageTitleProducts`Referenca lokalizirane niz koji sadrži na `h2` tekst zaglavlja za stranicu. Popis niza resursa, kontrole stranice i ikone koje su dostupne za koristiti u predlošcima portala za razvojne inženjere, potražite u članku [Referenca za upravljanje API -JA za razvojne inženjere portala predložaka](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>Da biste spremili predložak

Da biste spremili predložak, kliknite Spremi u uređivaču predložak.

![Spremanje predloška][api-management-save-template]

Spremljene promjene nisu uživo u portala za razvojne inženjere dok se ne mogu se objaviti.

## <a name="to-publish-a-template"></a>Da biste objavili predloška

Spremljeno predložaka moguće objavljivati pojedinačno ili sve zajedno. Da biste objavili pojedinačnih predložak, kliknite Objavi u uređivaču predložak.

![Objavljivanje predloška][api-management-publish-template]

Kliknite **da** da biste potvrdili i nemojte predložak uživo portala za razvojne inženjere.

![Potvrda objavljivanja][api-management-publish-template-confirm]

Da biste objavili svih verzija trenutno objavu predložak, kliknite **Objavi** na popisu predložaka. O neobjavljenim predložaka su određen zvjezdicu slijedi naziv predloška. U ovom primjeru tijeku Objavljivanje predložaka **popisa proizvoda** i **proizvoda** .

![Objavljivanje predložaka][api-management-publish-templates]

Kliknite **Objavi prilagodbe** da biste je potvrdili.

![Potvrda objavljivanja][api-management-publish-customizations]

Upravo objavljene Predlošci su učinkovitih odmah na portalu za razvojne inženjere.

## <a name="to-revert-a-template-to-the-previous-version"></a>Da biste vratili predloška na prethodnu verziju

Da biste vratili predloška na prethodnu objavljenu verziju, kliknite Vrati u uređivaču predložak.

![Vraćanje predloška][api-management-revert-template]

Kliknite **da** da biste potvrdili.

![Potvrda][api-management-revert-template-confirm]

Prethodno objavljenu verziju predloška uživo u portala za razvojne inženjere je nakon dovršetka postupka Vrati.

## <a name="to-restore-a-template-to-the-default-version"></a>Da biste vratili predloška na zadanu verziju

Vraćanje predložaka njihove zadanu verziju je dva koraka. Najprije morate vratiti predložaka, a zatim mora biti objavljena vraćene verzije.

Da biste vratili jedan predložak na zadanu verziju kliknite Vrati u uređivaču predložak.

![Predložak vratili][api-management-reset-template]

Kliknite **da** da biste potvrdili.

![Potvrda][api-management-reset-template-confirm]

Da biste vratili sve predloške njihove verzije zadane, kliknite **Vrati zadani predlošci** na popis predložaka.

![Vraćanje predložaka][api-management-restore-templates]

Vraćeni predložaka moraju pa objaviti pojedinačno ili svi odjednom tako da slijedite korake iz članka [Objavljivanje predloška](#to-publish-a-template).

## <a name="developer-portal-templates-reference"></a>Referenca za razvojne inženjere portala predložaka

Referentne informacije za razvojne inženjere portala predlošci, niz resursa, ikone i kontrole stranice potražite u članku [Upravljanje API reference portala predloške za razvojne inženjere](https://msdn.microsoft.com/library/azure/mt697540.aspx).

## <a name="watch-a-video-overview"></a>Pogledajte videozapis pregled

Pogledajte sljedeći videozapis da biste saznali kako dodati panela za raspravu i ocjene API i operacija stranice portala za razvojne inženjere pomoću predložaka.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







