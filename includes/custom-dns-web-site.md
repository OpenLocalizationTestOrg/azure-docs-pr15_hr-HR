#<a name="configuring-a-custom-domain-name-for-an-azure-website"></a>Konfiguriranje prilagođenog naziva domene za Azure web-mjesto

Kada stvorite web-mjesto, Azure nudi neslužbeni poddomene na domeni azurewebsites.net tako da korisnici mogu pristupati web-mjesta pomoću URL-a kao što je http://&lt;Moje web-mjesto >. azurewebsites.net. Međutim, ako konfigurirate vašeg web-mjesta za zajedničko korištenje ili standardnom načinu rada, možete mapirati web-mjesta vlastiti naziv domene.

Po želji Azure promet Manager možete koristiti da biste učitali dolaznih promet saldo na web-mjesto. Dodatne informacije o funkcioniranje Upravitelj promet s web-mjesta, potražite u članku [Upravljanje Azure web-mjesta promet s Azure promet Upravitelj][trafficmanager].

> [AZURE.NOTE] Postupci u ovom ćete zadatku primjenjuju se na web-mjesta Azure; Servisi u Oblaku, potražite u članku <a href="/develop/net/common-tasks/custom-dns/">Konfiguriranje prilagođeni naziv domene u Azure</a>.

> [AZURE.NOTE] Koraci u ovom ćete zadatku potrebno da biste konfigurirali web-mjestima sustava za zajedničko korištenje ili standardnom načinu, koje se mijenjaju koliko se naplatiti za vašu pretplatu. Dodatne informacije potražite u članku <a href="/pricing/details/web-sites/">Pojedinosti cijene web-mjesta</a> .

U ovom članku:

