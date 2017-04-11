## <a name="what-is-reverse-dns"></a>Što je obrnutim DNS-a?

Običan DNS zapisi omogućiti mapiranje iz naziv DNS (kao što je 'www.contoso.com') IP adresa (kao što je 64.4.6.100).  Obrnutom DNS omogućuje prevođenje IP adrese (64.4.6.100) ime ('www.contoso.com').

Obrnuti DNS zapisi se koriste u raznim situacijama. Na primjer, obrnutim DNS zapise široko koriste u combating neželjene e-pošte provjerom pošiljatelja poruke e-pošte.  Primanje poslužitelja e-pošte će dohvatiti obrnutim DNS zapis IP adresa pošiljatelja poslužitelja i provjerite ako su koja hostiraju ovlašteni za slanje e-pošte iz izvorišne domene. (Imajte Međutim taj [Azure izračunati services ne podržavaju slanje poruke e-pošte da biste vanjskih domena](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/).)

## <a name="how-reverse-dns-works"></a>Kako obrnutim funkcioniranja DNS-a

Obrnuti DNS zapisa nalaze se u posebno DNS zone naziva 'ARPA' zone.  Te zone obrasca zaseban hijerarhije DNS paralelno s uobičajene hijerarhije hosting domene kao što su "contoso.com".

Ako, na primjer, u DNS zapisa 'www.contoso.com' je implementirati pomoću zapisa DNS "A" s nazivom "www" u zonu "contoso.com".  Ovaj se zapis odgovora upućuje na odgovarajuće IP adresa u ovom slučaju 64.4.6.100.  Obrnuto pretraživanje je implementirati zasebno, pomoću 'PTR' zapis pod nazivom "100" u zoni '6.4.64.in-addr.arpa' (Napominjemo da se IP adrese poništiti u ARPA zone).  PTR zapis ako je konfiguriran ispravno upućuje na naziv "www.contoso.com".

Dodjele tvrtkom ili ustanovom IP adresnog bloka, oni također: D5 pravo da upravljaju odgovarajuće ARPA zone. Zone ARPA odgovara adresni blokovi IP koristi Azure su hostira i upravlja Microsoft. Davatelj internetskih usluga možda vođenje ARPA zone za IP adresama umjesto vas, ili možda mogli hostirati zonu ARPA u DNS servis po izboru, kao što je Azure DNS.

>[AZURE.NOTE] Prosljeđivanje DNS pretraživanja i obrnutim DNS pretraživanja primjenjuju se u zasebne, paralelno DNS hijerarhije. Obrnuto pretraživanje za 'www.contoso.com' se **ne** nalaze u zone 'contoso.com', umjesto toga ono je hostirano u zoni ARPA za odgovarajuće IP Adresni blok.

Dodatne informacije o obrnutim DNS potražite [Obrnuto pretraživanje DNS-a](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).

## <a name="azure-support-for-reverse-dns"></a>Azure podrška za obrnutim DNS-a

Azure podržava dva zasebna scenarija koji se odnose da biste Zrcalili DNS:

1. U kojem se nalazi u zonu ARPA koja odgovara vašem IP Adresni blok.
2. Omogućujući vam da biste konfigurirali obrnutom DNS zapis za IP adresa dodijeljena Azure service.

Za podršku DNS bivši, Azure može se koristiti hostirati zona na ARPA i upravljanja zapisima PTR svaki obrnutom DNS-a pretraživanja.  Postupak stvaranja ARPA zone, postavite na delegiranje i konfiguriranje PTR zapisa je kao običan zone DNS-a.  Samo razlike među njima da na delegiranje mora biti konfigurirana putem davatelja internetskih usluga umjesto registrar DNS-a, a želite koristiti samo PTR vrstu zapisa.

Za podršku drugu mogućnost, Azure omogućuje vam da biste konfigurirali obrnuto pretraživanje za IP adrese dodijeliti usluge.  U ovom obrnuto pretraživanje konfigurirao Azure kao PTR zapis u zoni odgovarajuće ARPA.  Te zone ARPA odgovara svi rasponi IP koristi Azure, su hostira tvrtka Microsoft. **Ostatak u ovom se članku opisuje scenarij detaljno.**
