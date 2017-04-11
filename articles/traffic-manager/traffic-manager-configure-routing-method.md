<properties
    pageTitle="Konfiguriranje upravitelja promet usmjeravanje metode | Microsoft Azure"
    description="U ovom se članku objašnjava kako konfigurira različite metode usmjeravanje u upravitelju promet"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="sewhee" />
<!-- repub for nofollow -->

# <a name="configure-traffic-manager-routing-methods"></a>Konfiguriranje upravitelja promet usmjeravanje metode

Azure promet Upravitelj pruža tri načina usmjeravanja koja određuju kako se promet usmjeren krajnje točke raspoloživ servis. Način promet usmjeravanje primjenjuje se na svaki upit DNS primili da biste utvrdili koji krajnjoj točki bi trebala biti vraćena kao odgovor DNS-a.

Dostupne u upravitelju promet su tri načina za usmjeravanje prometa:

- **Prioritet:** Odaberite "Prioritet" kada želite koristiti krajnja točka za primarni servisa i navedite sigurnosnih kopija u slučaju primarni nije dostupna.
- **Težinski faktor:** Odaberite "težinski faktor" kada želite distribuirati promet u skupu rezultata krajnje točke, ili ravnomjerno ili prema debljine koji definiraju.
- **Performanse:** Odaberite "Performanse" kada imate krajnje točke u različitim zemljopisna mjesta, a želite krajnjim korisnicima omogućuje korištenje "najbliže" krajnjoj točki pomoću najniže latenciju mreže.

## <a name="configure-priority-routing-method"></a>Konfiguriranje metodu usmjeravanja prioritet

Bez obzira na način web-mjesto web-mjesta Azure već funkcionalnosti prebacivanje za web-mjesta unutar podatkovnog centra (poznat i kao regija). Upravitelj promet omogućuje prebacivanje za web-mjesta u različitim podatkovnim centrima.

Uobičajeni obrazac za prebacivanje servis je promet slati primarni servisa i prikazuju skup identične sigurnosne kopije servise za prebacivanje. Sljedeći koraci objašnjavaju kako konfigurirati ovaj prioritet prebacivanje s servise u oblaku Azure i web-mjesta:

