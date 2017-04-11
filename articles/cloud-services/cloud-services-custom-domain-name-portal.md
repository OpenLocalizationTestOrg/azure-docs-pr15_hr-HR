<properties
    pageTitle="Konfiguriranje prilagođenog naziva domene u servise u Oblaku | Microsoft Azure"
    description="Saznajte kako da biste otkrili Azure aplikacije ili podataka s Interneta na prilagođenoj domeni konfiguriranjem postavki DNS-a.  U ovim se primjerima pomoću portala za Azure."
    services="cloud-services"
    documentationCenter=".net"
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="adegeo"/>

# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Konfiguriranje prilagođenog naziva domene za Azure oblaku

> [AZURE.SELECTOR]
- [Portal za Azure](cloud-services-custom-domain-name-portal.md)
- [Azure klasični portal](cloud-services-custom-domain-name.md)

Kada stvorite servis u Oblaku, Azure dodjeljuje ga poddomenu **cloudapp.net**. Ako, na primjer, ako servis u Oblaku pod nazivom "contoso", korisnici bit će moći pristupiti aplikacija na URL-a kao što je http://contoso.cloudapp.net. Azure dodjeljuje i virtualna IP adresa.

Međutim, aplikacija možete izložiti na vlastiti naziv domene, primjerice **contoso.com**. U ovom se članku objašnjava kako rezerviranje ili konfigurirajte prilagođenog naziva domene za servis u Oblaku web uloge.

