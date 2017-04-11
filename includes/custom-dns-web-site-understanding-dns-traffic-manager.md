Sustav naziva domena (DNS) koristi se za pronalaženje značajki na Internetu. Ako, na primjer, kada unesite adresu u pregledniku ili kliknite vezu na web-stranici, koristi DNS za domenu Prevedite IP adresa. IP adresa je sort programa kao što su poštansku adresu, ali nije vrlo Ljudski prikladnu. Na primjer, je mnogo lakše zapamtiti DNS naziv kao što je **contoso.com** nego što je zapamtiti IP adresu kao što su 192.168.1.88 ili 2001:0:4137:1f67:24a2:3888:9cce:fea3.

U DNS sustavu temelju *zapisa*. Zapisi pridružiti određene *naziv*, primjerice **contoso.com**IP adresa ili neki drugi naziv DNS-a. Kada aplikacija, kao što je web-preglednik, pretražuje ime i prezime u DNS-a, pronalazi zapis i koristi neku drugu pokazuje na kao adresu. Ako je vrijednost upućuje na IP adresu, web-pregledniku će koristiti tu vrijednost. Ako se pokazuje na neki drugi naziv DNS-a, zatim aplikacija ima da biste ponovno učinili razlučivost. Konačni će sve razlučivanje naziva završavaju s IP adresa.

Kada stvorite web-mjesto programa Azure, DNS naziva se automatski dodjeljuju web-mjesto. Naziv se sastoji od ** &lt;yoursitename&gt;. azurewebsites.net**. Kada dodate web-mjesta kao krajnje Upravitelj promet Azure, web-mjesto pa se može pristupiti putem u ** &lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domene.

> [AZURE.NOTE] Kada web-mjesto je konfiguriran kao krajnja točka Upravitelj promet, će koristiti u **. trafficmanager.net** adresa prilikom stvaranja DNS zapisa.

> Možete koristiti samo CNAME zapisa pomoću upravitelja promet

Postoje i više vrsta zapisa, svaka s vlastite funkcije i ograničenja, ali za web-mjesta kao upravitelj promet krajnje točke konfigurirano tako da, ne možemo samo brigu o nekom; *CNAME* zapisa.

###<a name="cname-or-alias-record"></a>Zapis CNAME ili pseudonim

CNAME zapis mapira *DNS naziv, kao što su **mail.contoso.com** ili **www.contoso.com**,* naziv druge domene (Kanonski). U slučaju Azure web-mjesta pomoću upravitelja promet je naziv Kanonski domene na ** &lt;myapp >. trafficmanager.net** naziv domene promet upravitelj profila. Nakon stvaranja, stvara se CNAME pseudonima za u ** &lt;myapp >. trafficmanager.net** naziv domene. Unos CNAME će riješiti IP adresu na ** &lt;myapp >. trafficmanager.net** naziva domene automatski u pa ako se promijeni IP adresu web-mjesta, nije potrebno ništa poduzimati.

Kad stigne promet na promet Upravitelj ga zatim usmjerava promet na web-mjestu pomoću metode je konfiguriran za za ujednačavanje opterećenja. To je potpuno proziran posjetitelji web-mjesta. Prikazat će se samo naziv prilagođene domene u njihovu pregledniku.

> [AZURE.NOTE] Neke registrara domena samo omogućuju da biste mapirali poddomene prilikom korištenja CNAME zapis, kao što je **www.contoso.com**, a ne korijenski naziva, primjerice **contoso.com**. Dodatne informacije o CNAME zapisa potražite u dokumentaciji registrar, <a href="http://en.wikipedia.org/wiki/CNAME_record">stavka Wikipedije CNAME zapis</a>ili <a href="http://tools.ietf.org/html/rfc1035">IETF domenama – implementaciji i specifikacija</a> dokumenta.
