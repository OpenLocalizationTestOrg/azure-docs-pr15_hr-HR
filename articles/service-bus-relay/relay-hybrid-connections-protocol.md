<properties 
    pageTitle="Azure preusmjeravanja hibridnog veze protokol | Microsoft Azure"
    description="Vodič za protokol veze za hibridno preusmjeravanja zure."
    services="service-bus"
    documentationCenter="na"
    authors="clemensv"
    manager="timlt"
    editor="tysonn" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="sethm" />

# <a name="azure-relay-hybrid-connections-protocol"></a>Azure preusmjeravanja hibridnog veze protokola

Azure preusmjeravanja jedan je od ključne mogućnost stupovima od Bus servisa Azure platforme. "Hibridnog veze" mogućnost web-mjesta u preusmjeravanja novi je sigurna, Otvori protokol proizvod koji se temelji na HTTP i WebSockets.

Ono zamjenjuje bivši, jednako pod nazivom "BizTalk Services" značajka koja je ugrađena na vlasničkih protocol foundation. Integraciju hibridnog veza u servisa Azure aplikacija će se i dalje funkcionirati kao-je.

"Hibridnog veze" omogućuje uspostavljanje dvosmjerni, binarni strujanje komunikaciju između dvije umreženi aplikacije, što znači jednu ili obje strane se može smjestiti iza NATs ili vatrozidi.

Ovaj dokument opisuju klijentsko interakcije s preusmjeravanja hibridnog veze za povezivanje klijente u ga slušatelj i pošiljatelj uloge i kako slušače prihvatiti nove veze.

## <a name="interaction-model"></a>Interakcija modela

Prijenos hibridnog veze povezuje dvije strane unosom rendezvous točke u Azure oblak koji i stranama možete otkrivanje i povezivanje s iz perspektive vlastite mreže. Točke rendezvous se zove "Hibridnog vezu" u ovom i ostala Dokumentacija u API-ji i na portalu za Azure. Krajnja točka servisa hibridnog veze će se nazivaju "servisa" do kraja ovog dokumenta.

Model interakcije leans na nomenclature postavio mnoge druge mrežne API-ji:

Postoji ga slušatelj najprije označava spremnosti za rukovanje dolazne veze, a kasnije ih prihvaća po primitku. Na drugoj strani je povezne klijent koji će se povezati ga slušatelj, očekuje tu vezu da biste prihvatili za uspostavljanje puta dvosmjerni komunikacije. "Povezivanje", "Preslušavanja", "Prihvati" su pronaći ćete na većini socket API-ji istim uvjetima.

Bilo koji povezivati komunikacije model sadrži ili proizvođača provjerite izlazne veze prema servisa krajnje koji stvara "ga slušatelj" i "klijent" kolokvijalne koristi i mogu uzrokovati i druge overloads terminologija; precizno terminologija stoga koristimo za hibridno veze je na sljedeći način:

Programi na obje strane veze nazivaju se "klijent", jer su klijente na servis. Klijent koji čeka i prihvaća veze je "ga slušatelj" ili u se u "ga slušatelj ulozi". Klijent koji pokreće novu vezu prema ga slušatelj putem servisa se zove "pošiljatelj" ili "pošiljatelj ulogu".

## <a name="listener-interactions"></a>Interakcija ga slušatelj

Ga slušatelj sadrži četiri interakcija sa servisom; sve pojedinosti žičani opisana su u nastavku ovog dokumenta u odjeljku referenca.

### <a name="listen"></a>Slušanje

Da biste naznačili spremnosti na servis koji se nalazi na ga slušatelj spreman za prihvaćanje veza, on stvara izlaznog web socket vezu. Veza rukovanja ima naziv veze hibridnog konfiguriran u prostor za naziv preusmjeravanja i sigurnosni token koji confers "Preslušavanja" u taj naziv. Kada servis je prihvatio web socket, Registracija dovršetka i socket utvrđene web se drži aktivnosti kao "kontrola kanal" za omogućivanje sve kasnije interakcije. Servis omogućuje do 25 Istodobni slušače hibridnog veza. Ako postoje 2 ili više aktivni slušače, dolazne veze će biti raspoređen po ih slučajni redoslijedom; sajma raspodjele je zajamčiti.

