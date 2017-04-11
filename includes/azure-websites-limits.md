Resurs|Besplatni|Zajedničke (pretpregled)|Osnovni|Standardna|Premium (pretpregled)</th>
---|---|---|---|---|---
[Web-mjesto mobitel ili aplikacije API](https://azure.microsoft.com/services/app-service/) po [aplikacije servisa za plan](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)<sup>1</sup>|10|100|Neograničeno<sup>2</sup>|Neograničeno<sup>2</sup>|Neograničeno<sup>2</sup>
[Logika aplikacije](https://azure.microsoft.com/services/app-service/logic/) po [aplikacije servisa za planiranje](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)</a><sup>1</sup>|10|10|10|20 po core|20 po core
[Aplikacije servisa za planiranje](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)|1 po regijama|10 komada po grupa resursa|100 po grupa resursa|100 po grupa resursa|100 po grupa resursa
Vrsta instance za izračun|Zajedničko korištenje|Zajedničko korištenje|Namjenski<sup>3</sup>|Namjenski<sup>3</sup>|Namjenski<sup>3</sup></p>
[Promjena veličine izlaz](../articles/app-service-web/web-sites-scale.md) (Maks instance)|1 zajedničko korištenje|1 zajedničko korištenje|3 namjenski<sup>3</sup>|10 namjenski<sup>3</sup>|20 namjenski (50 u elika i mala slova)<sup>3,4</sup>
Prostor za pohranu<sup>5</sup>|1 GB<sup>5</sup>|1 GB<sup>5</sup>|10 GB<sup>5</sup>|50 GB<sup>5</sup>|500 GB<sup>4,5</sup></p>
CPU vrijeme (short)<sup>6</sup>|3 minute|3 minute|Neograničeno, plaćanje na standardni [stopama.](https://azure.microsoft.com/pricing/details/app-service/)</a>|Neograničeno, plaćanje prema standardne stopama.|Neograničeno, plaćanje prema standardne stopama.
Vrijeme (dan) procesora<sup>6</sup>|60 minuta|240 minuta|Neograničeno, plaćanje na standardni [stopama.](https://azure.microsoft.com/pricing/details/app-service/)</a>|Neograničeno, plaćanje prema standardne stopama.|Neograničeno, plaćanje prema standardne stopama.
Memorija (1 sat)|1024 MB po aplikacije servisa za planiranje|1024 MB po aplikacije|N/D|N/D|N/D
Propusnosti|165 MB|Neograničeno, Primjena [tečajeve za prijenos podataka](https://azure.microsoft.com/pricing/details/data-transfers/)|Neograničeno, primijenite stope prijenos podataka|Neograničeno, primijenite stope prijenos podataka|Neograničeno, primijenite stope prijenos podataka
Arhitektura aplikacije|32-bitni|32-bitni|32-bitne i 64-bitni|32-bitne i 64-bitni|32-bitne i 64-bitni
Sockets web po instancu<sup>7</sup>|5|35|350|Neograničeno|Neograničeno
Istodobni [veze za ispravljanje pogrešaka](../articles/app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md) po aplikacije|1|1|1|5|5
[azurewebsites.NET poddomenu s FTP/S i SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|X|X|X|X|X
Podrška za [prilagođene domene](../articles/app-service-web/web-sites-custom-domain-name.md)||X|X|X|X
Prilagođene domene [podržava SSL](../articles/app-service-web/web-sites-configure-ssl-certificate.md)|||Neograničeno|Neograničeno, 5 SNI SSL i 1 IP SSL veze uključen|Neograničeno, 5 SNI SSL i 1 IP SSL veze uključen
Integrirano opterećenja||X|X|X|X
[Uvijek uključen](../articles/app-service-web/web-sites-configure.md)|||X|X|X
[Planirani sigurnosne kopije](../articles/app-service-web/web-sites-backup.md)||||Jednom dnevno|Jednom svaki pet minuta<sup>8</sup>
[Automatsko skaliranje](../articles/app-service-web/web-sites-scale.md)|||X|X|X
[WebJobs](../articles/app-service-web/web-sites-create-web-jobs.md) <sup>9</sup>|X|X|X|X|X
Podrška za [Azure rasporeda](https://azure.microsoft.com/services/scheduler/)||X|X|X|X
[Nadzor krajnje točke](../articles/app-service-web/web-sites-monitor.md)|||X|X|X
[Pripremna slobodnih (pretpregled)](../articles/app-service-web/web-sites-staged-publishing.md)||||5|20
Prilagođene domene po aplikacije</a>||500|500|500|500
SLA||<p>|99.9%|99.95%<sup>10</sup>|99.95%<sup>10</sup>

<sup>1</sup> Aplikacije i kvotama za pohranu su po aplikacije servisa za planiranje ako naznačeno u suprotnom.  
<sup>2</sup> Stvarni broj aplikacije koje možete hostirati na te ovisi o aktivnosti aplikacije, veličine instance računala i odgovarajuće Upotreba resursa.  
<sup>3</sup> Namjenski instanci može biti različitih veličina. Potražite u članku [Cijene za aplikacije servisa](https://azure.microsoft.com/pricing/details/data-transfers/pricing/details/app-service/) za više detalja.  
<sup>4</sup> Premium sloju omogućuje do 50 formula izračunava instance (primjenjuje dostupnost) i 500 GB prostora na disku prilikom korištenja aplikacije servisa okruženjima i 20 izračunati instanci i 250 GB prostora za pohranu u suprotnom.  
<sup>5</sup> Ograničenje prostora za pohranu je ukupna veličina sadržaja preko sve aplikacije u istu tarifu aplikacije servisa. Dodatne mogućnosti pohrane su dostupne u [Okruženju servisa aplikacija](../articles/app-service-web/app-service-web-configure-an-app-service-environment.md#storage)  
<sup>6</sup> Ti resursi ograničene su fizičkim resursima na namjenski instance (instanci veličina i koliko je instanci).  
<sup>7</sup> Ako promijenite aplikacije u osnovni sloju dvije instance, imate 350 Istodobni veze za svaku od dvije instance.  
<sup>8</sup> Premium sloju omogućuje sigurnosne kopije intervalima prema dolje do svakih 5 minuta prilikom korištenja aplikacije servisa okruženjima i 50 puta dnevno drukčije.  
<sup>9</sup> Pokretanje prilagođene izvršne datoteke i/ili skripte na zahtjev, na raspored ili kontinuirano kao pozadinu zadatak u vašoj instanci aplikacije servisa. Stalna potreban je za izvršavanje neprekinuti WebJobs. Azure raspored slobodno ili standardna je potrebna za zakazano WebJobs. Nema definiranog ograničenja broja WebJobs koji se može pokrenuti u instanci aplikacije servisa, ali postoje praktično ograničenja koja ovise o tome što kod aplikacija pokušava učinite.   
<sup>10</sup> Za prebacivanje konfiguriran SLA 99.95% namijenjeno implementacijama koje koriste više instanci programa Azure promet Manager.  
