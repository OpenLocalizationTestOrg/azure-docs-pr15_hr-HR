<properties
   pageTitle="Konfiguriranje upravitelja promet okrugle način za usmjeravanje prometa robin | Microsoft Azure"
   description="U ovom se članku pronaći ćete konfigurirati za krajnje točke vašeg upravitelja promet za ujednačavanje opterećenja za kružno."
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

# <a name="configure-round-robin-routing-method"></a>Konfiguriranje kružnog usmjeravanje način

Uobičajeni obrazac za promet usmjeravanje način je da biste naveli skup identične krajnje točke, što obuhvaća servise u oblaku i web-mjesta, i poslali promet za svaki unos u kružnog način. Koraci u nastavku strukture kako konfigurirati Upravitelj promet da bi se te vrste metodu usmjeravanja promet. Dodatne informacije o različite promet usmjeravanje metode potražite u članku [o upravitelju promet promet usmjeravanje metode](traffic-manager-routing-methods.md).

>[AZURE.NOTE] Azure web-mjesta već sadrži funkcije za web-mjesta unutar podatkovnog centra (poznat i kao regija) za ujednačavanje opterećenja kružnog dodjeljivanja. Upravitelj promet omogućuje vam da navedete metodu usmjeravanja kružnog promet za web-mjesta u različitim podatkovnim centrima.

## <a name="routing-traffic-equally-round-robin-across-a-set-of-endpoints"></a>Usmjeravanje prometa jednako (okrugle robin) u skupu rezultata krajnje točke:

1. Azure klasični portalu u lijevom oknu kliknite ikonu **Upravitelj promet** da biste otvorili okno Upravitelj promet. Ako još niste stvorili profila promet upravitelja, potražite u članku [Upravljanje profilima Upravitelj promet](traffic-manager-manage-profiles.md) za korake da biste stvorili osnovni promet Upravitelj profil.
2. Azure klasični portalu u oknu promet Upravitelj pronađite promet Upravitelj profil koji sadrži postavke koje želite izmijeniti, a zatim strelicu s desne strane naziva profila. Otvorit će se na stranicu Postavke profila.
3. Na stranici za svoj profil, kliknite **krajnje točke** pri vrhu stranice, a zatim provjerite jesu li krajnje točke servisa koji želite uvrstiti u konfiguraciji prezentacija. Upute za dodavanje ili uklanjanje krajnje točke, potražite u članku [Upravljanje točke u upravitelju promet](traffic-manager-endpoints.md).
4. Na stranici profila kliknite **Konfiguriraj** pri vrhu da biste otvorili stranicu konfiguracije.
5. Za **metodu usmjeravanja promet postavke**, provjerite je li metodu usmjeravanja promet **kružnog**. Ako nije, kliknite **kružnog** na padajućem popisu.
6. Provjerite je li se pravilno konfigurirane **Postavke nadzora** . Nadzor osigurava krajnje točke koje su izvan mreže se šalju promet. Da biste pratili krajnje točke, navedite put i naziv datoteke. Imajte na umu da kosa crta prosljeđivanja "/" valjana stavka za relativni put i podrazumijeva da se datoteka nalazi u korijenskom direktoriju (zadano). Dodatne informacije o nadzoru, potražite u članku [O upravitelju promet nadzor](traffic-manager-monitoring.md).
7. Kada unesete sve promjene konfiguracije, kliknite **Spremi** pri dnu stranice.
8. Da biste testirali promjene u konfiguraciji. Dodatne informacije potražite u članku [Testiranje postavki upravitelj promet](traffic-manager-testing-settings.md).
9. Kada je promet upravitelj profila postavljanje i radu, uređivanje DNS zapisa na DNS poslužitelj mjerodavne naziva domene tvrtke na promet Upravitelj naziva domene. Dodatne informacije o tome kako to učiniti potražite u članku [točke domene tvrtke Internet promet Upravitelj domena](traffic-manager-point-internet-domain.md).

## <a name="next-steps"></a>Daljnji koraci


[Domene tvrtke Internet na promet Upravitelj domena](traffic-manager-point-internet-domain.md)

[Upravitelj promet usmjeravanje metode](traffic-manager-routing-methods.md)

[Konfiguriranje metodu usmjeravanja prebacivanje](traffic-manager-configure-failover-routing-method.md)

[Konfiguriranje metodu usmjeravanja performansi](traffic-manager-configure-performance-routing-method.md)

[Otklanjanje poteškoća promet Upravitelj smanjena stanja](traffic-manager-troubleshooting-degraded.md)

[Promet Upravitelj – onemogućivanje, Omogući ili brisanje profila](disable-enable-or-delete-a-profile.md)

[Promet Upravitelj – onemogućivanje i omogućivanje krajnje točke](disable-or-enable-an-endpoint.md)

