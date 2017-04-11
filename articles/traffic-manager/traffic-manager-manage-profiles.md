<properties
    pageTitle="Upravljanje profilima Azure promet Upravitelj | Microsoft Azure"
    description="U ovom se članku olakšava stvaranje onemogućivanje, omogućite, brisanje i prikaz povijesti Azure promet upravitelj profila."
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="manage-an-azure-traffic-manager-profile"></a>Upravljanje profil programa Upravitelj promet Azure

Promet upravitelj profila pomoću metode promet usmjeravanje upravljajte raspodjele promet na servise u oblaku ili web-mjesto krajnje točke. U ovom se članku objašnjava kako stvoriti i upravljanje ove profile.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Stvaranje profila Upravitelja promet pomoću brzo stvaranje

Upravitelj promet profil možete brzo stvoriti pomoću brzo stvaranje Azure klasični portalu. Brzo stvaranje omogućuje stvaranje profila s osnovni konfiguracijske postavke. Međutim, nije moguće koristiti brzo stvaranje postavke kao što je skup krajnje točke (servise u oblaku i web-mjesta), redoslijed prebacivanje za prebacivanje metodu usmjeravanja promet ili postavke nadzora. Nakon stvaranja profila, možete konfigurirati postavke na portalu za Azure klasični. Upravitelj promet podržava do 200 krajnje točke po profilu. Međutim, većina scenariji za korištenje potreban je samo nekoliko krajnje točke.

### <a name="to-create-a-traffic-manager-profile"></a>Stvaranje profila Upravitelja promet

