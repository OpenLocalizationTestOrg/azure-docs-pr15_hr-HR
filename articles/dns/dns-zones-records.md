<properties 
   pageTitle="DNS zone i zapisi | Microsoft Azure" 
   description="Pregled podrške za hostiranje DNS zone i zapisa u Microsoft Azure DNS." 
   services="dns" 
   documentationCenter="na" 
   authors="jtuliani" 
   manager="carmonm" 
   editor=""/>

<tags
   ms.service="dns"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/17/2016"
   ms.author="jtuliani"/>

# <a name="dns-zones-and-records"></a>DNS zone i zapisa

Ova stranica objašnjava koncepata domena, DNS zone i DNS zapisima i skupove zapisa i način na koji su podržane u Azure DNS-a.

## <a name="domain-names"></a>Nazivi domena
 
Domain Name System je hijerarhiju domena. U hijerarhiji započinje s 'korijensku' domenu, čiji je naziv jednostavno "**.**".  Ispod nje potjecati domena najviše razine, kao što su "com', 'neto', 'ili',"Velika Britanija"ili"jp".  Ispod te je domena druge razine, kao što su 'org.uk' ili 'co.jp'. Domena u hijerarhiji DNS se globalno distribuirali, hostira DNS poslužitelji naziva diljem svijeta.

Registrara naziva domena je tvrtka ili ustanova koja omogućuje kupiti naziv domene, primjerice "contoso.com".  Kupnja naziva domene vam desno da biste odredili hijerarhiji DNS-a u odjeljku taj naziv, primjerice omogućujući vam da biste usmjerili naziv "www.contoso.com" na web-mjesto tvrtke. Na registrara možda hostira domene u vlastiti naziv poslužitelja u vaše ime ili umjesto toga možete navesti alternativni naziv poslužitelja.

Azure DNS-a nudi infrastrukturu poslužitelja globalno raspodijeljeno, visoku dostupnost naziv koji možete koristiti za hostiranje vaše domene. Tako da u kojem se nalazi domene u Azure DNS, možete upravljati DNS zapisima pomoću istih vjerodajnica, API-ji, alata, naplata i podršku kao drugih Azure servisa.

Azure DNS-a trenutno ne podržava Kupnja naziva domena. Ako želite kupiti naziv domene, morate koristiti registrara naziva domena treće strane. Na registrara će obično malu naknadu godišnje. Domena možete pa se nalaziti u Azure DNS za upravljanje DNS zapisima. Detalje potražite u članku [delegat domene Azure DNS-a](dns-domain-delegation.md) .

## <a name="dns-zones"></a>DNS zone 

DNS zone koristi se za hostiranje DNS zapise za određene domene. Da biste započeli hostiranje vaše domene u Azure DNS, morate stvoriti DNS zone za taj naziv domene. Svaki DNS zapis za svoju domenu pa se stvara u ovoj zoni DNS-a. 

Ako, na primjer, domene "contoso.com" može sadržavati nekoliko DNS zapisima, kao što su mail.contoso.com (za poslužitelj pošte) i www.contoso.com (za web-mjesta).

Prilikom stvaranja DNS zone u Azure DNS, naziv zone mora biti jedinstvena u grupu resursa. Isti naziv zone se može ponovno koristiti u grupu različite resursa ili neku drugu pretplatu na Azure. Gdje je više zona zajednički koristili s istim nazivom, svaku instancu dodjeljuje adrese drugi naziv poslužitelja. Samo jedan skup adrese možete konfigurirati registrara naziva domena.

>[AZURE.NOTE] Ne morate vlasnik naziva domene za stvaranje DNS zone taj naziv domene u Azure DNS. Međutim, morate vlasništvo nad domenom da biste konfigurirali Azure DNS poslužitelji naziva kao ispravan naziv poslužitelja za naziv domene kod registrara naziva domena.

Dodatne informacije potražite u članku [delegat domene Azure DNS-a](dns-domain-delegation.md).

## <a name="dns-records"></a>DNS zapisi

### <a name="record-types"></a>Vrsta zapisa

Svaki DNS zapisa koji sadrži naziv i vrstu. Zapisi su organizirane u različitim vrstama prema podataka koje sadrže. Najčešći vrsta je na ' "zapisa, čime se mapira naziv IPv4 adresa. Neku drugu vrstu uobičajeni je 'MX' zapis, koji karata naziv s poslužiteljem pošte.

Azure DNS podržava sve uobičajene vrstama zapisa u DNS: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV i TXT.

### <a name="record-names"></a>Nazivi zapisa

