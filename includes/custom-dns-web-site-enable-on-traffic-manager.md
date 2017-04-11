Kada ste propagira zapise za naziv domene, trebali biste moći pomoću preglednika da biste potvrdili da se prilagođeni naziv domene može koristiti za pristup aplikaciji aplikacije servisa za Azure web.

> [AZURE.NOTE] Može potrajati neko vrijeme za vaše CNAME proširiti kroz DNS sustav. Servis kao što su <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> možete koristiti da biste provjerili je li dostupna na CNAME.

Ako još niste dodali web-aplikaciju programa kao krajnja točka Upravitelj promet, to morate učiniti prije razlučivanje naziva će funkcionirati, kao naziv usmjerava prilagođenu domenu upravitelju promet. Upravitelj promet pa se preusmjerava na web-aplikaciju. Da biste dodali web-aplikaciju programa kao krajnje točke u profilu programa Upravitelj promet pomoću informacija u [Dodavanje ili brisanje krajnje točke](../articles/traffic-manager/traffic-manager-endpoints.md) .

> [AZURE.NOTE] Ako web-aplikaciju programa nije naveden prilikom dodavanja krajnje, provjerite je li konfiguriran za **standardne** aplikacije servisa za planiranje način. Da bi rad s programom Manager promet morate koristiti **standardnom** načinu rada za web-aplikacije.

1. U pregledniku otvorite [Azure Portal](https://portal.azure.com).

1. Na kartici **Web-aplikacije** kliknite naziv web-aplikacije, odaberite **Postavke**, a zatim odaberite **prilagođenih domena**

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

1. U plohu **prilagođenih domena** kliknite **Dodavanje naziv glavnog računala**.
    
1. Pomoću tekstne okvire **naziv glavnog računala** unesite naziv domene promet Upravitelj želite pridružiti web aplikacije.

    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)

1. Kliknite **Provjeri valjanost** da biste spremili konfiguracijom naziv domene.

7.  Nakon kliknete **Provjera** Azure će izbaciti domena uspješno tijeka rada. To će provjeru vlasništvo nad domenom kao i dostupnosti i izvješća uspjeh naziv glavnog računala ili detaljne pogreška s prescriptive vodženje kako ispraviti pogrešku.    

8.  Nakon uspješne provjere **Dodaj naziv glavnog računala** gumb postaje aktivna i moći ćete dodijelite naziv glavnog računala. Sada pomaknite se do prilagođeni naziv domene u pregledniku. Prikazat će se sada vaše pokretanje aplikacije pomoću prilagođeni naziv domene. 

    Kada konfiguracije završi, naziv prilagođene domene će biti navedene u odjeljku **naziva domena** web-aplikacije.

Sada, trebali biste moći unesite naziv za naziv domene Upravitelj promet u pregledniku, a zatim potražite u članku da je uspješno vodi vas na web-aplikaciju.
