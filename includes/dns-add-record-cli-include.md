#### <a name="create-an-a-record-set-with-single-record"></a>Stvorili A zapis postavljen s jednim zapisom

Da biste stvorili skup zapisa, koristite `azure network dns record-set create`. Određivanje grupa resursa, naziv zone zapisa postavite relativni naziv, vrstu zapisa i vrijeme Live (TTL).

    azure network dns record-set create myresourcegroup  contoso.com "test-a"  A --ttl 300

Nakon stvaranja A zapisa skup, dodajte IPv4 adresa zapisa postavite sa `azure network dns record-set add-record`.

    azure network dns record-set add-record myresourcegroup contoso.com "test-a" A -a 192.168.1.1

#### <a name="create-an-aaaa-record-set-with-a-single-record"></a>Stvaranje zapisa AAAA postavljen s jednim zapisom

    azure network dns record-set create myresourcegroup contoso.com "test-aaaa" AAAA --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-aaaa" AAAA -b "2607:f8b0:4009:1803::1005"

#### <a name="create-a-cname-record-set-with-a-single-record"></a>Stvorite CNAME zapis postavljen s jednim zapisom

CNAME zapisi dopustiti samo jednu vrijednost jednog niza.


    azure network dns record-set create -g myresourcegroup contoso.com  "test-cname" CNAME --ttl 300

    azure network dns record-set add-record  myresourcegroup contoso.com  test-cname CNAME -c "www.contoso.com"


#### <a name="create-an-mx-record-set-with-a-single-record"></a>Stvaranje MX zapisa postavite s jednim zapisom

U ovom primjeru se koristi naziv skup zapisa "@" za stvaranje MX zapis na vrh zone (u tom slučaju "contoso.com"). To je uobičajena za MX zapise.

    azure network dns record-set create myresourcegroup contoso.com  "@"  MX --ttl 300

    azure network dns record-set add-record -g myresourcegroup contoso.com  "@" MX -e "mail.contoso.com" -f 5


#### <a name="create-an-ns-record-set-with-a-single-record"></a>Stvaranje NS zapisa postavljen s jednim zapisom

    azure network dns record-set create myresourcegroup contoso.com test-ns  NS --ttl 300

    azure network dns record-set add-record myresourcegroup  contoso.com  "test-ns" NS -d "ns1.contoso.com"

#### <a name="create-a-ptr-record-set-with-a-single-record"></a>Stvaranje PTR zapis postavljen s jednim zapisom  
U ovom slučaju "Moje-arpa-zone.com" predstavlja zone ARPA koji predstavlja vaš raspon IP.  Svaki PTR zapis u ovoj zoni odgovara IP adresa u dosegu IP.    

    azure network dns record-set add-record myresourcegroup my-arpa-zone.com "10" PTR -P "myservice.contoso.com"   

#### <a name="create-an-srv-record-set-with-a-single-record"></a>Stvori SRV zapis postavljen s jednim zapisom

Ako stvarate SRV zapis u korijenskom direktoriju u zonu, možete odrediti "_service" i "_protocol" naziv zapisa. Nema potrebe da biste uključili "@" u naziv zapisa.


    azure network dns record-set create myresourcegroup contoso.com "_sip._tls" SRV --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com  "_sip._tls" SRV -p 0 - w 5 -o 8080 -u "sip.contoso.com"

#### <a name="create-a-txt-record-set-with-single-record"></a>Stvorite TXT zapis postavljen s jednim zapisom

    azure network dns record-set create myresourcegroup contoso.com "test-TXT" TXT --ttl 300

    azure network dns record-set add-record myresourcegroup contoso.com "test-txt" TXT -x "this is a TXT record"