U Azure DNS pomoću relativni naziva određeni su klauzulom zapisa. *Potpuno kvalificirani* naziv domene (FQDN) sadrži naziv zone dok *relativni* naziv ne. Na primjer, relativnih zapisa naziv www u zoni "contoso.com" daje naziv potpuno kvalificiran zapisa "www.contoso.com".

Zapis poslovnog *Vrh* je DNS zapisa na korijenskom (ili *Vrh*) DNS zone. Na primjer, u DNS zone "contoso.com" zapis vrh ima potpuno kvalificiran naziv "contoso.com" (to se katkad naziva *naked* domene).  Po konvencije, naziv relativni '@' se koristi za stvaranje zapisa za vrh.

### <a name="record-sets"></a>Skupove zapisa
Ponekad morati stvoriti više zapisa u DNS-a s naziva i vrste. Ako, na primjer, pretpostavimo da web-mjesta 'www.contoso.com' nalazi se na dvije različite IP adrese. Na web-mjestu zahtijeva dvije različite zapisa, jedan za svaki IP adresa. Ovo je primjer skupa zapisa:

    www.contoso.com.        3600    IN  A   134.170.185.46
    www.contoso.com.        3600    IN  A   134.170.188.221

Azure DNS upravlja DNS zapisima pomoću *postavlja zapis*. Skup zapisa (poznat i kao *resursa* skupu zapisa) je zbirka DNS zapisa u zoni s istim nazivom i su iste vrste. Većina skupove zapisa sadrže jedan zapis, ali Primjeri poput ovoga, u kojem skupu zapisa sadrži više od jednog zapisa nisu neuobičajeno.

Ako, na primjer, pretpostavimo da ste već stvorili A zapis "www" u zoni 'contoso.com', pokazivanjem na IP adresa '134.170.185.46' (prvi zapis gore).  Da biste stvorili drugi zapis koji želite Dodavanje zapisa u postojeći skup zapisa, a ne stvorite novi skup zapisa.

Vrste zapisa SOA i CNAME su iznimke. Standarde DNS-a ne dopuštaju više zapisa s istim nazivom za ove vrste, stoga ove skupove zapisa može sadržavati samo jedan zapis.

### <a name="time-to-live"></a>Vrijeme važenja

Vrijeme Live ili TTL, određuje koliko predmemorirani svaki zapis tako da klijenti prije ponovno mu. U gornjem primjeru, u TTL je 3600 sekundi ili 1 sat.

U Azure DNS na TTL navedena je za skup zapisa, ne i za svaki zapis, da bi jednaku vrijednost koristi se za sve zapise iz tog skup zapisa.  Možete odrediti bilo koja vrijednost TTL između 1 i 2,147,483,647 sekundi.

### <a name="wildcard-records"></a>Zamjenski zapisa

