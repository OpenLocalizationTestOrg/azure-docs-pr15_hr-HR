<properties
   pageTitle="Upravljanje krajnje točke u Azure promet Manager | Microsoft Azure"
   description="U ovom se članku pronaći ćete dodati, ukloniti, Omogućivanje i onemogućivanje krajnje točke iz Azure promet upravitelja."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Dodavanje, onemogućivanje, omogućivanje ili brisanje krajnje točke

Značajka web-aplikacije u aplikacije servisa za Azure već omogućuje prebacivanje i promet kružnog usmjeravanje funkcije za web-mjesta unutar podatkovnog centra, bez obzira na način web-mjesta. Azure promet Upravitelj omogućuje vam da navedete prebacivanje i promet kružnog preusmjeravanje za web-mjesta i oblaka usluge u različitim podatkovnim centrima. U prvom koraku potrebne za pružanje funkcija je da biste dodali krajnju točku usluga ili web-mjesto oblaka upravitelju promet.

>[AZURE.NOTE] Ne možete dodati vanjskim mjestima ili upravitelj promet profili kao krajnje točke pomoću portala za Azure klasični. Morate koristiti REST API-JA [Stvaranje definicija](http://go.microsoft.com/fwlink/p/?LinkId=400772) ili komponente Windows PowerShell [Dodaj AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774).

Možete i onemogućiti pojedinačne krajnje točke koje su dio promet upravitelj profila. Krajnje točke obuhvaćaju servise u oblaku i web-mjesta. Onemogućivanje krajnje ostavlja kao dio profila, ali profil se ponaša kao krajnja točka nije obuhvaćen ga. Ova akcija vrlo je praktična za privremeno uklanjanje krajnje točke u načinu za održavanje ili se ponovno implementirati. Kada na krajnjoj točki i pokretanje ponovno, možete je omogućiti.

>[AZURE.NOTE] Onemogućivanje krajnje ne sadrži ništa učiniti s stanje implementacije u Azure. Dobar krajnjoj točki će ostati i primati promet čak i kad je onemogućen u upravitelju promet. Uz to, onemogućivanje krajnje točke u jednom profilu ne utječe na njezino će se stanje drugi profil.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Da biste dodali oblaka krajnje usluga ili web-mjesta


1. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži krajnjoj točki postavke koje želite izmijeniti, a zatim strelicu s desne strane naziva profila. Otvorit će se na stranicu Postavke profila.
2. Pri vrhu stranice kliknite **krajnje točke** da biste pogledali krajnje točke koje su već dio konfiguraciju.
3. Pri dnu stranice kliknite **Dodaj** da biste pristupili stranici **Krajnje točke Dodavanje servisa** . Prema zadanim postavkama, na stranici popis servisa u oblaku u odjeljku **Krajnje točke servisa**.
4. Za servise u oblaku, na popisu da biste ih omogućiti kao krajnje točke za ovaj profil odaberite servise u oblaku. Čišćenje naziv usluge oblak je uklanja s popisa krajnje točke.
5. Za web-mjesta, kliknite padajućem popisu **Vrsta servisa** , a zatim odaberite **web-aplikacije**.
6. Na popisu da biste ih dodali kao krajnje točke za ovaj profil odaberite web-mjesta. Čišćenje naziv web-mjesta je uklanja s popisa krajnje točke. Imajte na umu da možete odabrati samo jednu web-mjesta po Azure podatkovnog centra (poznat i kao regija). Ako odaberete web-mjesto u podatkovnom centru koji hostira više web-mjesta kad odaberete prvu web-mjesta, drugima u istom podatkovnog centra postaju dostupne za odabir. Imajte na umu i navedene su samo standardni web-mjesta.
7. Nakon što odaberete krajnje točke za ovaj profil, kliknite kvačicu dolje desno da biste spremili promjene.

>[AZURE.NOTE] Ako koristite usmjeravanje način promet *Prebacivanje* nakon što dodate ili uklonite krajnje, obavezno da biste prilagodili popisu prioritet prebacivanje na stranici Konfiguracija u skladu s vizualnim prebacivanje redoslijedom kojim želite za konfiguraciju. Dodatne informacije potražite u članku [usmjeravanje prometa konfiguriranje prebacivanje](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Da biste onemogućili krajnje točke

1. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži krajnjoj točki postavke koje želite izmijeniti, a zatim strelicu s desne strane naziva profila. Otvorit će se na stranicu Postavke profila.
2. Pri vrhu stranice kliknite **krajnje točke** da biste pogledali krajnje točke uvrštene u konfiguraciji.
3. Kliknite krajnju točku koju želite onemogućiti, a zatim kliknite **Onemogući** pri dnu stranice.
4. Promet će prekinuti slijedi krajnjoj temelju na DNS vrijeme važenja (TTL) konfigurirana za promet Upravitelj naziva domene. Na stranici Konfiguracija profila Upravitelja promet možete promijeniti na TTL.

## <a name="to-enable-an-endpoint"></a>Da biste omogućili krajnje točke

1. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži krajnjoj točki postavke koje želite izmijeniti, a zatim strelicu s desne strane naziva profila. Otvorit će se na stranicu Postavke profila.
2. Pri vrhu stranice kliknite **krajnje točke** da biste pogledali krajnje točke uvrštene u konfiguraciji.
3. Kliknite krajnju točku koju želite omogućiti, a zatim kliknite **Omogući** pri dnu stranice.
4. Promet započinje slijedi servis ponovno kao Diktirani koju profil.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Da biste izbrisali oblaka krajnje usluga ili web-mjesta


1. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži krajnjoj točki postavke koje želite izmijeniti, a zatim strelicu s desne strane naziva profila. Otvorit će se na stranicu Postavke profila.
2. Pri vrhu stranice kliknite **krajnje točke** da biste pogledali krajnje točke koje su već dio konfiguraciju.
3. Na stranici krajnje točke kliknite naziv krajnju točku koju želite izbrisati iz profila.
4. Pri dnu stranice kliknite **Izbriši**.

>[AZURE.NOTE] Nije moguće izbrisati vanjskim mjestima ili upravitelj promet profili kao krajnje točke pomoću portala za Azure klasični. Morate koristiti komponente Windows PowerShell. Dodatne informacije potražite u članku [Uklanjanje AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx).

## <a name="next-steps"></a>Daljnji koraci

- [Konfiguriranje metodu usmjeravanja prebacivanje](traffic-manager-configure-failover-routing-method.md)
- [Konfiguriranje metodu usmjeravanja kružnog](traffic-manager-configure-round-robin-routing-method.md)
- [Konfiguriranje metodu usmjeravanja performansi](traffic-manager-configure-performance-routing-method.md)
- [Otklanjanje poteškoća promet Upravitelj smanjena stanja](traffic-manager-troubleshooting-degraded.md)
- [Operacije na Upravitelj promet (REST API referenca)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
