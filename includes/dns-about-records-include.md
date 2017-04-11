## <a name="about-records"></a>Zapisi

Svaki DNS zapisa koji sadrži naziv i vrstu. Zapisi su organizirane u različitim vrstama prema podataka koje sadrže. Najčešći vrsta je je "" zapisa, čime se mapira naziv IPv4 adresa. Druga vrsta je "MX" zapis, koji karata naziv s poslužiteljem pošte.

Azure DNS podržava sve uobičajene vrstama zapisa u DNS, uključujući odgovora, AAAA, CNAME, MX, NS, PTR, SOA, SRV i TXT. Imajte na umu da:
- SOA zapisa skupovima se stvaraju automatski svake zone, ne mogu se stvoriti zasebno.
- SPF zapisi moraju biti stvoren pomoću vrsta zapisa TXT. Dodatne informacije potražite u članku [ovu stranicu](http://tools.ietf.org/html/rfc7208#section-3.1).

U Azure DNS pomoću relativni naziva određeni su klauzulom zapisa. Naziv "potpuno kvalificiran" domene (FQDN) sadrži naziv zone dok "relativni" naziv ne. Na primjer, relativni zapisa naziv "www" u zoni "contoso.com" daje www.contoso.com potpuno kvalificiran naziv zapisa.

## <a name="about-record-sets"></a>O zapisa skupovima

Ponekad morati stvoriti više zapisa u DNS-a s naziva i vrste. Ako, na primjer, pretpostavimo da web-mjesta "www.contoso.com" nalazi se na dvije različite IP adrese. Na web-mjestu zahtijeva dvije različite zapisa, jedan za svaki IP adresa. Ovo je primjer skupa zapisa:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS upravlja DNS zapisima pomoću skupove zapisa. Skup zapisa je skup DNS zapisa u zoni koji imaju isti naziv i imaju iste vrste. Većina skupove zapisa sadrže jedan zapis, ali Primjeri poput ovoga, u kojem skupu zapisa sadrži više od jednog zapisa nisu neuobičajeno.

SOA i CNAME zapisa skupovima su iznimke. Standardima DNS-a ne dopuštaju više zapisa s istim nazivom za ove vrste.

Vrijeme Live ili TTL, određuje koliko predmemorirani svaki zapis tako da klijenti prije ponovno mu. U ovom se primjeru u TTL je 3600 sekundi ili 1 sat. Za TTL navedena je za skup zapisa, ne i za svaki zapis tako da je jednaku vrijednost koristi za sve zapise iz tog skup zapisa.

#### <a name="wildcard-record-sets"></a>Skupovi zapis zamjenskih znakova

Azure DNS podržava [zapise zamjenskih znakova](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Ove se vraćaju za bilo koji upit s odgovarajućom nazivom (osim ako postoji bliže podudaranje iz skupa zapisa koji nisu zamjenskih). Zamjenski zapisa skupovima podržani su za sve vrste zapisa osim NS i SOA.  

Da biste stvorili skup zapisa zamjenskih znakova, upotrijebite naziv skup zapisa "\*". Ili, upotrijebite naziv s oznakom "\*", na primjer,"\*.foo".

#### <a name="cname-record-sets"></a>CNAME zapisa skupovima

CNAME zapisa skupovima ne može postojati zajedno sa druge skupove zapisa s istim nazivom. Na primjer, ne možete stvarate postavljanje relativni naziva "www" CNAME zapis i A zapis relativni naziva "www" u isto vrijeme. Budući da vrh zone (naziv = ‘@’) uvijek sadrži NS i SOA zapisom skupovi koji su stvoreni kada je stvorena u zonu, ne možete stvoriti CNAME zapis postavljeno na vrh zone. Ove ograničenja dođe do sa standardima DNS-a i ne ograničenja Azure DNS-a.