### <a name="accept"></a>Prihvaćanje

Kad god pošiljatelja otvara novu vezu na servis, servis će odaberite i obavijestiti jedan aktivni slušače za hibridno vezu. U se šalje obavijest da biste ga slušatelj putem kanala open kontrolu kao JSON poruku koja sadrži URL Web socket krajnju točku koja ga slušatelj morate se povezati s prihvaćanja vezu.

URL-a možete i mora se koristiti izravno tako da ga slušatelj bez dodatnih; šifrirana informacija vrijedi samo na kratko vrijeme zapravo za dugo kao što je pošiljatelj naznačiti Pričekajte da se veza se uspostaviti završetka do kraja, ali najviše 30 sekundi. URL se može koristiti samo za jednu vezu uspješan pokušaj. Čim uspostavljanja veze socket Web rendezvous URL-om sve daljnje aktivnosti na ovom Web socket povezivati će se od i do pošiljatelja bez intervencije ili tumačenja servis.

### <a name="renew"></a>Obnavljanje 

Sigurnosni token koje valja koristiti da biste registrirali ga slušatelj i održavanje kanala kontrole mogu isteći dok ga slušatelj je aktivna. Tokena isteka ne utječe na buduće veze, ali uzrokovat će kontrola kanala da biste se prekida servis pri ili odmah nakon izravnih isteka. Gesta "obnavljanje" je JSON poruku da ga slušatelj možete poslati da biste zamijenili token pridružen kanala kontrola tako da se mogu održavati kanala kontrole za prošireni razdoblja.

### <a name="ping"></a>Ping 

Ako kontrolu kanala ostaje neaktivnosti dulje vrijeme intermediaries na putu, kao što su učitavanja balancers ili NATs možda odbaciti TCP vezu. Gesta "ping" izbjegava koji slanjem malu količinu podataka na kanalu za koji podsjeća svi na mreži veze je namijenjeni aktivnosti i služi i kao liveness test da ga slušatelj. Ping ne uspije, kontrola kanala razmatranje nestabilan i ga slušatelj mora se ponovno povezali.

## <a name="sender-interaction"></a>Interakcija pošiljatelja

Pošiljatelj sadrži samo jednu interakciju sa servisom, povezuje.

### <a name="connect"></a>Povezivanje

Gesta "povezivanje" otvara Web socket servis za pružanje naziv veze hibridnog i upozorenje (nije obavezno, ali potrebno po zadanom) sigurnosni token conferring dozvole "Pošalji" u nizu upita. Servis će komunicirati s ga slušatelj na prethodno opisan način i ste ga slušatelj stvoriti rendezvous vezu koja će biti pridružen ovom socket Web. Nakon prihvaćanja Web socket sve daljnje interakcije na webu socket bit će stoga s povezanim ga slušatelj.

## <a name="interaction-summary"></a>Interakcija sažetka

Rezultat funkcije ovaj model interakcije je da pošiljatelj klijent dolazi iz rukovanja s "clean" socket Web koji je povezan s na ga slušatelj i koje treba dodatno preambles ni Priprema. Time se omogućuje gotovo bilo koje postojeće Web socket klijent implementaciju lako prednosti servisa hibridnog veze pomoću unošenju podataka ispravno izgrađene URL-a u sloja za klijent socket svoje Web.

Veza za rendezvous Socket Web koje dohvaća ga slušatelj kroz interakcije Prihvati čist također i možete ljevoruka u bilo koje postojeće Web socket poslužitelja implementaciju s neke minimalnog dodatni apstrakcije razlikuje "Prihvati" operacija na njihove framework lokalne mreže slušače i hibridnog veze udaljene "Prihvati" operacija.

