<properties 
    pageTitle="Povezani i povezani resursi u galeriji pločica" 
    description="Saznajte više o povezani i povezani resursi koji se prikazuju u galeriji pločica portala za Azure pretpregled." 
    services="azure-portal" 
    documentationCenter="" 
    authors="adamabdelhamed" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="azure-portal" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/16/2015" 
    ms.author="adamab"/>

# <a name="related-and-linked-resources-in-the-tile-gallery"></a>Povezani i povezani resursi u galeriji pločica

Galerija pločica omogućuje vam da biste pronašli pločice za određeni resurs i povucite na trenutni plohu. Pomoću galerije pločice, možete stvoriti prikaze upravljanje koje se protežu na resurse. Povezani resursi za određeni resurs obuhvaćaju sve resurse koji se zajednički koristiti iste grupe resursa kao resurs i sve resursa koje sadrže vezu da biste ili resursa.

## <a name="linked-resources-in-azure-resource-manager"></a>Povezani resursi u Azure Voditelj resursa

Povezivanje je značajka od Voditelj resursa Azure.  Omogućuje deklarirati odnose između resursa, čak i ako se ne nalaze u istoj grupi resursa. Povezivanje ima bez utjecaja na runtime resursa, bez utjecaja na naplatu i bez utjecaja na temelju uloga pristup.  To je jednostavno mehanizam možete koristiti za predstavljanje odnosa tako da se pojave Alati kao što je galerija pločica pružaju obogaćenog upravljanje.  Alati za vaše možete pregledati veze pomoću veza API-JA i omogućuju upravljanje odnosima prilagođene iskustvo kao i. 

## <a name="how-do-i-link-my-resources"></a>Kako se povezati Moje resursa?

Kada stvarate resursi putem portala sustava ili uvođenje predloška Azure PowerShell ili EŽA Azure, veza se automatski stvaraju za neke zavisne resurse. Resursi i programski možete povezati pomoću [Povezani resursi REST API -JA](https://msdn.microsoft.com/library/azure/mt238499.aspx) ili deklariranje odnosa u predlošku. Rasprave u radu s povezane resursima potražite u članku [Linking resursa u Azure Voditelj resursa](../resource-group-link-resources.md).

## <a name="next-steps"></a>Daljnji koraci

- Ako vam je potrebna Uvod u predloške Azure Voditelj resursa za pisanje, potražite u članku [Predlošci na dokumentima](../resource-group-authoring-templates.md).
- Da biste prikazali detalje o stvaranju veza između resursa, potražite u članku [Linking resursa u Azure Voditelj resursa](../resource-group-link-resources.md).
- Da biste shvatili dodatne informacije o radu s grupama resursa putem portala za pretpregled, potražite [pomoću portala za pretpregled Azure upravljanja Azure resurse](resource-group-portal.md).
