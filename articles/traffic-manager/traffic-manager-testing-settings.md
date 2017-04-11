<properties
    pageTitle="Testiranje postavke upravitelja promet | Microsoft Azure"
    description="U ovom se članku pronaći ćete testiranje postavki upravitelj promet"
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

# <a name="test-your-traffic-manager-settings"></a>Testirajte postavke upravitelja promet

Testirajte postavke upravitelja promet, morate koristiti više klijenata različitih mjesta, iz koje možete pokrenuti testiranja. Nakon toga premjestiti krajnjih točaka u profilu programa Upravitelj promet dolje jedan po jedan.

* Postavljanje DNS-a TTL vrijednost niskog tako da se promjene Propagiranje brzo (na primjer, 30 sekundi).
* Znati IP adrese servisa Azure oblaka i web-mjesta u profilu koje želite testirati.
* Pomoću alata koji omogućuju Razrješavanje DNS naziva s IP adresom i prikazati tu adresu.

Morate provjeriti da biste vidjeli da DNS naziva riješiti IP adrese krajnje točke u svom profilu. Imena trebali razriješiti na način koji će biti usklađene s metodu usmjeravanja promet definirano u profilu Upravitelj promet. Možete koristiti alate kao što su **nslookup** ili **istražujte** za razrješavanje DNS naziva.

Sljedeći primjeri pomoći testirati promet upravitelj profila.

### <a name="check-traffic-manager-profile-using-nslookup-and-ipconfig-in-windows"></a>Provjera profila Upravitelja promet pomoću alata nslookup i ipconfig u sustavu Windows

1. Otvorite naredbe ili odzivniku komponente Windows PowerShell kao administrator.
2. Vrsta `ipconfig /flushdns` da biste pražnjenje predmemorije za razrješavanje DNS-a.
3. Vrsta `nslookup <your Traffic Manager domain name>`. Na primjer, sljedeća naredba provjerava naziv domene s prefiksom *myapp.contoso*

        nslookup myapp.contoso.trafficmanager.net

    Uobičajeni rezultat prikazuje sljedeće informacije:

    * DNS naziv i IP adresu DNS poslužitelja pristupa da biste riješili ovaj Upravitelj promet naziv domene.
    * Promet Upravitelj naziva domene koje ste naveli u naredbenom retku nakon "nslookup" i IP adresu koja razrješava promet Upravitelj domena. Drugi IP adresa važno je da biste provjerili. Ona mora odgovarati javnu virtualne IP (VIP) adresu za servise u oblaku ili web-mjesta u profilu promet Upravitelj testirate.

## <a name="how-to-test-the-failover-traffic-routing-method"></a>Kako testirati prebacivanje metodu usmjeravanja promet

1. Ostavite sve krajnje točke prema gore.
2. Pomoću jednog klijenta, zahtjev za razrješavanje DNS za naziv domene tvrtke pomoću alata nslookup ili slične utility.
3. Provjerite odgovara li razriješiti IP adresa primarni krajnjoj točki.
4. Premjesti dolje vaš primarni krajnja točka ili uklanjanje nadzora datoteke tako da se taj Upravitelj promet misli aplikacija je prema dolje.
5. Pričekajte da se DNS vrijeme važenja (TTL) profila promet Upravitelj plus dodatne dvije minute. Na primjer, ako je vaš DNS TTL 300 sekundi (5 minuta), morate pričekati sedam minuta.
6. Pražnjenje DNS klijent predmemorije i zahtjev DNS razlučivosti pomoću alata nslookup. U sustavu Windows, možete pražnjenje predmemorije DNS pomoću naredbe ipconfig/flushdns da biste.
7. Provjerite odgovara li razriješiti IP adresu na sekundarnom krajnjoj točki.
8. Ponovite postupak da dolje svaki krajnje točke u nizu. DNS-om Provjerite vraća li IP adresa sljedeći krajnje točke na popisu. Kad su svi krajnje točke prema dolje, trebali biste ponovno dohvaćanje IP adrese primarni krajnje točke.

## <a name="how-to-test-the-weighted-traffic-routing-method"></a>Kako testirati metodu usmjeravanja ponderiranog promet

1. Ostavite sve krajnje točke prema gore.
2. Pomoću jednog klijenta, zahtjev za razrješavanje DNS za naziv domene tvrtke pomoću alata nslookup ili slične utility.
3. Provjerite je li da riješi IP adresa podudara s jednim od vašeg krajnje točke.
4. Pražnjenje predmemorije za klijent DNS, a zatim ponovite korake 2 i 3 za svaki krajnjoj točki. Trebali biste vidjeti različite IP adresa za svaki od vašeg krajnje točke.

## <a name="how-to-test-the-performance-traffic-routing-method"></a>Kako testirati performanse metodu usmjeravanja promet

Da biste učinkovito testirali metodu usmjeravanja performanse promet, morate imati klijenti koja se nalazi u različitim dijelovima svijeta. Možete stvoriti klijente u različitim područjima Azure koje je moguće koristiti za testiranje servisa. Ako imate globalni mrežu, možete daljinski prijavite se na klijente na druge dijelove svijeta i izvođenje testiranja iz nje.

Osim toga, postoje besplatni utemeljen na webu traženje DNS i istražujte usluge dostupne. Neki od tih alata vam mogućnost da biste provjerili Razrješavanje DNS naziva iz različitih mjesta diljem svijeta. Izvedite pretraživanje "DNS traženje" primjere. Servisa drugih proizvođača, kao što su Gomez ili glavni može se koristiti da biste potvrdili vašeg profila su distribucija promet prema očekivanjima.

## <a name="next-steps"></a>Daljnji koraci

* [O upravitelju promet promet usmjeravanje metode](traffic-manager-routing-methods.md)
* [Razmatranja promet Upravitelj performansi](traffic-manager-performance-considerations.md)
* [Otklanjanje poteškoća promet Upravitelj smanjena stanja](traffic-manager-troubleshooting-degraded.md)