Želite već undestand su zapisi koje CNAME i odgovora? [Skočni prošle na explaination](#add-a-cname-record-for-your-custom-domain).

> [AZURE.NOTE]
> Postupci u ovom ćete zadatku primjenjuju se na servise u Oblaku Azure. Aplikacije servisa, potražite u članku [Ovo](../app-service-web/web-sites-custom-domain-name.md). Računi za pohranu potražite u članku [Ovo](../storage/storage-custom-domain-name.md).

<p/>

> [AZURE.TIP]
> Brz početak brže – pomoću NOVOG Azure [upute s vodičem](http://support.microsoft.com/kb/2990804)!  Servisi u Oblaku Azure ili web-mjesta Azure Tren oka čini zastavicom prilagođeni naziv domene i zaštiti komunikacije (SSL).

## <a name="understand-cname-and-a-records"></a>Razumijevanje zapise CNAME i odgovora

CNAME (ili Pseudonim zapisa) i zapisa i omogućuje povezivanje naziva domene s određenim poslužiteljem (ili servisa u ovom slučaju) no funkcioniraju različito. Postoje i neke određene pitanja vezana uz prilikom korištenja programa zapise pomoću servisa Azure oblaka koja treba imati na umu prije no što odlučite koju želite koristiti.

### <a name="cname-or-alias-record"></a>Zapis CNAME ili pseudonim

CNAME zapis karte na *određenu* domenu, kao što je **contoso.com** ili **www.contoso.com**, naziv Kanonski domene. U ovom slučaju je naziv Kanonski domene na **[myapp] .cloudapp .net** naziv domene vaše Azure hostirane aplikacije. Nakon stvaranja, stvara se CNAME pseudonima za na **[myapp] .cloudapp .net**. Unos CNAME će riješiti IP adresu na **[myapp] .cloudapp .net** servisa automatski, pa ako se promijeni IP adrese servisa u oblaku, ne morate ništa poduzimati.

> [AZURE.NOTE]
> Neke registrara domena samo omogućuju da biste mapirali poddomene prilikom korištenja CNAME zapis, kao što je www.contoso.com, a ne korijenski naziva, primjerice contoso.com. Dodatne informacije o CNAME zapisa potražite u dokumentaciji registrar, [stavka Wikipedije CNAME zapis](http://en.wikipedia.org/wiki/CNAME_record)ili [IETF domenama – implementaciji i specifikacija](http://tools.ietf.org/html/rfc1035) dokumenta.

### <a name="a-record"></a>Zapis

*A* zapis karata domenu, kao što je **contoso.com** ili **www.contoso.com**, *ili domene zamjenskih znakova* kao što su ** \*. contoso.com**, s IP adresom. Ako je Azure servis u Oblaku, virtualne IP usluge. Da bi Glavna prednost A zapis putem CNAME zapis koji može imati jednu stavku koja koristi zamjenski znak, kao što su \* **. contoso.com**, koje želite rukovati zahtjeva za više podmjesta domene kao što su **mail.contoso.com**, **login.contoso.com**ili **www.contso.com**.

> [AZURE.NOTE]
> Budući da je A zapis mapirana statičke IP adrese, ne može automatski riješiti promjene IP adresu servis u Oblaku. IP adresa koristi servis u Oblaku dodijeljeno prvi put implementacije prazan razdoblje (proizvodnje ili pripremna.) Ako izbrišete uvođenje za na vremensko razdoblje, IP adresa je objavio Azure i sve buduće implementacijama sustava vremensko razdoblje dali novi IP adresa.
>
> Jednostavnijeg IP adresu vremensko razdoblje navedeni implementacije (proizvodnje ili pripremna) je ista i kada zamjena između pripremna i implementacijama proizvodnje ili nadogradnje na mjestu postojeće implementacije. Dodatne informacije o izvođenju ove akcije potražite [u](cloud-services-how-to-manage.md)članku Upravljanje servise u oblaku.


## <a name="add-a-cname-record-for-your-custom-domain"></a>Dodajte CNAME zapis za prilagođenu domenu

Da biste stvorili CNAME zapis, morate dodati novu stavku u tablici DNS za svoju prilagođenu domenu pomoću alata za koje ste dobili od registrara. Svaki registrara je slična, ali malo drugačiji način navodeći CNAME zapis, no konceptima jednake.

1. Koristite neku od ovih metoda da biste pronašli na **. cloudapp.net** naziv domene dodijeljene servis u oblaku.

    * Prijavite se na [portal za Azure], odaberite na servis u oblaku, pogledajte u odjeljku **Osnove** , a zatim pronađite stavku **URL web-mjesta** .

        ![sažet odjeljak grafikoni s prikazom web-mjesta][csurl]
            
        **OR**
  
    * Instaliranje i konfiguriranje [Azure Powershell](../powershell-install-configure.md)i koristite sljedeću naredbu:

        ```powershell
        Get-AzureDeployment -ServiceName yourservicename | Select Url
        ```
    
    Spremite naziv domene koji se koristi u URL koji je vratio neke od ovih metoda će je potrebno prilikom stvaranja CNAME zapis.

1.  Prijavite se na web-mjestu registrara DNS i idite na stranicu za upravljanje DNS-a. Potražite veze ili područja web-mjesta koje su označene kao **Naziv domene**, **DNS-a**ili **Upravljanje naziv poslužitelja**.

2.  Sada možete pronaći gdje možete odabrati ili unijeti CNAME korisnika. Možda ćete morati odaberite željenu vrstu zapisa iz programa padajuće prema dolje ili idite na stranicu Napredne postavke. Trebao bi izgledati riječi **CNAME** **Pseudonim**ili **poddomene**.

3.  Drugi naziv domene i poddomene morate unijeti i za CNAME, kao što su **"www"** želite li stvoriti drugo ime za **www.customdomain.com**. Ako želite stvoriti drugo ime za korijensku domenu, ona može biti navedena kao na '**@**' simbol u alatima za vaš registrar DNS-a.

4. Zatim navedite Kanonski naziv glavnog računala, što je domena **cloudapp.net** vaše aplikacije u tom slučaju.

Na primjer, sljedeći CNAME zapis prosljeđuje sav promet iz **www.contoso.com** **contoso.cloudapp.net**, naziv prilagođene domene distribuiranih aplikacije:

| Naziv poddomene pseudonim/glavnog računala | Kanonski domene     |
| ------------------------- | -------------------- |
| "www"                       | contoso.cloudapp.NET |

> [AZURE.NOTE]
Posjetitelja **www.contoso.com** nikada nećete vidjeti true glavnog računala (contoso.cloudapp.net), pa je postupak prosljeđivanje nevidljiv za krajnjeg korisnika.

> U primjeru iznad odnosi se samo na promet na poddomene **"www"** . Budući da ne možete koristiti zamjenske znakove s CNAME zapisa, morate stvoriti jedan CNAME za svaku domenu i poddomenu. Ako želite da preusmjere promet s poddomene, kao što su *. contoso.com, na vašu adresu cloudapp.net možete konfigurirati na * *Preusmjeravanje URL-a* * ili * *URL Proslijedi** stavke u vašim postavkama DNS-a ili stvorili A zapis.


## <a name="add-an-a-record-for-your-custom-domain"></a>Dodavanje zapisa odgovora za prilagođenu domenu

Da biste stvorili A zapis, najprije morate pronaći virtualna IP adresa servis u oblaku. Zatim dodajte novi unos u tablici DNS za svoju prilagođenu domenu pomoću alata za koje ste dobili od registrara. Svaki registrara je slična, ali malo drugačiji način navodeći A zapis, ali konceptima jednake.

1. Koristite neku od sljedećih načina za dohvaćanje IP adrese servis u oblaku.

    * Prijava na [portal za Azure]odaberite servis u oblaku, pogledajte u odjeljku **Osnove** i zatim pronađite stavku **javne IP adrese** .

        ![sažet odjeljak prikazuje na VIP][vip]

        **OR**

    * Instaliranje i konfiguriranje [Azure Powershell](../powershell-install-configure.md)i koristite sljedeću naredbu:

        ```powershell
        get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
        ```
    
    Spremite IP adresa će je potrebno prilikom stvaranja A zapisa.

1.  Prijavite se na web-mjestu registrara DNS i idite na stranicu za upravljanje DNS-a. Potražite veze ili područja web-mjesta koje su označene kao **Naziv domene**, **DNS-a**ili **Upravljanje naziv poslužitelja**.

2.  Sada možete pronaći gdje možete odabrati ili unijeti zapisa. Možda ćete morati odaberite željenu vrstu zapisa iz programa padajuće prema dolje ili idite na stranicu Napredne postavke.

3. Odaberite ili unesite domene i poddomene koji će koristiti taj zapis odgovora. Ako želite stvoriti drugo ime za **www.customdomain.com**, na primjer, odaberite **"www"** . Ako želite stvoriti zamjenski unosa za sve poddomene unesite "__*__". To će obuhvaća sve poddomene kao što su **mail.customdomain.com**, **login.customdomain.com**i **www.customdomain.com**.

    Ako želite da biste stvorili A zapis za korijensku domenu, ona može biti navedena kao na '**@**' simbol u alatima za vaš registrar DNS-a.

4. Unesite IP adresu servis u oblaku u navedeno polje. To povezuje stavku domene koriste u A zapis s IP adresom uvođenja servisa oblaka.

Ako, na primjer, sljedeći zapis prosljeđuje sav promet s **contoso.com** **137.135.70.239**, IP adresa distribuiranih aplikacije:

| Naziv glavnog računala/poddomene | IP adresa     |
| ------------------- | -------------- |
| @                   | 137.135.70.239 |


U ovom se primjeru pokazuje stvaranja A zapisa za korijensku domenu. Ako želite da biste stvorili unos zamjenskih da prekrije sve poddomene, unesite "__*__" kao poddomena.

>[AZURE.WARNING]
>IP adrese u Azure nisu dinamički prema zadanim postavkama. Će vjerojatno htjeti koristiti [Rezervirana IP adresa](../virtual-network/virtual-networks-reserved-public-ip.md) da biste bili sigurni da se ne mijenja IP adresa.

## <a name="next-steps"></a>Daljnji koraci

* [Kako upravljati servise u Oblaku](cloud-services-how-to-manage.md)
* [Upute za mapiranje CDN sadržaj prilagođene domene](../cdn/cdn-map-content-to-custom-domain.md)
* [Općenito konfiguracije servis u oblaku](cloud-services-how-to-configure-portal.md).
* Saznajte kako [implementirati servis u oblaku](cloud-services-how-to-create-deploy-portal.md).
* Konfiguriranje [ssl certifikata](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Portal za Azure]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
 