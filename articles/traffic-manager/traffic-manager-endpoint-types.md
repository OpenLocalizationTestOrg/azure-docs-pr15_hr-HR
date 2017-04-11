<properties
    pageTitle="Vrste krajnje točke promet Upravitelj | Microsoft Azure"
    description="U ovom se članku objašnjava različite vrste krajnje točke koje je moguće koristiti s programom Azure promet Manager"
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
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-endpoints"></a>Upravitelj promet krajnje točke

Upravitelj promet za Microsoft Azure omogućuje vam da biste odredili način na koji je mrežni promet raspodijeljen implementacijama aplikacije u drugu podatkovnim centrima. Konfiguriranje svaki implementaciju aplikacije kao 'krajnje' u upravitelju promet. Kada upravitelj promet primi zahtjev DNS-a, odabire krajnje dostupna da biste se vratili u odgovoru DNS-a. Upravitelj promet temelji odabir na trenutni status krajnjoj točki i metodu promet usmjeravanja. Dodatne informacije potražite u članku [Kako funkcionira Upravitelj promet](traffic-manager-how-traffic-manager-works.md).

Postoje tri vrste krajnje točke podržane Upravitelj promet:

- **Azure krajnje točke** koriste se za servise smješten u Azure.
- **Vanjski krajnje točke** koriste se za servise hostira izvan Azure, bilo lokalno ili s nekog drugog davatelja usluge hostinga.
- **Ugniježđene krajnje točke** koriste se za kombiniranje promet upravitelj profila da biste stvorili fleksibilnije promet usmjeravanje sheme za podršku potrebama veće, složenije implementacije.

Postoji bez ograničenja na kako se krajnje točke različitih vrsta kombiniraju u jedan profil Upravitelj promet. Svaki profil mogu sadržavati bilo koje kombinacije vrste krajnje točke.

U sljedećim odjeljcima opisuju svaku vrstu krajnja točka veća detaljniju.

## <a name="azure-endpoints"></a>Azure krajnje točke

Azure krajnje točke koriste se za servise utemeljene na Azure u upravitelju promet. Podržane su sljedeće vrste Azure resursa:

- "Klasični IaaS VMs i PaaS servise u oblaku.
- Web-aplikacije
- Resursi za PublicIPAddress (koji može se povezati s VMs izravno ili putem programa Azure raspoređivača opterećenja). Na publicIpAddress mora imati naziv DNS dodijeljeni koja će se koristiti u profilu Upravitelj promet.

Resursi za PublicIPAddress su resursi Azure Voditelj resursa. Ne postoji u modelu klasični implementacije. Stoga su samo podržane u promet rukovoditelja Voditelj resursa Azure sučelja. Druge vrste krajnje točke podržani su putem upravitelja resursa i model klasični implementacije.