Azure DNS podržava [zapise zamjenskih znakova](https://en.wikipedia.org/wiki/Wildcard_DNS_record). Zamjenski zapisa se vraćaju kao odgovor na bilo koji upit pod nazivom koji se podudaraju (osim ako postoji bliže podudaranje iz skupa zapisa koji nisu zamjenskih). Za sve vrste zapisa osim NS i SOA podržane su skupove zapisa zamjenskih znakova.  

Da biste stvorili skup zapisa zamjenskih znakova, upotrijebite naziv skup zapisa "\*". Osim toga, možete koristiti naziv s "\*"kao njegovu lijevi oznaku, na primjer,"\*.foo".

### <a name="cname-records"></a>CNAME zapisi

CNAME zapisa skupovima ne može postojati zajedno sa druge skupove zapisa s istim nazivom. Na primjer, ne možete stvarate postavljanje relativni naziva "www" CNAME zapis i A zapis relativni naziva "www" u isto vrijeme.

Budući da vrh zone (naziv = ‘@’) uvijek sadrži NS i SOA zapisom skupova koje su stvorene kada je stvorena u zonu, ne možete stvoriti CNAME zapis postavljeno na vrh zone.

Ove ograničenja biste sa standardima DNS i nisu ograničenja Azure DNS-a.

### <a name="ns-records"></a>NS zapise

Skup slogova NS automatski se stvara na vrh svake zone (naziv = ‘@’), i automatski brišu nakon brisanja zone (se ne može izbrisati zasebno).  Možete izmijeniti TTL ovaj skup zapisa, ali ne možete mijenjati zapisa, što je unaprijed konfigurirana da biste se pozvali na poslužitelje naziva Azure DNS dodijeljene zone.

Možete stvarati i brisati druge NS zapise u zoni ne na vrh zone.  To vam omogućuje da konfiguriranje zone podređeni (pogledajte [Delegating poddomene u Azure DNS](dns-domain-delegation.md#delegating-sub-domains-in-azure-dns)).

### <a name="soa-records"></a>SOA zapisa

Skup zapisa SOA automatski se stvara na vrh svake zone (naziv = ‘@’), i automatski brišu nakon brisanja zone.  Zapisi SOA ne može stvoriti ili izbrisati zasebno. 

Možete izmijeniti sva svojstva SOA zapisa osim svojstvo "glavno računalo", koja je unaprijed konfigurirana za upućivanje na naziv poslužitelja ime primarnog nudi Azure DNS-a.

### <a name="spf-records"></a>SPF zapisi

Pošiljatelj pravila Framework (SPF) zapisa koriste se za određivanje koji poslužitelji e-pošte dopušteno slanje e-pošte ime naziv domene navedene.  Ispravna konfiguracija SPF zapisa je važno da biste spriječili primatelji označavanje e-pošte kao 'Bezvrijedna pošta'.

DNS RFCs izvorno uvedena novu vrstu zapisa 'SPF' koji će podržavaju taj scenarij. Za podršku starije poslužitelji naziva, i oni dopušteno korištenje vrste TXT zapisa da biste odredili SPF zapisi.  U ovom dvosmislenosti koji vode zbrku koji je rješava [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1).  To je naveden samo mora biti stvoren pomoću vrsta zapisa TXT SPF zapisa, a izostavljen je vrsta zapisa SPF. 

**SPF zapisi nisu podržane prema Azure DNS i mora biti stvoren pomoću vrsta zapisa TXT.** Zastarjeli vrstu zapisa za SPF nije podržana. Prilikom [uvoza DNS zone datoteka](dns-import-export.md), sve SPF zapise pomoću vrsta zapisa za SPF pretvaraju se u vrstu zapisa TXT. 

### <a name="srv-records"></a>SRV zapisi

[SRV zapisi](https://en.wikipedia.org/wiki/SRV_record) koriste različite servise za određivanje mjesta. Prilikom određivanja SRV zapis u Azure DNS:

- *Servis* i *protokol* morate navesti kao dio naziva skup zapisa, mjestu s podvlake.  Na primjer, "\_sip. \_tcp.name ".  Zapis na vrh zone, nema potrebe da biste odredili '@' u naziv zapisa pomoću funkcije jednostavno servisa i protokol, npr "\_sip. \_tcp ".
- *Prioritet*, *debljinu*, *priključak*i *ciljne* su navedeni kao parametar svaki zapis u skup zapisa. 

## <a name="tags-and-metadata"></a>Oznake i metapodaci

### <a name="tags"></a>Oznaka
Oznake su popis parove vrijednost za naziv i po Azure Voditelj resursa koji se koristi za natpis resurse.  Azure Voditelj resursa koristi oznake da biste omogućili filtriranih prikaza Azure računa, a omogućuje vam da biste postavili pravila na kojoj su potrebni oznake. Dodatne informacije o oznake potražite u članku [Korištenje oznake da biste organizirali Azure resurse](../resource-group-using-tags.md).

Azure DNS podržava pomoću oznaka Voditelj resursa Azure DNS zone resursa.  Ne podržava oznake na DNS zapisa skupovima.

### <a name="metadata"></a>Metapodataka

Kao zamjena za skup zapisa oznake, Azure DNS podržava dodavanje opaski skupove zapisa pomoću 'metapodataka'.  Slično oznake, metapodatke omogućuje pridružiti parove naziv vrijednosti za svaki skup zapisa.  To može biti korisno da biste, primjerice zapis Svrha svaki skup zapisa.  Za razliku od oznake, metapodataka ne može se koristiti za davanje filtrirani prikaz Azure računa, a nije naveden u pravilo Voditelj resursa Azure.

## <a name="limits"></a>Ograničenja

Kada pomoću Azure DNS, primjenjuju se sljedeća ograničenja zadane:

[AZURE.INCLUDE [dns-limits](../../includes/dns-limits.md)] 

## <a name="next-steps"></a>Daljnji koraci

- Da biste počeli koristiti Azure DNS, Saznajte kako [stvoriti DNS zone](dns-getstarted-create-dnszone-portal.md) i [Stvaranje DNS zapisa](dns-getstarted-create-recordset-portal.md).
- Migracija postojeće DNS zone, Saznajte kako [uvoza i datoteka zone DNS-a](dns-import-export.md).