1. **Implementacija servisa u oblaku i web-mjesta radnom okruženju.** Dodatne informacije o servise u oblaku, potražite u članku [Servisi u Oblaku](http://go.microsoft.com/fwlink/p/?LinkId=314074). Dodatne informacije o web-mjesta potražite u članku [web-mjesta](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Prijavite se na portal Azure klasični.** Kliknite **Novo** u donjem lijevom kutu portalu, kliknite **mrežnim servisima > promet Upravitelj**, a zatim kliknite **Brzo stvaranje** da biste započeli s konfiguracijom profila.
3. **Konfiguriranje DNS prefiks.** Dajte upravitelj profila promet jedinstveni DNS naziv prefiksa. Možete navesti prefiks za promet Upravitelj naziva domene.
4. **Odaberite pretplatu.** Odaberite odgovarajući Azure pretplatu. Svaki profil povezan je s jednom pretplate. Ako imate samo jedan pretplatu, ovo ne prikazuje mogućnost.
5. **Odaberite način usmjeravanje prometa.** Odaberite način usmjeravanje prometa u **promet usmjeravanje pravila**. Dodatne informacije o načina za usmjeravanje prometa potražite u članku [o upravitelju promet promet usmjeravanje metode](traffic-manager-routing-methods.md).
6. **Kliknite "Stvori" da biste stvorili profil**. Po dovršetku konfiguracije profila svoj profil možete pronaći u oknu crtežima promet na portalu za Azure klasični.
7. **Na portalu za Azure klasični konfigurirati krajnje točke, nadzor i dodatne postavke.** Samo pomoću brzo stvaranje konfigurira osnovne postavke. Je potrebno da biste konfigurirali dodatne postavke kao što je popis krajnjih točaka i redoslijed prebacivanje krajnjoj točki.


## <a name="disable-enable-or-delete-a-profile"></a>Onemogućivanje, omogućite ili brisanje profila

Postojeći profil možete onemogućiti tako da se taj promet Upravitelj ne odnosi zahtjevima korisnika konfigurirani krajnje točke. Kada onemogućite promet upravitelj profila, profil i informacije koje se nalaze u profilu ostaju očuvani i može se urediti u sučelju Upravitelj promet.  Kada ponovno omogućite profil za pisanje životopisa referenci. Kada stvorite profil Upravitelj promet na portalu za Azure klasični, on je automatski omogućena. Ako odlučite da profil više nije potrebna, možete je izbrisati.

### <a name="to-disable-a-profile"></a>Da biste onemogućili profila

1. Ako koristite prilagođeni naziv domene, promijenite CNAME zapis na Internet DNS poslužitelju tako da više ne pokazuje na promet upravitelj profila.
2. Promet zaustavlja se usmjereni na krajnje točke putem upravitelja promet postavke profila.
3. Odaberite profil koji želite onemogućiti. Na stranici upravitelja promet istaknite profil tako da kliknete stupac pokraj naziva profila. Bilješke, a zatim kliknete naziv profila ili strelicu pokraj naziva otvorit će se na stranicu Postavke profila.
4. Nakon odabira profila, pritisnite **onemogućiti** pri dnu stranice.

### <a name="to-enable-a-profile"></a>Da biste omogućili profila

1. Odaberite profil koji želite onemogućiti. Na stranici upravitelja promet istaknite profil tako da kliknete stupac pokraj naziva profila. Bilješke, a zatim kliknete naziv profila ili strelicu pokraj naziva otvorit će se na stranicu Postavke profila.
2. Nakon odabira profila, kliknite **Omogući** pri dnu stranice.
3. Ako koristite prilagođeni naziv domene, stvorite CNAME zapis resursa na poslužitelj za Internet DNS da upućuje na naziv domene promet upravitelj profila.
4. Promet se preusmjereni na krajnjih točaka ponovno.

### <a name="to-delete-a-profile"></a>Da biste izbrisali profila

1. Provjerite je li DNS zapisa resursa na Internet DNS poslužitelj više ne koristi CNAME zapis za resursa koji upućuje na naziv domene promet upravitelj profila.
2. Odaberite profil koji želite onemogućiti. Na stranici upravitelja promet istaknite profil tako da kliknete stupac pokraj naziva profila. Bilješke, a zatim kliknete naziv profila ili strelicu pokraj naziva otvorit će se na stranicu Postavke profila.
3. Nakon odabira profila, kliknite **Izbriši** pri dnu stranice.

## <a name="view-traffic-manager-profile-change-history"></a>Povijest promjena za prikaz promet upravitelja profila

Možete pogledati povijest promjena za svoj profil promet Upravitelj Azure klasični portal u Management Services.

### <a name="to-view-your-traffic-manager-change-history"></a>Da biste prikazali povijest promjena upravitelju promet

1. U lijevom oknu portala za Azure klasični kliknite **Upravljanje servisima**.
2. Na stranici Upravljanje uslugama kliknite **Zapisnik postupka**.
3. Na stranici zapisnik postupka možete filtrirati da biste prikazali povijest promjena za svoj profil Upravitelj promet. Nakon odabira mogućnosti filtriranja, kliknite kvačicu da biste vidjeli rezultate.

   - Da biste prikazali promjene za sve profile, odaberite svoju pretplatu i vremenskog razdoblja, a zatim **Upravitelj promet** na izborniku prečaca **Vrsta** .
   - Da biste filtrirali prema naziv profila, upišite naziv profila u polje **Naziv usluge** , ili ga odaberite na izborniku prečaca.
   - Da biste pogledali detalje o svakom pojedinačnih promjena, odaberite redak s promjenom koje želite prikazati, a zatim **Detalji** pri dnu stranice. U prozoru **Pojedinosti operacije** možete pogledati XML predstavljanje API objekt koji je Stvori ili ažurira u sklopu postupka.

## <a name="next-steps"></a>Daljnji koraci

- [Dodavanje krajnje točke](traffic-manager-endpoints.md)
- [Konfiguriranje metodu usmjeravanja prebacivanje](traffic-manager-configure-failover-routing-method.md)
- [Konfiguriranje metodu usmjeravanja kružnog](traffic-manager-configure-round-robin-routing-method.md)
- [Konfiguriranje metodu usmjeravanja performansi](traffic-manager-configure-performance-routing-method.md)
- [Domene tvrtke Internet na promet Upravitelj naziva domene](traffic-manager-point-internet-domain.md)
- [Otklanjanje poteškoća promet Upravitelj smanjena stanja](traffic-manager-troubleshooting-degraded.md)