Prilikom korištenja Azure krajnje točke, promet Upravitelj otkriva kada VM za IaaS 'Klasični', servis u oblaku ili web-aplikacijama se zaustavio i rada. Ovaj status prikazuje se status krajnjoj točki. Potražite u članku [Upravitelj promet krajnjoj točki nadzor](traffic-manager-monitoring.md#endpoint-and-profile-status) detalje. Kada se podlozi usluga zaustavi, promet Upravitelj izvođenje provjere stanja krajnja točka ili izravno promet krajnjoj. Nema upravitelja promet naplate događajem prestao instance. Prilikom ponovnog pokretanja servisa naplate životopise i krajnju točku ispunjava uvjete za primanje promet. U ovom otkrivanje ne primjenjuju se na PublicIpAddress krajnje točke.

## <a name="external-endpoints"></a>Vanjski krajnje točke

Vanjski krajnje točke koriste se za servise izvan Azure. Na primjer, servis hostira lokalnog ili s nekog drugog davatelja usluge. Vanjski krajnje točke mogu koristiti pojedinačno ili u istom profilu Upravitelj promet u kombinaciji s Azure krajnje točke. Kombiniranje Azure krajnje točke s vanjskim krajnje točke omogućuje različite scenarije:

- Neki aktivno aktivan "ili" aktivno pasivni prebacivanje modela, koristite Azure povećana zalihosti postojećih lokalnog računala.
- Da biste smanjili Latencija aplikacije za korisnike diljem svijeta, proširivanje postojeće lokalnog aplikacije na dodatne zemljopisnog mjesta u Azure. Dodatne informacije potražite u članku [promet Upravitelj 'Performanse' usmjeravanje prometa](traffic-manager-routing-methods.md#performance-traffic-routing-method).
- Korištenje Azure omogućuju dodatne kapaciteta za postojeći lokalnog računala, neprekidno ili kao rješenje "neprekidnog rada u oblak" da bi odgovarao šiljka u zahtjev.

U određenim slučajevima, korisno je koristi vanjski krajnje točke referentni Azure services (Primjeri, potražite u članku [Najčešća pitanja vezana uz](#faq)). U ovom slučaju provjere stanja se naplatiti brzinom Azure krajnje točke, ne stopa vanjskih krajnje točke. Međutim, za razliku od Azure krajnje točke ako zaustavljanje ili izbrišite podlozi servis stanja provjerite naplata nastavlja dok onemogućivanje i brisanje krajnje točke u upravitelju promet.

## <a name="nested-endpoints"></a>Ugniježđene krajnje točke

Ugniježđene krajnje točke kombinirati više profila Upravitelja promet da biste stvorili fleksibilne promet usmjeravanje sheme i podržava potrebama veće, složene implementacije. S ugniježđene krajnje točke, 'podređeni' profila dodani kao krajnje 'nadređenog' profil. Podređeni i nadređene profili mogu sadržavati druge krajnje točke bilo koje vrste, uključujući drugih ugniježđene profila. Dodatne informacije potražite u članku [ugniježđene promet upravitelj profila](traffic-manager-nested-profiles.md).

## <a name="web-apps-as-endpoints"></a>Web Apps kao krajnje točke

Neke dodatne informacije vrijediti prilikom konfiguriranja web-aplikacijama kao krajnje točke u upravitelju promet:

1. Samo web-aplikacije na 'standardne SKU ili noviji su uvjete za korištenje s Upravitelj promet. Prilikom pokušaja da biste dodali web-aplikacijama od niže SKU neće uspjeti. Prijelaz na stariju SKU postojeće Web App rezultata u promet Upravitelj više vam ne šalje promet u taj Web App.

2. Kada krajnje primi zahtjev HTTP, koristit će zaglavlje 'domaćina' u zahtjevu za da biste utvrdili koje web-aplikacije treba servisni zahtjev. Zaglavlje glavnog računala sadrži naziv DNS-a koji se koristi da pošalju zahtjev, primjerice "contosoapp.azurewebsites.net". Da biste koristili neki drugi naziv DNS-a web-aplikaciju programa, DNS naziv mora biti registriran kao prilagođeni naziv domene za aplikaciju. Prilikom dodavanja krajnjoj točki Web App kao krajnje Azure, DNS naziv profila promet Upravitelj automatski se registrirati za aplikaciju. U ovom Registracija se uklanjaju se automatski nakon brisanja krajnju točku.

3. Svaki profil promet upravitelj može sadržavati najviše jedan Web App krajnje točke sa svakom području Azure. Da biste zaobišli za ovo ograničenje, možete konfigurirati web-aplikacijama kao vanjski krajnjoj točki. Dodatne informacije potražite u članku [Najčešća Pitanja](#faq).

## <a name="enabling-and-disabling-endpoints"></a>Omogućivanje i onemogućivanje krajnje točke

Onemogućivanje krajnje točke u upravitelju promet može biti korisno da biste privremeno uklonili promet s krajnje točke u načinu za održavanje ili se ponovno implementirati. Kada krajnju točku je ponovno pokrenuti, možda ćete ponovno omogućiti.

Krajnje točke mogu se omogućuje i onemogućuje putem upravitelja promet portal PowerShell, EŽA ili REST API-JA, koje su podržane u Voditelj resursa i model klasični implementacije.

>[AZURE.NOTE] Onemogućivanje krajnje Azure ne sadrži ništa učiniti s stanje implementacije u Azure. Programa Azure service (kao što su VM ili Web App ostaje pokrenut i primati promet čak i kad je onemogućen u upravitelju promet. Promet može biti adresirane izravno na instancu servisa, a ne putem naziv DNS Manager promet profila. Dodatne informacije potražite u članku [funkcioniranje Upravitelj promet](traffic-manager-how-traffic-manager-works.md).

Trenutni uvjete potrebno svaki krajnje točke prima promet ovisi o sljedeće čimbenike:

- Status profila (omogućeno/onemogućeno)
- Status krajnju točku (omogućeno/onemogućeno)
- Rezultati stanja provjerava taj krajnje točke

Detalje potražite u članku [Upravitelj promet krajnjoj točki nadzor](traffic-manager-monitoring.md#endpoint-and-profile-status).

>[AZURE.NOTE] Budući da upravitelj promet funkcionira na razini DNS-a, ne može utjecati postojeće veze s bilo kojeg krajnjoj točki. Kada krajnje nije dostupna, promet Upravitelj usmjerava nove veze s drugom dostupna krajnje točke. Međutim, glavno računalo iza krajnju točku onemogućene ili dobro može nastaviti primati promet putem postojeće veze dok su te sesije prekinuti. Aplikacija potrebno ograničiti trajanje sesija da dopušta promet za praznite iz postojeće veze.

Onemogućuju se sve krajnje točke u profilu ili samom profilu je onemogućen, promet Upravitelj šalje je odgovor 'NXDOMAIN' u novi upit DNS-a.

## <a name="faq"></a>NAJČEŠĆA PITANJA

### <a name="can-i-use-traffic-manager-with-endpoints-from-multiple-subscriptions"></a>Možete koristiti upravitelj promet s krajnje točke iz višestruke pretplate?

Korištenje krajnje točke s više pretplata nije moguće u Azure Web Apps. Azure web-aplikacije potreban je da neki prilagođeni naziv domene koristi s web-aplikacijama koristi se samo u jednom pretplate. Nije moguće koristiti web-aplikacije iz višestruke pretplate s istim nazivom domene.

Za ostale vrste krajnje točke moguće je korištenje upravitelja promet krajnje točke s više od jedne pretplate. Kako konfigurirati promet Upravitelj ovisi o tome koristite model klasični implementacije ili rad s resursima.

- U upravitelju resursa krajnje točke s bilo kojeg pretplate možete dodati promet upravitelju dok god osoba konfigurirate zaseban profil Upravitelj promet ima pristup za čitanje krajnjoj. Možete dobiti te dozvole pomoću [Kontrola pristupa na temelju uloga upravitelja Azure resursa (RBAC)](../active-directory/role-based-access-control-configure.md).
- Model sučelja uvođenje klasičnog promet Upravitelj zahtijeva da se servisi u Oblaku ili web-aplikacije konfigurirati kao Azure krajnje točke nalaze u okviru iste pretplate kao upravitelj promet profil. Krajnje točke servisa u oblaku, u druge pretplate za moguće dodavati upravitelju promet kao 'vanjskih krajnje točke. Te vanjskim krajnje točke se naplatiti kao Azure krajnje točke, a ne vanjskih stopa.

### <a name="can-i-use-traffic-manager-with-cloud-service-staging-slots"></a>Možete koristiti upravitelj promet s slobodnih 'pripremna servis u Oblaku?

Da. Servis u oblaku 'pripremna' slobodnih moguće je konfigurirati u upravitelju promet kao vanjski krajnje točke. Provjere stanja se i dalje se naplaćuju brzinom Azure krajnje točke. Budući da je vrstu vanjskog krajnjoj točki koristi, promjene u podlozi servisa ne izdvajaju prema gore automatski. S vanjskim krajnje točke promet Manager ne može otkriti kada je servis u Oblaku prekinuli ili izbrisati. Zbog toga Upravitelj promet nastavlja naplatom za provjere stanja dok krajnju točku je onemogućena ili izbrisati.

### <a name="does-traffic-manager-support-ipv6-endpoints"></a>Podržava li upravitelj promet krajnje točke IPv6?

Upravitelj promet trenutno ne nudi IPv6 addressible poslužitelje naziva. Međutim, Upravitelj promet i dalje mogu koristiti IPv6 klijenti za povezivanje s krajnje točke IPv6. Klijent ne provjerite DNS zahtjeva izravno promet upravitelju. Umjesto toga, klijent koristi rekurzivne DNS servis. Klijent za samo za IPv6 šalje zahtjeva za DNS servis rekurzivne putem protokola IPv6. Zatim servis rekurzivna treba obratiti poslužitelje naziva Upravitelj promet pomoću IPv4.

Upravitelj promet Odgovori s nazivom DNS krajnje točke. Da biste podržali krajnje IPv6, DNS AAAA zapisa koja pokazuje DNS naziv krajnje točke na IPv6 address mora postojati. Upravitelj promet provjere stanja podržavaju samo IPv4 adrese. Da biste otkrili krajnje IPv4 na isti naziv DNS-a mora se servis.

### <a name="can-i-use-traffic-manager-with-more-than-one-web-app-in-the-same-region"></a>Možete koristiti upravitelj promet s više od jedne Web App u istom području?

Obično se koristi Upravitelj promet da biste usmjerili promet aplikacije u uveden u različitim područjima. Međutim, i korištenja gdje aplikacije ima više od jedne implementacije u istom području. Krajnje točke Azure promet Upravitelj ne dopuštaju više od jedne krajnjoj točki web-aplikacije iz iste Azure područja koja će se dodati na isti profil Upravitelj promet.

Sljedeći koraci pružaju zaobilazno rješenje na ovo ograničenje:

1. Provjerite jesu li vaše krajnje točke u drugoj web app 'skaliranje jedinice". Naziv domene mora mapirati na jednog web-mjesta u navedeni skaliranje jedinica. Stoga dva Web Apps u istoj jedinici skaliranje ne možete zajednički koristiti upravitelj promet profila.
2. Dodajte naziv domene vanity kao prilagođeni naziv glavnog računala svaki Web App. Svaki web-aplikacije mora biti u drugoj veličini jedinica. Sve web-aplikacije mora pripadati iste pretplate.
3. Dodajte jednog (i samo jedan) krajnjoj točki web-aplikacije u svoj profil promet Upravitelj kao Azure krajnjoj točki.
4. Dodajte svaki dodatni krajnjoj točki web-aplikacije u svoj profil promet Upravitelj kao vanjski krajnje točke. Vanjski krajnje točke nije moguće dodati pomoću modela implementacije Voditelj resursa.
5. Stvaranje DNS CNAME zapis u vašem domenu koja upućuje na naziv vašeg profila DNS Manager promet (<> …. trafficmanager.net).
6. Pristup web-mjestu putem vanity naziv domene nije promet Upravitelj DNS naziv profila.

## <a name="next-steps"></a>Daljnji koraci

- Saznajte [Kako funkcionira Upravitelj promet](traffic-manager-how-traffic-manager-works.md).
- Informirajte se o upravitelju promet [nadzor krajnjoj točki i automatsko prebacivanje](traffic-manager-monitoring.md).
- Saznajte više o promet Upravitelj [promet usmjeravanje metode](traffic-manager-routing-methods.md).
