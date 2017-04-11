<properties 
    pageTitle="Kako koristiti svojstva u pravila upravljanja API Azure" 
    description="Saznajte kako koristiti svojstva u pravila upravljanja Azure API-JA." 
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


# <a name="how-to-use-properties-in-azure-api-management-policies"></a>Kako koristiti svojstva u pravila upravljanja API Azure

Pravila upravljanja API su naprednih mogućnosti sustava koje omogućuju publisher da biste promijenili ponašanje API kroz konfiguraciju. Pravila su skup izraza koji se izvode sekvencijalno na zahtjev ili odgovor API. Izraze pravila možete konstruirana pomoću doslovni tekst vrijednosti, pravila izraza i svojstva. 

Svaku instancu servisa za upravljanje API sadrži skup svojstava parove ključa vrijednosti koji su globalni instanca servisa. Tih svojstava se može koristiti za upravljanje vrijednosti konstanta niza preko svih API konfiguracije i pravila. Svako svojstvo sadrži sljedećim atributima:


| Atribut | Vrsta            | Opis                                                                                             |
|-----------|-----------------|---------------------------------------------------------------------------------------------------------|
| Ime      | niz          | Naziv svojstva. Može sadržavati samo slova, brojki, razdoblje, crtice i podvučene znakove. |
| Vrijednost     | niz          | Vrijednost svojstva. Možda neće biti prazna ili se sastojati samo od razmak.                           |
| Tajna    | Booleove vrijednosti         | Određuje je li vrijednost u tajna i trebali šifrirati ili ne.                                |
| Oznaka      | polje niza | Neobavezno oznake koje kada dani može se koristiti za filtriranje popisa svojstava.                               |

Na portalu za publisher na kartici **Svojstva** su konfigurirana svojstva. U sljedećem primjeru su konfigurirana svojstva cjelokupno tri.

![Svojstva][api-management-properties]