1. Azure klasični portalu u lijevom oknu kliknite ikonu **Upravitelj promet** da biste otvorili okno Upravitelj promet.
2. U oknu Upravitelj promet na portalu za Azure klasični pronađite promet Upravitelj profil koji sadrži postavke koje želite izmijeniti. Da biste otvorili stranicu Postavke profila, kliknite strelicu s desne strane naziva profila.
3. Na stranici profila kliknite **krajnje točke** pri vrhu stranice. Provjerite jesu li servisi u oblaku i web-mjesta koja želite uvrstiti u konfiguraciji prezentacija.
4. Kliknite **Konfiguriraj** pri vrhu da biste otvorili stranicu konfiguracije.
5. **Način postavke za usmjeravanje prometa**, provjerite je li metodu usmjeravanja promet **Prebacivanje**. Ako nije, kliknite **Prebacivanje** s padajućeg popisa.
6. Za **Prebacivanje prioritet popis**Prilagodba redoslijeda prebacivanje za vaše krajnje točke. Kad odaberete **Prebacivanje** metodu usmjeravanja promet, redoslijed odabranih krajnje točke zapise. Primarni krajnje je na vrhu. Pomoću gore i dolje strelice da biste promijenili redoslijed prema potrebi. Informacije o postavljanju prioritet prebacivanje pomoću komponente Windows PowerShell potražite u članku [Postavljanje AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Provjerite je li se pravilno konfigurirane **Postavke nadzora** . Nadzor osigurava krajnje točke koje su izvan mreže se šalju promet. Da biste pratili krajnje točke, navedite put i naziv datoteke. A kosa crta "/" valjana stavka za relativni put i podrazumijeva da se datoteka nalazi u korijenskom direktoriju (zadano).
8. Kada unesete sve promjene konfiguracije, kliknite **Spremi** pri dnu stranice.
9. Da biste testirali promjene u konfiguraciji.
10. Kada upravitelj promet profila radi uređivanje DNS zapisa na DNS poslužitelj mjerodavne naziva domene tvrtke na promet Upravitelj naziva domene.

## <a name="configure-weighted-routing-method"></a>Konfiguriranje ponderiranog metodu usmjeravanja

Uobičajeni obrazac za promet usmjeravanje način je da biste naveli skup identične krajnje točke, što obuhvaća servise u oblaku i web-mjesta, i poslali promet za svaki unos u kružnog način. Sljedeći koraci strukture kako konfigurirati ovu vrstu metodu usmjeravanja promet.

>[AZURE.NOTE] Azure web-mjesta već sadrže funkcije za web-mjesta unutar podatkovnog centra (poznat i kao regija) za ujednačavanje opterećenja kružnog dodjeljivanja. Upravitelj promet omogućuje vam da navedete metodu usmjeravanja kružnog promet za web-mjesta u različitim podatkovnim centrima.

1. Azure klasični portalu u lijevom oknu kliknite ikonu **Upravitelj promet** da biste otvorili okno Upravitelj promet.
2. U oknu promet crtežima pronađite promet Upravitelj profil koji sadrži postavke koje želite izmijeniti. Da biste otvorili stranicu Postavke profila, kliknite strelicu s desne strane naziva profila.
3. Na stranici za svoj profil, kliknite **krajnje točke** pri vrhu stranice, a zatim provjerite jesu li krajnje točke servisa koji želite uvrstiti u konfiguraciji prezentacija.
4. Na stranici profila kliknite **Konfiguriraj** pri vrhu da biste otvorili stranicu konfiguracije.
5. Za **metodu usmjeravanja promet postavke**, provjerite je li metodu usmjeravanja promet **kružnog**. Ako nije, kliknite **kružnog** na padajućem popisu.
6. Provjerite je li se pravilno konfigurirane **Postavke nadzora** . Nadzor osigurava krajnje točke koje su izvan mreže se šalju promet. Da biste pratili krajnje točke, navedite put i naziv datoteke. A kosa crta "/" valjana stavka za relativni put i podrazumijeva da se datoteka nalazi u korijenskom direktoriju (zadano).
7. Kada unesete sve promjene konfiguracije, kliknite **Spremi** pri dnu stranice.
8. Da biste testirali promjene u konfiguraciji.
9. Kada upravitelj promet profila radi uređivanje DNS zapisa na DNS poslužitelj mjerodavne naziva domene tvrtke na promet Upravitelj naziva domene.

## <a name="configure-performance-traffic-routing-method"></a>Konfiguriranje način za usmjeravanje prometa performanse

Performanse metodu usmjeravanja promet omogućuje da biste usmjerili promet krajnjoj s najmanje Latencija iz klijentskog programa mreže. Obično s podatkovnim centrom s najmanje Latencija se na najbližu geografske udaljenost. Ovu metodu usmjeravanja promet ne računa za promjene u stvarnom vremenu u mrežnoj konfiguraciji ili učitavanje.

1. Azure klasični portalu u lijevom oknu kliknite ikonu **Upravitelj promet** da biste otvorili okno Upravitelj promet.
2. U oknu promet crtežima pronađite promet Upravitelj profil koji sadrži postavke koje želite izmijeniti. Da biste otvorili stranicu Postavke profila, kliknite strelicu s desne strane naziva profila.
3. Na stranici za svoj profil, kliknite **krajnje točke** pri vrhu stranice, a zatim provjerite jesu li krajnje točke servisa koji želite uvrstiti u konfiguraciji prezentacija.
4. Na stranici za svoj profil, kliknite **Konfiguriraj** pri vrhu da biste otvorili stranicu konfiguracije.
5. **Način postavke za usmjeravanje prometa**, provjerite je li metodu usmjeravanja promet * *performansi*. Ako nije, kliknite * *performanse** na padajućem popisu.
6. Provjerite je li se pravilno konfigurirane **Postavke nadzora** . Nadzor osigurava krajnje točke koje su izvan mreže se šalju promet. Da biste pratili krajnje točke, navedite put i naziv datoteke. A kosa crta "/" valjana stavka za relativni put i podrazumijeva da se datoteka nalazi u korijenskom direktoriju (zadano).
7. Kada unesete sve promjene konfiguracije, kliknite **Spremi** pri dnu stranice.
8. Da biste testirali promjene u konfiguraciji.
9. Kada upravitelj promet profila radi uređivanje DNS zapisa na DNS poslužitelj mjerodavne naziva domene tvrtke na promet Upravitelj naziva domene.

## <a name="next-steps"></a>Daljnji koraci

* [Upravljanje profilima Upravitelj promet](traffic-manager-manage-profiles.md)
* [Upravitelj promet usmjeravanje metode](traffic-manager-routing-methods.md)
* [Testiranje postavke upravitelja promet](traffic-manager-testing-settings.md)
* [Domene tvrtke Internet na promet Upravitelj domena](traffic-manager-point-internet-domain.md)
* [Upravljanje promet Upravitelj krajnje točke](traffic-manager-manage-endpoints.md)
* [Otklanjanje poteškoća promet Upravitelj smanjena stanja](traffic-manager-troubleshooting-degraded.md)
