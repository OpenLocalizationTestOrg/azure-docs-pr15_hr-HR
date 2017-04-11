## <a name="load-balancer-differences"></a>Razlike raspoređivača učitavanja

Dostupne su različite mogućnosti za raspodjelu mrežni promet koristeći Microsoft Azure. Ove mogućnosti funkcioniraju drukčije od drugoga, imate različite značajke postavljanje i podrška različitim scenarijima. Što svaki mogu se koristiti u odvajanja ili kombinirati te podatke.

- **Azure opterećenja** funkcionira na sloj prijenosa (sloja 4 u stogu referenca upravljački mreže). Distribucija razinom mrežni promet nudi kroz sve instance aplikacije koje se izvode u istom Azure podatkovnog centra.

- **Aplikacija pristupnika** radi sloju aplikacije (sloju 7 u stogu referencu na mreži upravljački). Djeluje kao obrnuti proxy servisa, prekidanje veze za klijenta i prosljeđivanje zahtjeva za krajnje točke pozadinske.

- **Upravitelj promet** funkcionira na razini DNS-a.  Da biste usmjerili promet krajnjeg korisnika globalno distribuirane krajnje točke koristi DNS odgovore. Klijenti zatim povezati s tim krajnje točke izravno.

U sljedećoj su tablici prikazane su značajke koje nudi svaku uslugu:

| Servis | Azure opterećenja | Pristupnik za aplikaciju | Upravitelj promet |
|---|---|---|---|
|Tehnologija| Prijenos razine (sloja 4) | Razini aplikacije (Layer 7) | Razina DNS-a |
| Podržani protokoli za aplikaciju | Sve | HTTP i HTTPS |  Sve (krajnje HTTP je potreban za nadzor krajnje točke) |
| Krajnje točke | Azure instance uloga VMs i servise u Oblaku | Bilo koji Azure interne IP adresa ili javni internet IP adresa | Azure VMs, servise u Oblaku, Azure web-aplikacije i vanjske krajnje točke |
| Podrška za Vnet | Moguće je koristiti za oba internetske dostupnog i internih aplikacije (Vnet) | Moguće je koristiti za oba internetske dostupnog i internih aplikacije (Vnet) |    Podržava samo aplikacije mjesto na Internetu |
Nadzor krajnje točke | Podržani putem probes | Podržani putem probes | Podržani putem HTTP/HTTPS GET | 

Azure opterećenja i mreže usmjeravanje aplikacije pristupnika promet za krajnje točke, ali imaju različite korištenje scenarija koji promet za rukovanje. U sljedećoj su tablici pomaže razlika između dva učitavanja balancers:

| Vrsta | Azure opterećenja | Pristupnik za aplikaciju |
|---|---|---|
| Protokole | UDP/TCP | HTTP / HTTPS |
| Rezervacija IP | Podržana | Nije podržano | 
| Način za ujednačavanje opterećenja | 5 tuple(source IP, source port, destination IP, destination port, protocol type) | Kružno<br>Usmjeravanje temelju URL-a | 
| Način rada za ujednačavanje opterećenja (izvorni IP / ljepljive sesije) |  2-n-torke (IP izvora i odredišni IP), 3-n-torke (IP izvora, IP odredište i priključak). Mogu mijenjati veličinu prema gore ili dolje ovisno o broju virtualnim strojevima | Afinitet utemeljen na kolačića<br>Usmjeravanje temelju URL-a |
| Stanje probes | Zadani: probni interval - 15 sekundi. Izbaciti iz rotacije: 2 neprekinuti pogreške. Podržava korisnički definirane probes | Neaktivne probni interval 30 sekundi. Uzima nakon 5 neuspjeha uzastopne uživo promet ili jedan probni pogreške u stanju mirovanja načinu. Podržava korisnički definirane probes | 
| SSL rasteretite | Nije podržano | Podržana | 
  