## <a name="protocol-reference"></a>Protokol reference

U ovom se odjeljku opisuju detalje o interakcije protokol prethodno opisan.

Sve veze socket Web vrše na priključak 443 kao nadogradnju iz 1.1 HTTPS koji često izvučene neke framework socket Web ili API-JA. Opis se drži implementaciju neutralni, bez predlaganja određene framework.

## <a name="listener-protocol"></a>Protokol ga slušatelj

Protokol ga slušatelj sastoji se od tri operacije poruke i dva geste veze.

### <a name="listener-control-channel-connection"></a>Veza za kanal ga slušatelj kontrola

Kanal kontrola je otvorena s povezivanja Web socket:

*wss: / / {prostor naziva adresa} /* *$servicebus* */* *hybridconnection /**{path}? sb hc akcije =... & sb hc id =... & sb hc token =... "*

*Prostor naziva adresa* je na potpuno kvalificirani naziv domene Azure preusmjeravanja prostora za naziv koji hostira hibridnog veze, obično obrazac {*myname}. servicebus.windows.net.*

Mogućnosti za parametra niza upita slijede

| Parametarskog        | Obavezan? | Opis                                                                                                                                                                                        |
|--------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc akcija | Da       | Uloga ga slušatelj parametar mora biti **sb hc akcija = preslušavanja**                                                                                                                                |
| {path}       | Da       | Put kodiran URL-om prostor naziva konfiguriranog hibridnog vezu da biste registrirali ovaj ga slušatelj na. Ovaj izraz se dodaje na na fiksno * **$servicebus**/**hybridconnection /*** dio puta. |
| SB hc tokena  | Da\*     | Ga slušatelj navedite na valjana, kodiran URL-om servisa Bus zajednički pristup tokena za prostor naziva ili hibridnog vezu koju confers **preslušali** desno.                                           |
| SB hc id     | ne        | Taj klijent navedeni neobavezno ID omogućuje završetka do kraja dijagnostičkih praćenje.                                                                                                                             |

Veza Web Socket ne uspije zbog put hibridnog vezu u tijeku neregistriran, ili je valjan ili nedostaje token ili neke druge pogreške, povratne informacije o pogrešci objavit ćemo zajedno koristi običan model HTTP 1.1 status povratne informacije. Opis statusa će sadržavati do pogreške praćenje id koji možete prenosi na podršku za Azure:

| Kod | Pogreška          | Opis                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nije pronađen      | Hibridno veze **put** nije valjan ili osnovni URL oblikovan |
| 401  | Neovlašteno   | Sigurnosni token je nedostaju ili oštećen ili nije valjana                  |
| 403  | Zabranjen      | Sigurnosni token nije valjan za ovaj put za ovu akciju          |
| 500  | Interna pogreška | Dogodila se pogreška u servisu                                    |

Ako web-veze socket namjerno isključi servis nakon što ga je prethodno postavljen, razlog za to da će se prenosi pomoću odgovarajuće Web socket protokol Šifra pogreške zajedno s opisni poruka o pogrešci koja će sadržavati i id praćenja. Servis će isključuje kanala kontrola bez nailazi na pogrešku. Sve clean isključivanja je klijent upravlja.

| WS Status | Opis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Put veze za hibridno izbrisane ili je onemogućen.                           |
| 1008      | Sigurnosni token istekla, i pravila autorizacije stoga bi. |
| 1011      | Dogodila se pogreška unutar servisa.                                           |

### <a name="accept-handshake"></a>Prihvaćanje rukovanja 

Prihvaćanje obavijesti se šalje servis ga slušatelj putem kanala prethodno uobičajene kontrole kao JSON poruka u web-socket tekstnog okvira. Postoji bez odgovor na ovu poruku.

Poruka sadrži pod nazivom "prihvatili", objekt JSON koji definira sljedeća svojstva trenutno:

-   **Adresa** – niz URL-a koji će se koristiti za uspostavljanje Socket Web sa servisom za prihvaćanje dolazne veze.

-   **id** – Jedinstveni identifikator za tu vezu. Ako je id koji vam je dao klijent pošiljatelja, to je pošiljatelj unesena vrijednost, inače generira sustav vrijednost.

-   **connectHeaders** – sva zaglavlja HTTP navedene krajnjoj preusmjeravanja pošiljatelj, što uključuje i Sec-WebSocket-protokol i zaglavlja Sec WebSocket proširenja.

| Prihvaćanje poruke                                                                     |
|------------------------------------------------------------------------------------|
```json
{                                                                                  
    "accept" : {                                                                                    
        "address" : "wss://168.61.148.205:443/$servicebus/hybridconnection/{path}?...",                                                                          
        "id" : "4cb542c3-047a-4d40-a19f-bdc66441e736\_G0\_G1",                         
        "connectHeaders" : {                                                                
            "Host" : "…",                                                                       
            "Sec-WebSocket-Protocol" : "…",                                                     
            "Sec-WebSocket-Extensions" : "…"                                                    
        }                                                                                     
    }
}
```

URL adresa navedene u poruci JSON koristi ga slušatelj da biste uspostavili Socket Web za prihvaćanje ili odbijanje socket pošiljatelja.

#### <a name="accepting-the-socket"></a>Prihvaćanje na socket

Da biste prihvatili, ga slušatelj uspostavlja WebSocket vezu na navedene adrese.

Smeta da za razdoblje pretpregled adresu URI možda koristi Gola IP adresa i TLS certifikat koji vam je dao poslužitelj neće uspjeti Provjera valjanosti na tu adresu. To će rectified tijekom pretpregleda.

Ako poruka "prihvatiti" ima naslov "Sec-WebSocket-protokol", očekuje da ga slušatelj samo prihvatiti na WebSocket ako to podržava tu protocol i ga postavlja zaglavlja kao što je pokrenut na WebSocket.

Isto vrijedi za zaglavlje "Sec-WebSocket-proširenja". Ako okvir podržava datotečni nastavak, je odgovor strani *poslužitelj* potrebna rukovanja "Sec-WebSocket-proširenja" za proširenje postavite zaglavlje.

URL mora se koristiti kao-je za uspostavljanje socket prihvati, ali sadrži sljedećih parametara:

| Parametarskog        | Obavezan? | Opis                                                                                                                                                                                                                                                                                   |
|--------------|-----------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc akcija | Da       | Za prihvaćanje krajnju točku veze parametar mora biti **sb hc akcija = prihvatiti**                                                                                                                                                                                                                          |
| {path}       | Da       | Put kodiran URL-om prostor naziva konfiguriranog hibridnog vezu da biste registrirali ovaj ga slušatelj na. Ovaj izraz se dodaje na na fiksno * **$servicebus**/**hybridconnection /*** dio puta.                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The path expression MAY be extended with a suffix and a query string expression that follows the registered name after a separating forward slash.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                           
                            This allows the sender client to pass dispatch arguments to the accepting listener when it is not possible to include HTTP headers.                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                           
                            The expectation is that the listener framework will parse out the fixed path portion and the registered name from the path and make the remainder, possibly without any query string arguments prefixed by “sb-“, available to the application for deciding whether to accept the connection.  
                                                                                                                                                                                                                                                                                                                           
                            For more detail see the “Sender Protocol” section below.                                                                                                                                                                                                                                       |
| SB hc id | No        | U odjeljku opis **id** iznad.                                                                                                                                                                                                                                                              |

Ako je došlo do pogreške, li možda servis odgovori na sljedeći način:

| Kod | Pogreška          | Opis                         |
|------|----------------|-------------------------------------|
| 403  | Zabranjen      | URL-a nije valjana.               |
| 500  | Interna pogreška | Dogodila se pogreška u servisu |

Kada je uspostavljena veza, poslužitelj će se isključiti Web socket prilikom zatvaranja Web socket pošiljatelja prema dolje ili sa statusom sljedeće

| WS Status | Opis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1001      | Klijent pošiljatelja zatvara veze                                        |
| 1001      | Put veze za hibridno izbrisane ili je onemogućen.                           |
| 1008      | Sigurnosni token istekla, i pravila autorizacije stoga bi. |
| 1011      | Dogodila se pogreška unutar servisa.                                           |

#### <a name="rejecting-the-socket"></a>Odbijanje na socket

Odbijanje na socket nakon pregled poruka "prihvatiti" potreban je slično rukovanja tako da se Šifra stanja i opis stanja komunikacije razlog na odbijanja možete tijeka pošiljatelju.

Odabir protokol dizajn ovdje je da biste koristili rukovanja WebSocket (osmišljena da biste završili u stanju pogreške definirani) tako da ga slušatelj klijent implementacije možete nastaviti ovise o WebSocket klijenta i ne morate uključivanja u extra, Gola HTTP klijenta.

Da biste odbacili u krajnju točku veze, klijent uzima adresu URI iz poruke "prihvatiti" i dodaje dva parametra niza upita:

| Parametarskog             | Obavezan? | Opis                             |
|-------------------|-----------|-----------------------------------------|
| kôd statusa        | Da       | Brojčani kod stanja HTTP                |
| statusDescription | Da       | Ljudski čitljiv razlog na odbijanja |

Rezultat URI zatim koristiti uspostaviti vezu WebSocket; ponovno umu certifikat TLS možda odgovara adresi tijekom pretpregleda, pa možda ćete morati onemogućiti provjere valjanosti.

Prilikom dovršetka ispravno, ova rukovanja namjerno neće pripadnog koda pogreške HTTP 410, jer nema WebSocket uspostavljena. Ako se pojavi pogreška, ovo su mogućnosti:

| Kod | Pogreška          | Opis                         |
|------|----------------|-------------------------------------|
| 403  | Zabranjen      | URL-a nije valjana.               |
| 500  | Interna pogreška | Dogodila se pogreška u servisu |

### <a name="listener-token-renewal"></a>Ga slušatelj tokena obnove

Kada na ga slušatelj token uskoro će isteći, to možete zamijeniti tako da pošaljete poruku u okvir s tekstom na servis putem kanala uobičajene kontrole. Poruka sadrži JSON objekt pod nazivom "renewToken", koji definira sljedeće svojstvo trenutno:

-   **tokena** – na valjana, kodiran URL-om servisa Bus zajednički pristup tokena za prostor za naziv ili hibridnog vezu koju confers **preslušali** desno.

| renewToken poruke                                                                                                                                                    
|------------------------------------------------------------------------------------------------|
```json
{
    "renewToken" : {      
        "token" : "SharedAccessSignature sr=http%3a%2f%2fcontoso.servicebus.windows.net%2fHybridConnection1%2f&amp;sig=XXXXXXXXXX%3d&amp;se=1471633754&amp;skn=SasKeyName"
    }                                                                                                                                                            
}
```                                                                                                                                                                      

Ako ne uspije tokena Provjera valjanosti, pristup je zabranjen i u oblaku će se zatvoriti websocket kanala na kontrolu s pogreškom, u suprotnom je nema odgovora.

| WS Status | Opis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1008      | Sigurnosni token istekla, i pravila autorizacije stoga bi. |

<a name="sender-protocol"></a>Protokol pošiljatelja
---------------

Protokol pošiljatelja jednak učinkovito kako je pokrenut na ga slušatelj. Cilj je maksimalna prozirnost Web socket završetka do kraja. Adresa za povezivanje s je ista kao ga slušatelj, ali razlikuje "Akcije" i token mora različite dozvole:

*wss: / / {prostor naziva adresa} /* *$servicebus* */* *hybridconnection /**{path}? sb hc akcije =... & sb hc id =... \[i sbc hc token =... \]*

*Prostor naziva adresa* je na potpuno kvalificirani naziv domene Azure preusmjeravanja prostora za naziv koji hostira hibridnog veze, obično obrazac {*myname}. servicebus.windows.net.*

Zahtjev može sadržavati proizvoljne dodatni HTTP zaglavlja, uključujući one definira aplikacija. Sva zaglavlja navedenom tijekom da biste ga slušatelj i nalazi se na objekt "connectHeader" poruke "prihvatiti" kontrole.

Mogućnosti za parametra niza upita slijede

| Parametarskog        | Obavezan? | Opis                                                                                                                                                                                                       |
|--------------|-----------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| SB hc akcija | Da       | Uloga ga slušatelj parametar mora biti **Akcija = povezivanje**                                                                                                                                                    |
| {path}       | Da       | Put kodiran URL-om prostor naziva konfiguriranog hibridnog vezu da biste registrirali ovaj ga slušatelj na.                                                                                                               
                                                                                                                                                                                                                                               
                            The path expression MAY be extended with a suffix and a query string expression to communicate further                                                                                                             
                                                                                                                                                                                                                                               
                            If the Hybrid Connection is registered under the path “hyco”, the path expression can be “**hyco/**suffix?param=value&…” followed by the query string parameters defined here. A complete expression may then be:  
                                                                                                                                                                                                                                               
                            *wss://{ns}/**$servicebus**/**hybridconnection/*** **hyco/**suffix?param=value*& sb-hc-action=…&sb-hc-id=…\[&sbc-hc-token=…\]*                                                                                     
                                                                                                                                                                                                                                               
                            The path expression is passed through to the listener in the address URI contained in the “accept” control message.                                                                                                |
| SB hc token | Yes\*     | Ga slušatelj navedite na valjana, kodiran URL-om servisa Bus zajednički pristup tokena za prostor za naziv ili hibridnog vezu koju confers desnom **Pošalji** .                                                            | | SB hc id | No        | Neobavezni ID koji omogućuje završetka do kraja dijagnostičkih praćenje i postane dostupna da biste ga slušatelj tijekom rukovanja Prihvati.                                                                                       |

Veza Web Socket ne uspije zbog put hibridnog vezu u tijeku neregistriran, ili je valjan ili nedostaje token ili neke druge pogreške, povratne informacije o pogrešci objavit ćemo zajedno koristi običan model HTTP 1.1 status povratne informacije. Opis statusa će sadržavati do pogreške praćenje id koji možete prenosi na podršku za Azure:

| Kod | Pogreška          | Opis                                                            |
|------|----------------|------------------------------------------------------------------------|
| 404  | Nije pronađen      | Hibridno veze **put** nije valjan ili osnovni URL oblikovan |
| 401  | Neovlašteno   | Sigurnosni token je nedostaju ili oštećen ili nije valjana                  |
| 403  | Zabranjen      | Sigurnosni token nije valjan za ovaj put za ovu akciju          |
| 500  | Interna pogreška | Dogodila se pogreška u servisu                                    |

Ako web-veze socket namjerno isključi servis nakon što ga je prethodno postavljen, razlog za to da će se prenosi pomoću odgovarajuće Web socket protokol Šifra pogreške zajedno s opisni poruka o pogrešci koja će sadržavati i id praćenja.

| WS Status | Opis                                                                        |
|-----------|------------------------------------------------------------------------------------|
| 1000      | Ga slušatelj isključiti na socket.                                                 |
| 1001      | Put veze za hibridno izbrisane ili je onemogućen.                           |
| 1008      | Sigurnosni token istekla, i pravila autorizacije stoga bi. |
| 1011      | Dogodila se pogreška unutar servisa.                                           |

## <a name="next-steps"></a>Sljedeće korake:

- [Prijenos najčešća Pitanja](relay-faq.md)
- [Stvaranje prostor naziva](relay-create-namespace-portal.md)
- [Početak rada s .NET](relay-hybrid-connections-dotnet-get-started.md)
- [Početak rada s čvor](relay-hybrid-connections-node-get-started.md)