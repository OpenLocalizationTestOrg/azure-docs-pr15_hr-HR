Kada ste propagira zapise za naziv domene, morate ih povezati s web-aplikaciju programa. Poduzmite sljedeće korake da biste omogućili nazivi domena pomoću web-preglednika.

> [AZURE.NOTE] Može potrajati neko vrijeme za stvorenih u prethodnim koracima proširiti kroz DNS sustav TXT zapisa. Naziv domene ne možete dodati na web-aplikaciju dok propagira TXT zapis. Ako koristite A zapis, ne možete dodati naziv zapisa domene odgovora na web-aplikaciju dok propagira TXT zapis stvorili u prethodnom koraku.
>
> Servis kao što su <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> možete koristiti da biste provjerili je li dostupna TXT zapis.

1. U pregledniku otvorite [Azure Portal](https://portal.azure.com).

2. Na kartici **Web-aplikacije** kliknite naziv web-aplikacije, a zatim odaberite **prilagođenih domena**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. U plohu **prilagođenih domena** kliknite **Dodavanje naziv glavnog računala**.
    
4. Pomoću tekstne okvire **naziv glavnog računala** unesite nazivi domena želite pridružiti web aplikacije.

    ![](./media/custom-dns-web-site/add-custom-domain.png)

6.  Kliknite **Provjeri valjanost**.

7.  Nakon kliknete **Provjera** Azure će izbaciti domena uspješno tijeka rada. To će provjeru vlasništvo nad domenom kao i dostupnosti i izvješća uspjeh naziv glavnog računala ili detaljne pogreška s prescriptive vodženje kako ispraviti pogrešku.    

Sada, trebali biste moći unesite naziv prilagođene domene u pregledniku, a zatim potražite u članku da je uspješno vodi vas na web-aplikaciju.
