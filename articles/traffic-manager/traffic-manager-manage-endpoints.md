<properties
    pageTitle="Upravljanje krajnje točke u Azure promet Manager | Microsoft Azure"
    description="U ovom se članku pronaći ćete dodati, ukloniti, Omogućivanje i onemogućivanje krajnje točke iz Azure promet upravitelja."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="add-disable-enable-or-delete-endpoints"></a>Dodavanje, onemogućivanje, Omogućivanje i brisanje krajnje točke

Značajka web-aplikacije u aplikacije servisa za Azure već omogućuje prebacivanje i promet kružnog usmjeravanje funkcije za web-mjesta unutar podatkovnog centra, bez obzira na način web-mjesta. Azure promet Upravitelj omogućuje vam da navedete prebacivanje i promet kružnog preusmjeravanje za web-mjesta i oblaka usluge u različitim podatkovnim centrima. U prvom koraku potrebne za pružanje funkcija je da biste dodali krajnju točku usluga ili web-mjesto oblaka upravitelju promet.

>[AZURE.NOTE]  U ovom se članku objašnjava kako pomoću portala za klasični. Azure klasični portal podržava samo stvaranje i dodjela servise u oblaku i web-aplikacije kao krajnje točke. Novi [Azure portal](https://portal.azure.com) je Preferirani sučelje.

Možete i onemogućiti pojedinačne krajnje točke koje su dio promet upravitelj profila. Onemogućivanje krajnje ostavlja kao dio profila, ali profil se ponaša kao krajnja točka nije obuhvaćen ga. To je korisno za privremeno uklanjanje krajnje točke u načinu za održavanje ili se ponovno implementirati. Kada na krajnjoj točki i pokretanje ponovno, možete je omogućiti.

>[AZURE.NOTE] Onemogućivanje krajnje ne sadrži ništa učiniti s stanje implementacije u Azure. Dobar krajnjoj točki ostaje i primati promet čak i kad je onemogućen u upravitelju promet. Uz to, onemogućivanje krajnje točke u jednom profilu ne utječe na njezino će se stanje drugi profil.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Da biste dodali oblaka krajnje usluga ili web-mjesta

1. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži krajnjoj točki postavke koje želite izmijeniti. Da biste otvorili stranicu s postavkama, kliknite strelicu s desne strane naziva profila.
2. Pri vrhu stranice kliknite **krajnje točke** da biste pogledali krajnje točke koje su već dio konfiguraciju.
3. Pri dnu stranice kliknite **Dodaj** da biste pristupili stranici **Krajnje točke Dodavanje servisa** . Prema zadanim postavkama, na stranici popis servisa u oblaku u odjeljku **Krajnje točke servisa**.
4. Za servise u oblaku, na popisu da biste ih dodali kao krajnje točke za ovaj profil odaberite servise u oblaku. Čišćenje naziv usluge oblak je uklanja s popisa krajnje točke.
5. Za web-mjesta, kliknite padajućem popisu **Vrsta servisa** , a zatim odaberite **web-aplikacije**.
6. Na popisu da biste ih dodali kao krajnje točke za ovaj profil odaberite web-mjesta. Čišćenje naziv web-mjesta je uklanja s popisa krajnje točke. Možete odabrati samo jednu web-mjesta po Azure podatkovnog centra (poznat i kao regija). Kad odaberete prvu web-mjesta, na drugim web-mjesta u istom podatkovnog centra postaju dostupne za odabir. Imajte na umu i navedene su samo standardni web-mjesta.
7. Nakon što odaberete krajnje točke za ovaj profil, kliknite kvačicu dolje desno da biste spremili promjene.

>[AZURE.NOTE] Nakon dodavanja ili ukloniti krajnje iz profila metodom usmjeravanje *Prebacivanje* promet, na popisu prioritet prebacivanje možda ne naručiti oni onako kako želite. Možete prilagoditi njihov redoslijed na popisu prioritet prebacivanje na stranici Konfiguracija. Dodatne informacije potražite u članku [usmjeravanje prometa konfiguriranje prebacivanje](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Da biste onemogućili krajnje točke

1. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži krajnjoj točki postavke koje želite izmijeniti. Da biste otvorili stranicu s postavkama, kliknite strelicu s desne strane naziva profila.
2. Pri vrhu stranice kliknite **krajnje točke** da biste pogledali krajnje točke uvrštene u konfiguraciji.
3. Kliknite krajnju točku koju želite onemogućiti, a zatim kliknite **Onemogući** pri dnu stranice.
4. Klijenti i dalje želite slati promet krajnjoj tijekom trajanja vrijeme važenja (TTL). Možete promijeniti TTL na stranici Konfiguracija promet upravitelj profila.

## <a name="to-enable-an-endpoint"></a>Da biste omogućili krajnje točke

1. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži krajnjoj točki postavke koje želite izmijeniti. Da biste otvorili stranicu s postavkama, kliknite strelicu s desne strane naziva profila.
2. Pri vrhu stranice kliknite **krajnje točke** da biste pogledali krajnje točke uvrštene u konfiguraciji.
3. Kliknite krajnju točku koju želite omogućiti, a zatim kliknite **Omogući** pri dnu stranice.
4. Klijenti oni koji sadrže omogućeno krajnju točku kao diktirali koju profil.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Da biste izbrisali oblaka krajnje usluga ili web-mjesta

1. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži krajnjoj točki postavke koje želite izmijeniti. Da biste otvorili stranicu s postavkama, kliknite strelicu s desne strane naziva profila.
2. Pri vrhu stranice kliknite **krajnje točke** da biste pogledali krajnje točke koje su već dio konfiguraciju.
3. Na stranici krajnje točke kliknite naziv krajnju točku koju želite izbrisati iz profila.
4. Pri dnu stranice kliknite **Izbriši**.

## <a name="next-steps"></a>Daljnji koraci

* [Upravljanje profilima Upravitelj promet](traffic-manager-manage-profiles.md)
* [Konfiguriranje načina usmjeravanja](traffic-manager-configure-routing-method.md)
* [Otklanjanje poteškoća promet Upravitelj smanjena stanja](traffic-manager-troubleshooting-degraded.md)
* [Razmatranja promet Upravitelj performansi](traffic-manager-performance-considerations.md)
* [Operacije na Upravitelj promet (REST API referenca)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