-   [Objašnjenje zapise CNAME i odgovora](#understanding-records)
-   [Konfiguriranje web-mjesta za zajedničke ili standardnom načinu rada](#bkmk_configsharedmode)
-   [Dodavanje web-mjesta u upravitelju promet](#trafficmanager)
-   [Dodavanje CNAME za prilagođenu domenu](#bkmk_configurecname)
-   [Dodavanje zapisa odgovora za prilagođenu domenu](#bkmk_configurearecord)

<h2><a name="understanding-records"></a>Razumijevanje zapisi CNAME i odgovora</h2>

CNAME (ili Pseudonim zapisa) i zapisa i omogućuje povezivanje naziva domene s web-mjesta, no svaki korisnik funkcioniraju različito.

###<a name="cname-or-alias-record"></a>Zapis CNAME ili pseudonim

CNAME zapis karte na *određenu* domenu, kao što je **contoso.com** ili **www.contoso.com**, naziv Kanonski domene. U ovom slučaju Kanonski je naziv domene na nešto od sljedećeg u ** &lt;myapp >. azurewebsites.net** naziv domene Azure web-mjesta ili ** &lt;myapp >. trafficmgr.com** naziv domene promet upravitelj profila. Nakon stvaranja, stvara se CNAME pseudonima za u ** &lt;myapp >. azurewebsites.net** ili ** &lt;myapp >. trafficmgr.com** naziv domene. Unos CNAME će riješiti IP adresu na ** &lt;myapp >. azurewebsites.net** ili ** &lt;myapp >. trafficmgr.com** naziva domene automatski u pa ako se promijeni IP adresu web-mjesta, nije potrebno ništa poduzimati.

> [AZURE.NOTE] Neke registrara domena samo omogućuju da biste mapirali poddomene prilikom korištenja CNAME zapis, kao što je www.contoso.com, a ne korijenski naziva, primjerice contoso.com. Dodatne informacije o CNAME zapisa potražite u dokumentaciji registrar, <a href="http://en.wikipedia.org/wiki/CNAME_record">stavka Wikipedije CNAME zapis</a>ili <a href="http://tools.ietf.org/html/rfc1035">IETF domenama – implementaciji i specifikacija</a> dokumenta.

###<a name="a-record"></a>Zapis

A zapis karata domenu, kao što je **contoso.com** ili **www.contoso.com**, *ili domene zamjenskih znakova* kao što su ** \*. contoso.com**, s IP adresom. U slučaju programa Azure web-mjesto virtualne IP servis ili iz određene IP adresa koje ste kupili za web-mjesta. Da bi Glavna prednost A zapis putem CNAME zapis koji može imati jednu stavku koja koristi zamjenski znak, kao što su * **. contoso.com**, kao što su koje želite rukovati zahtjeva za više poddomene * *mail.contoso.com**; * *login.contoso.com**, ili * *www.contso.com**.

> [AZURE.NOTE] Budući da je A zapis mapirana statičke IP adrese, ne može automatski riješiti promjene IP adresa web-mjesta. IP adresa za uporabu s zapisa se nudi prilikom konfiguriranja postavki naziv prilagođene domene za web-mjesta; No tu vrijednost može promijeniti ako izbrišete i ponovno stvorite web-mjesta ili promijenite način web-mjesta u pozadinu radi oslobađanja.

> [AZURE.NOTE] U zapisima nije moguće koristiti za pomoću upravitelja promet za ujednačavanje opterećenja. Dodatne informacije potražite u članku [Upravljanje Azure web-mjesta promet s Azure promet Upravitelj][trafficmanager].

<a name="bkmk_configsharedmode"></a><h2>Konfiguriranje web-mjesta za zajedničke ili standardnom načinu rada</h2>

Postavljanje prilagođenog naziva domene na web-mjesto dostupna je samo za zajedničko korištenje i standardnog načina za Azure web-mjesta. Prije promjene web-mjestu u načinu besplatno web-mjesta da biste zajednički se koristi ili standardni web-mjesta, najprije morate ukloniti kupujete velika slova na mjestu za pretplatu na web-mjesta. Dodatne informacije o zajednički se koristi i standardnu način cijene, potražite u članku [Cijene pojedinosti][PricingDetails].

1. U pregledniku otvorite [Portal za upravljanje][portal].
2. Na kartici **web-mjesta** kliknite naziv web-mjesta.

    ![][standardmode1]

3. Kliknite karticu **Promjena VELIČINE** .

    ![][standardmode2]


4. U odjeljku **Općenito** postavite način web-mjesto tako da kliknete **zajednički se koristi**.

    ![][standardmode3]

    > [AZURE.NOTE] Ako namjeravate koristiti upravitelj promet za ovo web-mjesto, morate koristiti odaberite standardnom načinu umjesto zajednički se koristi.

5. Kliknite **Spremi**.
6. Kada se pojavi upit o povećava trošak zajednički rad (ili standardnom načinu ako odaberete standardna), kliknite **da** ako se slažete.

    <!--![][standardmode4]-->

    **Napomena**<br />
   Ako vam se prikaže poruka o pogrešci u "Konfiguriranje mjerilo za web-mjesto"naziv web-mjesta"nije uspjela", koristite gumb Pojedinosti da biste saznali više.

<a name="trafficmanager"></a><h2>(Neobavezno) Dodavanje web-mjestima sustava u promet Manager</h2>

Ako želite koristiti web-mjesta pomoću upravitelja promet poduzeti sljedeće korake.

1. Ako već nemate promet upravitelj profila, koristite informacije u [Stvaranje profila Upravitelja promet pomoću brzo stvaranje] [ createprofile] da biste je stvorili. Napomena u **. trafficmgr.com** naziv domene povezane sa promet upravitelj profila. To će se koristiti u noviji koraku.

2. Pomoću informacija u [Dodavanje ili brisanje krajnje točke] [ addendpoint] da biste dodali web-mjesta kao krajnje točke u profilu programa Upravitelj promet.

    > [AZURE.NOTE] Ako web-mjesta nije naveden prilikom dodavanja krajnje, provjerite je li konfiguriran za standardnom načinu rada. Morate koristiti standardnom načinu rada za web-mjesta da bi rad s programom Manager promet.

3. Prijavite se na web-mjestu registrara DNS-a, a zatim idite na stranicu za upravljanje DNS-a. Potražite veze ili područja web-mjesta koje su označene kao **Naziv domene**, **DNS-a**ili **Upravljanje naziv poslužitelja**.

4. Sada možete pronaći gdje možete odabrati ili unijeti CNAME zapisa. Možda ćete morati odaberite željenu vrstu zapisa iz programa padajuće prema dolje ili idite na stranicu Napredne postavke. Trebao bi izgledati riječi **CNAME** **Pseudonim**ili **poddomene**.

5. Morate navesti i drugi naziv domene i poddomene za na CNAME. Na primjer, **"www"** želite li stvoriti drugo ime za **www.customdomain.com**.

5. Morate navesti naziv glavnog računala koja je naziv Kanonski domene za taj CNAME pseudonim. Ovo je na **. trafficmgr.com** naziv vašeg web-mjesta.

Na primjer, sljedeći CNAME zapis prosljeđuje sav promet iz **www.contoso.com** **contoso.trafficmgr.com**, naziv domene web-mjesta:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Naziv poddomene pseudonim/glavnog računala</strong></td>
<td><strong>Kanonski domene</strong></td>
</tr>
<tr>
<td>"www"</td>
<td>contoso.trafficmgr.com</td>
</tr>
</table>

Posjetitelja **www.contoso.com** nikada nećete vidjeti true glavnog računala (contoso.azurewebsite.net), pa je postupak prosljeđivanje nevidljiv za krajnjeg korisnika.

> [AZURE.NOTE] Ako koristite Upravitelj promet s web-mjesto, ne morate slijedite korake u sljedećim odjeljcima, "**Dodavanje CNAME za prilagođenu domenu**" i "**Add A zapisa za prilagođene domene**". CNAME zapis stvorili u prethodnim koracima će dolazne promet usmjerili promet upravitelju, koji usmjerava promet endpoint(s) web-mjesta.

<a name="bkmk_configurecname"></a><h2>Dodavanje CNAME za prilagođenu domenu</h2>

Da biste stvorili CNAME zapis, morate dodati novu stavku u tablici DNS za svoju prilagođenu domenu pomoću alata za koje ste dobili od registrara. Svaki registrara je slična, ali malo drugačiji način navodeći CNAME zapis, ali konceptima jednake.

1. Koristite neku od ovih metoda da biste pronašli na **. azurewebsite.net** naziv domene dodijeljene web-mjesta.

    * Prijavite se na [Portal za upravljanje Azure][portal]odaberite web-mjesta, odaberite **nadzornu ploču**, a zatim pronađite stavku **URL web-mjesta** u odjeljku **brzi pogled** .

    * Instaliranje i konfiguriranje [Azure Powershell](/manage/install-and-configure-windows-powershell/)i koristite sljedeću naredbu:

            get-azurewebsite yoursitename | select hostnames

    * Instaliranje i konfiguriranje [Azure naredbeni redak sučelja](/manage/install-and-configure-cli/)i koristite sljedeću naredbu:

            azure site domain list yoursitename

    Spremiti **. azurewebsite.net** ime, kao što je će se koristiti u sljedećim koracima.

3. Prijavite se na web-mjestu registrara DNS-a, a zatim idite na stranicu za upravljanje DNS-a. Potražite veze ili područja web-mjesta koje su označene kao **Naziv domene**, **DNS-a**ili **Upravljanje naziv poslužitelja**.

4. Sada možete pronaći gdje možete odabrati ili unijeti CNAME zapisa. Možda ćete morati odaberite željenu vrstu zapisa iz programa padajuće prema dolje ili idite na stranicu Napredne postavke. Trebao bi izgledati riječi **CNAME** **Pseudonim**ili **poddomene**.

5. Morate navesti i drugi naziv domene i poddomene za na CNAME. Na primjer, **"www"** želite li stvoriti drugo ime za **www.customdomain.com**. Ako želite stvoriti drugo ime za korijensku domenu, ona može biti navedena kao na '**@**' simbol u alatima za vaš registrar DNS-a.

5. Morate navesti naziv glavnog računala koja je naziv Kanonski domene za taj CNAME pseudonim. Ovo je na **. azurewebsite.net** naziv vašeg web-mjesta.

Na primjer, sljedeći CNAME zapis prosljeđuje sav promet iz **www.contoso.com** **contoso.azurewebsite.net**, naziv domene web-mjesta:

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tr>
<td><strong>Naziv poddomene pseudonim/glavnog računala</strong></td>
<td><strong>Kanonski domene</strong></td>
</tr>
<tr>
<td>"www"</td>
<td>contoso.azurewebsite.NET</td>
</tr>
</table>

Posjetitelja **www.contoso.com** nikada nećete vidjeti true glavnog računala (contoso.azurewebsite.net), pa je postupak prosljeđivanje nevidljiv za krajnjeg korisnika.

> [AZURE.NOTE] U primjeru iznad odnosi se samo na promet na poddomene __"www"__ . Budući da ne možete koristiti zamjenske znakove s CNAME zapisa, morate stvoriti jedan CNAME za svaku domenu i poddomenu. Ako želite da preusmjere promet s poddomene, kao što su *. contoso.com, na vašu adresu azurewebsite.net možete konfigurirati __Preusmjeravanje URL-a__ ili __Proslijedi URL-a__ stavke u postavkama DNS-a ili stvorili A zapis.

> [AZURE.NOTE] Može potrajati neko vrijeme za vaše CNAME proširiti kroz DNS sustav. Dok se CNAME propagira nećete moći postaviti CNAME web-mjesta. Servis kao što su <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> možete koristiti da biste provjerili je li dostupna na CNAME.

###<a name="add-the-domain-name-to-your-website"></a>Dodajte naziv domene web-mjestu

Kada propagira CNAME zapis za naziv domene, morate je povezati s web-mjesta. Možete dodati prilagođenu domenu definira CNAME zapis za svoje web-mjesto pomoću bilo Azure sučelja naredbenog retka (Azure EŽA) ili pomoću portala za upravljanje Azure.

**Da biste dodali naziv domene pomoću alata za naredbenog retka**

Instaliranje i konfiguriranje [sučelje naredbenog retka za Azure](/manage/install-and-configure-cli/)i koristite sljedeću naredbu:

    azure site domain add customdomain yoursitename

Ako, na primjer, sljedeće će dodati prilagođeni naziv domene od **www.contoso.com** **contoso.azurewebsite.net** web-mjesto:

    azure site domain add www.contoso.com contoso

Možete potvrditi da se naziv prilagođene domene dodan na web-mjesto pomoću sljedeće naredbe:

    azure site domain list yoursitename

Na popisu vratio ta naredba mora sadržavati naziv prilagođene domene, kao i zadani **. azurewebsite.net** unosa.

**Da biste dodali naziv domene pomoću portala za upravljanje Azure**

1. U pregledniku otvorite [Portal za upravljanje Azure][portal].

2. Na kartici **web-mjesta** kliknite naziv web-mjesta, odaberite **nadzornu ploču**, a zatim **Upravljanje domenama** na dno stranice.

    ![][setcname2]

6. U tekstni okvir **Naziva domena** upišite naziv domene koji ste konfigurirali.

    ![][setcname3]

6. Kliknite kvačicu da biste prihvatili naziv domene.

Kada konfiguracije završi, naziv prilagođene domene će biti navedene u odjeljku **naziva domena** na stranici **Konfiguracija** web-mjesta.

<a name="bkmk_configurearecord"></a><h2>Dodavanje zapisa odgovora za prilagođenu domenu</h2>

Da biste stvorili A zapis, morate najprije pronađite IP adresu web-mjesta. Zatim dodajte stavku u tablici DNS za svoju prilagođenu domenu pomoću alata za koje ste dobili od registrara. Svaki registrara je slična, ali malo drugačiji način navodeći A zapis, ali konceptima jednake. Osim stvaranja A zapisa, morate stvoriti i CNAME zapis koji koristi Azure da biste provjerili A zapis.

1. U pregledniku otvorite [Portal za upravljanje Azure][portal].

2. Na kartici **web-mjesta** kliknite naziv web-mjesta, odaberite **nadzornu ploču**, a zatim **Upravljanje domenama** od dna zaslona.

    ![][setcname2]

5. U dijaloškom okviru **Upravljanje prilagođene domene** pronađite **U IP adresa za korištenje prilikom konfiguriranja na zapise**. Kopirajte IP adresa. To će se koristiti prilikom stvaranja A zapis.

5. U dijaloškom okviru **Upravljanje prilagođene domene** , imajte na umu awverify naziv domene na kraju teksta pri vrhu dijaloškog okvira. Mora biti **awverify.mysite.azurewebsites.net** pri čemu je **Moje web-mjesto** web-mjesta. Kopiranje, kao što je naziv domene koji se koristi prilikom stvaranja CNAME zapis za potvrdu.

6. Prijavite se na web-mjestu registrara DNS-a, a zatim idite na stranicu za upravljanje DNS-a. Potražite veze ili područja web-mjesta koje su označene kao **Naziv domene**, **DNS-a**ili **Upravljanje naziv poslužitelja**.

6. Pronađite gdje možete odabrati ili unijeti A i CNAME zapisa. Možda ćete morati odaberite željenu vrstu zapisa iz programa padajuće prema dolje ili idite na stranicu Napredne postavke.

7. Poduzmite sljedeće korake da biste stvorili A zapis:

    1. Odaberite ili unesite domene i poddomene koji će koristiti A zapis. Ako želite stvoriti drugo ime za **www.customdomain.com**, na primjer, odaberite **"www"** . Ako želite stvoriti zamjenski unosa za sve poddomene unesite "__*__". To će obuhvaća sve poddomene kao što su **mail.customdomain.com**, **login.customdomain.com**i **www.customdomain.com**.

        Ako želite da biste stvorili A zapis za korijensku domenu, ona može biti navedena kao na '**@**' simbol u alatima za vaš registrar DNS-a.

    2. Unesite IP adresu servis u oblaku u navedeno polje. To povezuje stavku domene koriste u A zapis s IP adresom uvođenja servisa oblaka.

        Na primjer, sljedeće zapis prosljeđuje sav promet s **contoso.com** **137.135.70.239**, IP adresa naš distribuiranih aplikacije:

        <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
        <tr>
        <td><strong>Naziv glavnog računala/poddomene</strong></td>
        <td><strong>IP adresa</strong></td>
        </tr>
        <tr>
        <td>@</td>
        <td>137.135.70.239</td>
        </tr>
        </table>

        U ovom se primjeru pokazuje stvaranja A zapisa za korijensku domenu. Ako želite da biste stvorili unos zamjenskih da prekrije sve poddomene, unesite "__*__" kao poddomena.

7. Nakon toga stvorite CNAME zapis koji sadrži pseudonima od **awverify**i Kanonski domenu **awverify.mysite.azurewebsites.net** koji ste nabavili neke starije verzije.

    > [AZURE.NOTE] Radite na pseudonim awverify možda za neke registrare, drugim korisnicima biti potrebna pseudonim za puni naziv domene awverify.www.customdomainname.com ili awverify.customdomainname.com.

    Sljedeće će, na primjer, stvorite CNAME zapis koji Azure možete koristiti da biste provjerili zapis konfiguracije odgovora.

    <table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
    <tr>
    <td><strong>Naziv poddomene pseudonim/glavnog računala</strong></td>
    <td><strong>Kanonski domene</strong></td>
    </tr>
    <tr>
    <td>awverify</td>
    <td>awverify.contoso.azurewebsites.NET</td>
    </tr>
    </table>

> [AZURE.NOTE] Može potrajati neko vrijeme za awverify CNAME proširiti kroz DNS sustav. Ne možete postaviti naziv prilagođene domene definira A zapisa za web-mjesta dok propagira awverify CNAME. Servis kao što su <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> možete koristiti da biste provjerili je li dostupna na CNAME.

###<a name="add-the-domain-name-to-your-website"></a>Dodajte naziv domene web-mjestu

Kada propagira **awverify** CNAME zapis za naziv domene, zatim možete povezati prilagođenu domenu definira A zapis s web-mjesta. Možete dodati prilagođenu domenu definira A zapis za svoje web-mjesto pomoću EŽA Azure ili pomoću portala za upravljanje Azure.

**Da biste dodali naziv domene pomoću sučelja Azure naredbenog retka (Azure EŽA)**

Instaliranje i konfiguriranje [EŽA Azure](/manage/install-and-configure-cli/)i koristite sljedeću naredbu:

    azure site domain add customdomain yoursitename

Ako, na primjer, sljedeće će dodati prilagođeni naziv domene od **contoso.com** **contoso.azurewebsite.net** web-mjesto:

    azure site domain add contoso.com contoso

Možete potvrditi da se naziv prilagođene domene dodan na web-mjesto pomoću sljedeće naredbe:

    azure site domain list yoursitename

Na popisu vratio ta naredba mora sadržavati naziv prilagođene domene, kao i zadani **. azurewebsite.net** unosa.

**Da biste dodali naziv domene pomoću portala za upravljanje Azure**

1. U pregledniku otvorite [Portal za upravljanje Azure][portal].

2. Na kartici **web-mjesta** kliknite naziv web-mjesta, odaberite **nadzornu ploču**, a zatim **Upravljanje domenama** na dno stranice.

    ![][setcname2]

6. U tekstni okvir **Naziva domena** upišite naziv domene koji ste konfigurirali.

    ![][setcname3]

6. Kliknite kvačicu da biste prihvatili naziv domene.

Kada konfiguracije završi, naziv prilagođene domene će biti navedene u odjeljku **naziva domena** na stranici **Konfiguracija** web-mjesta.

> [AZURE.NOTE] Nakon što dodate prilagođenu domenu definira A zapis za svoje web-mjesto, možete ukloniti awverify CNAME zapis pomoću alata za koje ste dobili od registrara. Međutim, ako želite da biste dodali drugu A zapisa u budućnosti, morat ćete ponovno stvoriti u awverify zapis prije nego što možete pridružiti novi naziv domene definira novi zapis u web-mjesta.

## <a name="next-steps"></a>Daljnji koraci

-   [Upravljanje web-mjesta](/manage/services/web-sites/how-to-manage-websites/)

-   [Konfiguriranje SSL certifikata za web-mjesta](/develop/net/common-tasks/enable-ssl-web-site/)


<!-- Bookmarks -->

[Configure your web sites for shared mode]: #bkmk_configsharedmode
[Configure the CNAME on your domain registrar]: #bkmk_configurecname
[Configure a CNAME verification record on your domain registrar]: #bkmk_configurecname
[Configure an A record for the domain name]:#bkmk_configurearecord
[Set the domain name in management portal]: #bkmk_setcname

<!-- Links -->

[PricingDetails]: /pricing/details/
[portal]: http://manage.windowsazure.com
[digweb]: http://www.digwebinterface.com/
[cloudservicedns]: ../articles/custom-dns.md
[trafficmanager]: ../articles/app-service-web/web-sites-traffic-manager.md
[addendpoint]: ../articles/traffic-manager/traffic-manager-endpoints.md
[createprofile]: ../articles/traffic-manager/traffic-manager-manage-profiles.md

<!-- images -->

[setcname1]: ../media/dncmntask-cname-5.png


<!-- images -->
[standardmode1]: ./media/custom-dns-web-site/dncmntask-cname-1.png
[standardmode2]: ./media/custom-dns-web-site/dncmntask-cname-2.png
[standardmode3]: ./media/custom-dns-web-site/dncmntask-cname-3.png
[standardmode4]: ./media/custom-dns-web-site/dncmntask-cname-4.png


[setcname2]: ./media/custom-dns-web-site/dncmntask-cname-6.png
[setcname3]: ./media/custom-dns-web-site/dncmntask-cname-7.png