Vrijednosti svojstava mogu sadržavati nizova literala i [pravila izraza](https://msdn.microsoft.com/library/azure/dn910913.aspx). Sljedeća tablica prikazuje prethodna tri primjera svojstva i njihove atribute. Vrijednost `ExpressionProperty` je pravilnik izraz koji vraća niz koji sadrži trenutni datum i vrijeme. Svojstvo `ContosoHeaderValue` označen kao tajna, tako da se ne prikazuje vrijednost.

| Ime               | Vrijednost                      | Tajna | Oznaka    |
|--------------------|----------------------------|--------|---------|
| ContosoHeader      | TrackingId                 | FALSE  | Contoso |
| ContosoHeaderValue | ••••••••••••••••••••••     | TRUE   | Contoso |
| ExpressionProperty | @(DateTime.Now.ToString()) | FALSE  |         |

## <a name="to-use-a-property"></a>Da biste koristili svojstvo

Da biste koristili svojstvu pravilo, postavite naziv svojstva unutar dvostrukih par vitičaste zagrade kao što su `{{ContosoHeader}}`, kao što je prikazano u sljedećem primjeru.

    <set-header name="{{ContosoHeader}}" exists-action="override">
      <value>{{ContosoHeaderValue}}</value>
    </set-header>

U ovom primjeru `ContosoHeader` se koristi kao naziv zaglavlja u na `set-header` pravila, i `ContosoHeaderValue` koristi se kao vrijednost tog zaglavlja. Kad se to pravilo procijeni tijekom odgovora pristupnika za upravljanje API-JA ili zahtjev za `{{ContosoHeader}}` i `{{ContosoHeaderValue}}` zamjenjuju njihovim vrijednostima odgovarajuća svojstva.

Svojstva mogu se koristiti kao dovršen atribut ili element vrijednosti kao što je prikazano u prethodnom primjeru, no oni mogu i umetnuti u ili u kombinaciji s dio teksta izraz kao što je prikazano u sljedećem primjeru:`<set-header name = "CustomHeader{{ContosoHeader}}" ...>`

Svojstva također mogu sadržavati pravila izraza. U sljedećem primjeru u `ExpressionProperty` koristi.

    <set-header name="CustomHeader" exists-action="override">
        <value>{{ExpressionProperty}}</value>
    </set-header>

Kad se to pravilo procijeni, `{{ExpressionProperty}}` stupaca mijenja vrijednošću: `@(DateTime.Now.ToString())`. Budući da je vrijednost pravila izraz, procjenjuje izraz i pravila nastavlja se sa njegova izvođenja.

Možete testirati ovaj Izlaz na portalu za razvojne inženjere tako da nazovete postupak koji sadrži pravila sa svojstvima u opsegu. U sljedećem primjeru operaciju zove se s dva prethodnom primjeru `set-header` pravila sa svojstvima. Imajte na umu da odgovor sadrži dvije prilagođenih zaglavlja koja su konfigurirana pomoću pravila sa svojstvima.

![Portala za razvojne inženjere][api-management-send-results]

Ako pogledate [praćenje API kontrola](api-management-howto-api-inspector.md) za pozive programa koji sadrži pravila prethodna dva uzorka sa svojstvima, vidjet ćete dva `set-header` pravilnike za vrijednosti nekretnina umetnuti kao i procjenu izraz pravila za svojstvo koje se nalaze izraz pravila.

![Praćenje kontrola API-JA][api-management-api-inspector-trace]

Imajte na umu tijekom vrijednosti nekretnina može sadržavati izraza pravila, vrijednosti nekretnina ne smije sadržavati druga svojstva. Ako je tekst koji sadrži referencu svojstvo se koristi za vrijednosti svojstva, kao što su `Property value text {{MyProperty}}`, referenci tog svojstva neće zamijeniti, a će biti dio vrijednosti svojstva.

## <a name="to-create-a-property"></a>Da biste stvorili svojstvo

Da biste stvorili svojstvo, kliknite **Dodaj svojstvo** na kartici **Svojstva** .

![Dodavanje svojstva][api-management-properties-add-property-menu]

**Naziv** i **vrijednost** su tražene vrijednosti. Ako je ovo svojstvo vrijednost u tajna, označite potvrdni okvir **Ovo je na tajna** . Unesite jednu ili više oznaka neobavezna će vam pomoći u organiziranju svojstva pa kliknite **Spremi**.

![Dodavanje svojstva][api-management-properties-add-property]

Prilikom spremanja novo svojstvo tekstni okvir za **pretraživanje svojstvo** je popunjen naziv novo svojstvo i prikazuju se novo svojstvo. Da biste prikazali sva svojstva, poništite tekstni okvir za **pretraživanje svojstva** , a zatim pritisnite enter.

![Svojstva][api-management-properties-property-saved]

Informacije o stvaranju svojstvo pomoću REST API-JA, potražite u članku [Stvaranje svojstva pomoću REST API -JA](https://msdn.microsoft.com/library/azure/mt651775.aspx#Put).

## <a name="to-edit-a-property"></a>Da biste uredili svojstvo

Da biste uredili svojstvo, kliknite **Uređivanje** pokraj svojstvo koje želite urediti.

![Uređivanje svojstva][api-management-properties-edit]

Napravite željene promjene, a zatim kliknite **Spremi**. Ako promijenite naziv svojstva, sva pravila koje upućuju na to svojstvo automatski se ažuriraju da koriste novi naziv.

![Uređivanje svojstva][api-management-properties-edit-property]

Informacije o uređivanju svojstvo pomoću REST API-JA, potražite u članku [Uređivanje svojstva pomoću REST API -JA](https://msdn.microsoft.com/library/azure/mt651775.aspx#Patch).

## <a name="to-delete-a-property"></a>Da biste izbrisali svojstvo

Da biste izbrisali svojstvo, kliknite **Izbriši** pokraj svojstvo koje želite izbrisati.

![Brisanje svojstva][api-management-properties-delete]

Kliknite da biste potvrdili **Da, izbrišite je** .

![Potvrda brisanja][api-management-delete-confirm]

>[AZURE.IMPORTANT] Ako je svojstvo poziva tako da sva pravila, neće biti moguće uspješno je izbrisati sve dok ne uklonite svojstvo iz svih pravila.

Informacije o brisanju svojstvo pomoću REST API-JA potražite u članku [Brisanje svojstva pomoću REST API -JA](https://msdn.microsoft.com/library/azure/mt651775.aspx#Delete).

## <a name="to-search-and-filter-properties"></a>Da biste pretraživanja i filtriranja svojstva

Kartica **Svojstva** obuhvaća pretraživanja i filtriranja za upravljanje svojstva. Da biste filtrirali popis svojstava prema naziv svojstva, u tekstni okvir **Svojstva za pretraživanje** unesite pojam za pretraživanje. Da biste prikazali sva svojstva, poništite tekstni okvir za **pretraživanje svojstva** , a zatim pritisnite enter.

![Pretraživanje][api-management-properties-search]

Da biste filtrirali popis svojstava po vrijednostima oznaku, unesite jednu ili više oznaka u tekstni okvir **Filtriraj po oznake** . Da biste prikazali sva svojstva, poništite okvir na **filtrirati oznake** tekstni okvir i pritisnite tipku enter.

![Filtar][api-management-properties-filter]

## <a name="next-steps"></a>Daljnji koraci

-   Dodatne informacije o radu s pravilima
    -   [Pravila u odjeljku Upravljanje API-JA](api-management-howto-policies.md)
    -   [Referenca za pravila](https://msdn.microsoft.com/library/azure/dn894081.aspx)
    -   [Pravilnik o izrazima](https://msdn.microsoft.com/library/azure/dn910913.aspx)

## <a name="watch-a-video-overview"></a>Pogledajte videozapis pregled

> [AZURE.VIDEO use-properties-in-policies]

[api-management-properties]: ./media/api-management-howto-properties/api-management-properties.png
[api-management-properties-add-property]: ./media/api-management-howto-properties/api-management-properties-add-property.png
[api-management-properties-edit-property]: ./media/api-management-howto-properties/api-management-properties-edit-property.png
[api-management-properties-add-property-menu]: ./media/api-management-howto-properties/api-management-properties-add-property-menu.png
[api-management-properties-property-saved]: ./media/api-management-howto-properties/api-management-properties-property-saved.png
[api-management-properties-delete]: ./media/api-management-howto-properties/api-management-properties-delete.png
[api-management-properties-edit]: ./media/api-management-howto-properties/api-management-properties-edit.png
[api-management-delete-confirm]: ./media/api-management-howto-properties/api-management-delete-confirm.png
[api-management-properties-search]: ./media/api-management-howto-properties/api-management-properties-search.png
[api-management-send-results]: ./media/api-management-howto-properties/api-management-send-results.png
[api-management-properties-filter]: ./media/api-management-howto-properties/api-management-properties-filter.png
[api-management-api-inspector-trace]: ./media/api-management-howto-properties/api-management-api-inspector-trace.png

