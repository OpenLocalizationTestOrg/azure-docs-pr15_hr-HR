<properties
    pageTitle="Otklanjanje poteškoća s krajnje točke Azure CDN vraćanje 404 status | Microsoft Azure"
    description="Otklanjanje poteškoća s 404 šifre odgovor s Azure CDN krajnje točke."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>
    
# <a name="troubleshooting-cdn-endpoints-returning-404-statuses"></a>Otklanjanje poteškoća s krajnje točke CDN vraćanje 404 statusi

Ovaj članak sadrži otklanjanje poteškoća s [krajnje točke CDN](cdn-create-new-endpoint.md) uputio 404 pogreške.

Ako vam je potrebna dodatna pomoć u ovom članku u bilo kojem trenutku, možete se obratiti Azure stručnjaka na [MSDN Azure i forume hrpu preljeva](https://azure.microsoft.com/support/forums/). Osim toga, možete uputiti incident Azure podršci. Idi na [web-mjesto za podršku za Azure](https://azure.microsoft.com/support/options/) i kliknite **Dohvati podrška**.

## <a name="symptom"></a>Simptoma

Koje ste stvorili CDN profila i krajnje, ali sadržaj ne čini se da bi bio dostupan na na CDN.  Korisnici koji pokušaju pristupiti sadržaju putem CDN URL primaju kodovi stanja HTTP 404. 

## <a name="cause"></a>Uzrok

Postoji nekoliko mogućih razloga, uključujući:

- Izvor datoteke nije vidljiva na CDN
- Krajnja točka je pogrešno konfigurirana, uzrokuju CDN potražite na krivom mjestu
- Glavno računalo je odbijanje zaglavlja glavnog računala s na CDN
- Krajnja točka nije imali vremena za prijenos cijeloj na CDN

## <a name="troubleshooting-steps"></a>Korake za otklanjanje poteškoća

> [AZURE.IMPORTANT] Nakon stvaranja krajnjoj točki CDN neće odmah biti dostupan za korištenje, kao što je potrebno vrijeme za registraciju proširiti kroz na CDN.  Profili <b>CDN Azure s Akamai</b> prijenos se obično dovršava unutar jedne minute.  Profili <b>CDN Azure s Verizon</b> prijenos će obično dovršiti za 90 minuta, ali u nekim slučajevima može trajati dulje.  Ako dovršite korake u ovom dokumentu, a i dalje primate odgovore 404, razmislite o čekanja od nekoliko sati da biste provjerili ponovno prije otvaranja zahtjev za podršku možete.

### <a name="check-the-origin-file"></a>Provjerite datoteku origin

Najprije ćemo trebali biste provjerite koje želimo predmemorirane datoteke je dostupan na našem polazište, a javno dostupno.  Najbrži način za to je u privatno ili u sesiju Inkognito otvorite preglednik i Idite izravno na datoteku.  Samo upišite ili zalijepite URL u okvir adresa i potražite u članku ako to rezultira datoteke koje ste i očekivali.  U ovom primjeru ću korištenje datoteke mogu imati u račun za pohranu Azure, dostupna na `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`.  Kao što vidite, uspješno prosljeđuje test.

![Uspjeh!](./media/cdn-troubleshoot-endpoint/cdn-origin-file.png)

> [AZURE.WARNING] Dok je najbrži i najjednostavniji način provjerite je li datoteka javno dostupna, neke mrežnim konfiguracijama u tvrtki ili ustanovi nije vam dao privid datoteka je javno dostupna kad se, činjenica, samo vidljive korisnicima mrežom (čak i ako se hostira u Azure).  Ako imate vanjski preglednika koji se iz kojeg možete testirati, kao što je mobilni uređaj koji nije povezan s mrežom svoje tvrtke ili ustanove ili virtualnog računala u Azure, koje bi najbolje.

### <a name="check-the-origin-settings"></a>Provjerite postavke polazište

Nakon što smo ste potvrdili datoteka je javno dostupno na Internetu, ne možemo trebali biste provjerite naš polazište postavke.  [Portal za Azure](https://portal.azure.com)pronađite CDN profila, a zatim kliknite krajnju točku ste otklanjanje poteškoća.  U rezultirajućem plohu **krajnju točku** , kliknite izvor.  

![Krajnja točka plohu s polazište istaknuta](./media/cdn-troubleshoot-endpoint/cdn-endpoint.png)

Pojavit će se plohu **polazište** . 

![Polazište plohu](./media/cdn-troubleshoot-endpoint/cdn-origin-settings.png)

#### <a name="origin-type-and-hostname"></a>Vrsta izvora i naziv glavnog računala

Provjerite je li ispravna **Vrsta izvora** , pa provjerite **polazište naziv glavnog računala**.  U primjeru, `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, naziv glavnog računala dio URL-a je `cdndocdemo.blob.core.windows.net`.  Kao što možete vidjeti u snimku zaslona, to nije točno.  Za pohranu Azure, web-aplikacije i drugačijeg izvora u Oblaku, polje **naziv glavnog računala polazište** je na padajućem popisu tako da mi ne morate brinuti o provjere pravopisa pravilno.  Međutim, ako koristite prilagođeni polazište je *apsolutno ključnih* na naziv glavnog računala pravilno napisane!

#### <a name="http-and-https-ports"></a>HTTP i HTTPS priključci

Stvar da biste provjerili ovdje je **HTTP** i **HTTPS priključaka**.  U većini slučajeva, koristite li točan 80 i 443, a potrebno nikakve promjene.  Međutim, ako izvorni poslužitelj priključuje na različitim port (priključak), koje morat ćete ovdje predstavljena.  Ako niste sigurni, samo pogledajte URL polazište datoteke.  Specifikacije HTTP i HTTPS navedite priključke 80 i 443 kao zadane. U moje URL `https://cdndocdemo.blob.core.windows.net/publicblob/lorem.txt`, priključak nije naveden, tako da se zadana 443 pretpostavlja se da je i Moje postavke ispravne.  

Međutim, izgovorite URL za datoteku polazište koji ste ranije testira je `http://www.contoso.com:8080/file.txt`.  Napomena u `:8080` na kraju segment naziv glavnog računala.  Po tome zna preglednik tako da koristi priključak `8080` da biste se povezali s web-poslužiteljem na `www.contoso.com`, pa ćete morati unijeti 8080 u polju **HTTP priključak** .  Važno je da Zapamtite ove postavke priključka samo utječu koje priključak krajnju točku koristi za dohvaćanje podataka iz izvor.

> [AZURE.NOTE] Krajnje točke **CDN Azure s Akamai** ne dopuštaju cijeli raspon TCP priključak za drugačijeg izvora.  Popis priključaka polazište je dopušteno, potražite u članku [CDN Azure s Akamai dopuštene polazište priključke](https://msdn.microsoft.com/library/mt757337.aspx).  
  
### <a name="check-the-endpoint-settings"></a>Provjerite postavke krajnje točke

Ponovno uključite plohu **krajnju točku** , kliknite gumb **Konfiguracija** .

![Krajnja točka plohu s istaknutim gumbom za konfiguriranje](./media/cdn-troubleshoot-endpoint/cdn-endpoint-configure-button.png)

Pojavit će se plohu **Konfiguriraj** krajnju točku.

![Konfiguriranje plohu](./media/cdn-troubleshoot-endpoint/cdn-configure.png)

#### <a name="protocols"></a>Protokole

Za **protokoli**, provjerite je li odabran protokol koji se koristi za klijente.  Isti protokol koji se koristi klijenta bit će onog koji je korišten za pristup polazište, stoga je važno da bi priključke polazište ispravno konfigurirano u prethodnom odjeljku.  Krajnja točka očekuje podatke samo na zadani HTTP i HTTPS priključke (80 i 443), bez obzira na to priključke polazište.

Pogledajmo vratili u našem hipotetska primjer s `http://www.contoso.com:8080/file.txt`.  Naveden kao ćete lako zapamtiti, Contoso `8080` kao njihove HTTP priključak, ali ćemo i pretpostavlja da su navedeni `44300` kao njihove HTTPS port (priključak).  Ako su stvorili krajnje pod nazivom `contoso`, njihov naziv glavnog računala za krajnje točke CDN bio `contoso.azureedge.net`.  Zahtjev za `http://contoso.azureedge.net/file.txt` je HTTP zahtjev krajnju točku bi putem HTTP-a na port 8080 dohvatiti iz izvor.  Zahtjev za sigurnu putem HTTPS, `https://contoso.azureedge.net/file.txt`, može prouzročiti krajnja točka za korištenje HTTPS na priključak 44300 kada retriving datoteke iz izvor.

#### <a name="origin-host-header"></a>Izvor zaglavlja glavnog računala

**Zaglavlje domaćina polazište** je šalje polazište sa zahtjevom za svaku vrijednost zaglavlja glavnog računala.  U većini slučajeva, to mora biti isti kao **naziv glavnog računala polazište** smo neke starije verzije provjerava.  Neispravna vrijednost u polju obično neće uzrokovati 404 statusi, no vjerojatno će uzrokovati druge statusi 4xx, ovisno o tome što očekuje izvor.

#### <a name="origin-path"></a>Put Origin

Na kraju, ne možemo provjerite hoće li naš **polazište put**.  Prema zadanim postavkama to je prazno.  To polje treba koristiti samo ako želite suziti opseg polazište hostira resursa koje želite da bi bili dostupni u CDN.  

Na primjer, u moj krajnjoj točki I željeli svih resursa na moj račun za pohranu da bi bio dostupan, tako da se prazna **polazište put** .  To znači da zahtjev za `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` rezultira vezu iz moje krajnja točka za `cdndocdemo.core.windows.net` koji zahtijeva `/publicblob/lorem.txt`.  Isto tako, zahtjev za `https://cdndocdemo.azureedge.net/donotcache/status.png` rezultati traži krajnju točku `/donotcache/status.png` iz izvor.

No ako se ne želite koristiti u CDN za svaki put na moj polazište?  Samo izgovorite kojeg želite izložiti u `publicblob` put.  Ako se unosi */publicblob* Moje polje **polazište put** , zbog kojih će krajnja točka za umetanje */publicblob* prije svaki zahtjev za izvor.  To znači da zahtjeva za `https://cdndocdemo.azureedge.net/publicblob/lorem.txt` zapravo sada se zahtjev za dio URL-a, `/publicblob/lorem.txt`, i dodavati `/publicblob` na početak. Rezultat u zahtjev za `/publicblob/publicblob/lorem.txt` iz izvor.  Ako taj put ne riješite Stvarni datoteku, izvor će vratiti 404 status.  Zapravo bio ispravan URL za dohvaćanje lorem.txt u ovom primjeru `https://cdndocdemo.azureedge.net/lorem.txt`.  Napomena da mi ne uvrstite */publicblob* put uopće jer je zahtjev za dio URL-a `/lorem.txt` i dodati krajnju točku `/publicblob`, rezultirajući u `/publicblob/lorem.txt` zahtjev proslijeđen izvor.
