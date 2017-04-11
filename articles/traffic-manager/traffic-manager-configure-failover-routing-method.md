<properties
   pageTitle="Konfiguriranje metodu usmjeravanja za prebacivanje promet Upravitelj promet | Microsoft Azure"
   description="U ovom se članku pronaći ćete konfigurirati način za usmjeravanje prometa prebacivanje u upravitelju promet"
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-failover-routing-method"></a>Konfiguriranje metodu usmjeravanja prebacivanje

Često tvrtkom ili ustanovom želi pouzdanosti omogućavaju njegove servise. To čini unosom sigurnosne kopije servise za slučaj da funkcionira njihove primarni servisa. Uobičajeni obrazac za prebacivanje servisa tako da omogućuje skup identične usluga i slanje promet na servise u primarni uz zadržavanje konfigurirani popis jedne ili više usluga sigurnosne kopije. Ovu vrstu sigurnosno kopiranje s servise u oblaku Azure i web-mjesta možete konfigurirati prateći sljedeće postupke.

Imajte na umu da web-mjesta Azure već nudi prebacivanje promet usmjeravanje funkcionalnost način za web-mjesta unutar podatkovnog centra (poznat i kao područje), bez obzira na način web-mjesta. Upravitelj promet omogućuje vam da navedete metodu usmjeravanja prebacivanje promet za web-mjesta u različitim podatkovnim centrima.

## <a name="to-configure-failover-traffic-routing-method"></a>Konfiguriranje načina za usmjeravanje prometa prebacivanje:

1. Azure klasični portalu u lijevom oknu kliknite ikonu **Upravitelj promet** da biste otvorili okno Upravitelj promet. Ako još niste stvorili promet upravitelj profila, potražite u članku [Upravljanje profilima promet Upravitelj](traffic-manager-manage-profiles.md) korake da biste stvorili osnovni promet Upravitelj profil.
2. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži postavke koje želite izmijeniti, a zatim strelicu s desne strane naziva profila. Otvorit će se na stranicu Postavke profila.
3. Na stranici profila kliknite **krajnje točke** pri vrhu stranice i provjerite je li u oba cloud services i web-mjesta (krajnje točke) koja želite uvrstiti u konfiguraciji postoje. Upute za dodavanje ili uklanjanje krajnje točke, potražite u članku [Upravljanje točke u upravitelju promet](traffic-manager-endpoints.md).
4. Na stranici profila kliknite **Konfiguriraj** pri vrhu da biste otvorili stranicu konfiguracije.
5. **Način postavke za usmjeravanje prometa**, provjerite je li metodu usmjeravanja promet **Prebacivanje**. Ako nije, kliknite **Prebacivanje** s padajućeg popisa.
6. Za **Prebacivanje prioritet popis**Prilagodba redoslijeda prebacivanje za vaše krajnje točke. Kad odaberete **Prebacivanje** metodu usmjeravanja promet, redoslijed odabranih krajnje točke zapise. Primarni krajnje je na vrhu. Pomoću gore i dolje strelice da biste promijenili redoslijed prema potrebi. Informacije o postavljanju prioritet prebacivanje pomoću komponente Windows PowerShell potražite u članku [Postavljanje AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Provjerite je li se pravilno konfigurirane **Postavke nadzora** . Nadzor osigurava krajnje točke koje su izvan mreže se šalju promet. Da biste pratili krajnje točke, navedite put i naziv datoteke. Imajte na umu da kosa crta prosljeđivanja "/" valjana stavka za relativni put i podrazumijeva da se datoteka nalazi u korijenskom direktoriju (zadano). Dodatne informacije o nadzoru potražite u članku [Upravitelj promet praćenja](traffic-manager-monitoring.md).
8. Kada unesete sve promjene konfiguracije, kliknite **Spremi** pri dnu stranice.
9. Da biste testirali promjene u konfiguraciji. Dodatne informacije potražite u članku [Testiranje postavki upravitelj promet](traffic-manager-testing-settings.md) .
10. Kada je promet upravitelj profila postavljanje i radu, uređivanje DNS zapisa na DNS poslužitelj mjerodavne naziva domene tvrtke na promet Upravitelj naziva domene. Dodatne informacije o tome kako to učiniti potražite u članku [točke domene tvrtke Internet promet Upravitelj domena](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Daljnji koraci

[Domene tvrtke Internet na promet Upravitelj domena](traffic-manager-point-internet-domain.md)

[Upravitelj promet usmjeravanje metode](traffic-manager-routing-methods.md)

[Konfiguriranje metodu usmjeravanja kružnog](traffic-manager-configure-round-robin-routing-method.md)

[Konfiguriranje metodu usmjeravanja performansi](traffic-manager-configure-performance-routing-method.md)

[Otklanjanje poteškoća promet Upravitelj smanjena stanja](traffic-manager-troubleshooting-degraded.md)

[Promet Upravitelj – onemogućivanje, Omogući ili brisanje profila](disable-enable-or-delete-a-profile.md)

[Promet Upravitelj – onemogućivanje i omogućivanje krajnje točke](disable-or-enable-an-endpoint.md)

