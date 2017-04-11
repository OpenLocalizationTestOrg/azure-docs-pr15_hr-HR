<properties
    pageTitle="Prijava analitiku dizajner prikaza | Microsoft Azure"
    description="Dizajner prikaza u analize zapisnika omogućuje stvaranje prilagođenih prikaza na konzoli OMS koje sadrže različite vizualizacije podataka u spremištu OMS. U ovom se članku objašnjava postavki za svaku vizualizaciju dijelova dostupan za korištenje u prilagođene prikaze."
    services="log-analytics"
    documentationCenter=""
    authors="bwren"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-visualization-part-reference"></a>Zapisnik dizajner prikaza analize vizualizacije dijela reference
Dizajner prikaza u analize zapisnika omogućuje stvaranje prilagođenih prikaza na konzoli OMS koje sadrže različite vizualizacije podataka u spremištu OMS. U ovom se članku objašnjava postavki za svaku vizualizaciju dijelova dostupan za korištenje u prilagođene prikaze.

Ostali članci dostupne za dizajner prikaza su:

- [Dizajner prikaza](log-analytics-view-designer.md) – pregled dizajner prikaza i postupke za stvaranje i uređivanje prilagođene prikaze.
- [Pločica referenca](log-analytics-view-designer-tiles.md) – pregled postavki za svaku pločice dostupan za korištenje u prilagođene prikaze. 

U sljedećoj tablici opisane različite vrste pločice filtre za prikaz.  U odjeljcima opisuju svaku vrstu pločica u pojedinosti i njihova svojstva.

| Vrsta prikaza | Opis |
|:--|:--|
| [Popis upita](#list-of-queries-part) | Prikazuje popis upita za pretraživanje zapisnika.  Korisnik može kliknuti na svaki upit da biste prikazali rezultate.  |
| [Broj i popisa](#number-amp-list-part) | Zaglavlje ima jedan broj prikazuje broj zapisa iz upita za pretraživanje zapisnika.  Popis prikazuje gornjoj deset rezultata upita s grafikon koji označava relativni vrijednost numerički stupac ili njegov promjena tijekom vremena. |
| [Dvaju brojeva i popisa](#two-numbers-amp-list-part) | Zaglavlje ima dvaju brojeva na kojoj je prikazan zbroj zapise iz upita za pretraživanje zasebnom zapisnika.  Popis prikazuje gornji deset rezultata upita s grafikon koji označava relativni vrijednost numerički stupac ili njegov promjena tijekom vremena. |
| [Popis & prstenastih](#donut-amp-list-part) | Zaglavlje prikazuje samo jedan broj sažeta iz vrijednost stupca u upitu zapisnika.  Na prstenastih grafički prikazuje rezultate gornje tri zapisa. |
| [Dvije vremenske crte i popisa](#two-timelines-amp-list-part) | Zaglavlje prikazuje rezultate dva zapisnika upita tijekom vremena kao stupčasti grafikoni s oblačićem prikazuje samo jedan broj sažeta iz vrijednost stupca u upitu zapisnika.  Popis prikazuje gornji deset rezultata upita s grafikon koji označava relativni vrijednost numerički stupac ili njegov promjena tijekom vremena. |   
| [Informacije o](#information-part) | Zaglavlje prikazuje statički tekst i neobavezna vezu.  Popis prikazuje jednu ili više stavki s statički tekst i naslov. |
| [Linijski grafikon, oblačić i popisa](#line-chart-callout-amp-list-part) | Zaglavlje prikazuje linijski grafikon s više nizova iz zapisnika upita s vremenom i Oblačić s sažete vrijednosti.  Popis prikazuje gornji deset rezultata upita s grafikon koji označava relativni vrijednost numerički stupac ili njegov promjena tijekom vremena. |
| [Linijski grafikon i popisa](#line-chart-amp-list-part) | Zaglavlje prikazuje linijski grafikon s više nizova iz zapisnika upita tijekom vremena.  Popis prikazuje gornji deset rezultata upita s grafikon koji označava relativni vrijednost numerički stupac ili njegov promjena tijekom vremena. |
| [Stoga dijela linijski grafikoni](#stack-of-line-charts-part) | Prikazuje tri zasebna linijski grafikoni s više nizova iz zapisnika upita tijekom vremena. |


## <a name="list-of-queries-part"></a>Popis dijela upita

Prikazuje popis upita za pretraživanje zapisnika.  Korisnik može kliknuti na svaki upit da biste prikazali rezultate.  Prikaz neće sadržavati jedan upita prema zadanim postavkama, a zatim kliknuti **+ upita** za dodavanje dodatnih upita.

![Popis prikaz upita](media/log-analytics-view-designer/view-list-queries.png)

| Postavka | Opis |
|:--|:--|
| **Općenito** |
| Naslov | Tekst koji se prikazuje pri vrhu prikaza. |
| Nova grupa | Odaberite za stvaranje nove grupe u prikazu počevši od trenutnog prikaza. |
| Unaprijed odabranih filtara | Razgraničene popis svojstava za uključivanje u oknu s lijeve filtar kada korisnik odabere upita. |
| Prikaz način rada | Početni prikaz koji se prikazuje kada je upit odabran.  Korisnik može odabrati neki dostupni prikazi nakon otvaranja upit. |
| **Upiti** |
| Upit za pretraživanje | Upit za pokretanje. |
| Neslužbeni naziv | Opisni naziv upita koji želite prikazati korisniku. |


## <a name="number--list-part"></a>Broj & popis dijela

Zaglavlje ima jedan broj prikazuje broj zapisa iz upita za pretraživanje zapisnika.  Popis prikazuje gornji deset rezultata upita s grafikon koji označava relativni vrijednost numerički stupac ili njegov promjena tijekom vremena.


![Popis prikaz upita](media/log-analytics-view-designer/view-number-list.png)

| Postavka | Opis |
|:--|:--|
| **Općenito** |
| Naslov grupe | Tekst koji se prikazuje pri vrhu prikaza. |
| Nova grupa | Odaberite za stvaranje nove grupe u prikazu počevši od trenutnog prikaza. |
| Ikona | Slikovna datoteka da biste prikazali pokraj rezultata u zaglavlju.
| Pomoću ikone | Odaberite kako bi prikaz ikone. |
| **Naslov** |
| Legende | Tekst koji se prikazuje pri vrhu zaglavlje. |
| Upit | Upit da biste pokrenuli za zaglavlje.  Prikazat će se broj broj zapisa je vratio upit. |
| **Popis** |
| Upit | Upit da biste pokrenuli za popis.  Prikazat će se prve dvije svojstva za prvo deset zapise u rezultatima.  Prvo svojstvo mora biti tekstne vrijednosti i svojstvo drugi brojčana vrijednost.  Stupci automatski stvaraju na relativnu vrijednosti numerički stupac.<br><br>Sortiranje zapisa na popisu pomoću naredbe za sortiranje u upitu.  Korisnik može kliknuti vidjeti sve da biste pokrenuli upit i vraćanje svih zapisa. |
| Skrivanje grafikonu | Odaberite da biste onemogućili grafikonu s desne strane numerički stupac. |
| Omogućivanje minigrafikona | Odaberite da biste prikazali minigrafikona umjesto vodoravna traka.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Boja | Boja traka ili minigrafikona. |
| Naziv razdjelnik vrijednost | Znak razdjelnika ako želite analizirati svojstvo teksta u više vrijednosti.  Detalje potražite u članku [Uobičajene postavke](#name-value-separator) . |
| Upit za navigaciju | Upit da biste pokrenuli kada korisnik odabere stavku na popisu.  Detalje potražite u članku [Uobičajene postavke](#navigation-query) . |
| **Popis** | **> Naslova stupaca** |
| Ime | Tekst koji se prikazuje pri vrhu prvi stupac na popisu. |
| Vrijednost | Tekst koji se prikazuje pri vrhu drugog stupca na popisu. |
| **Popis** | **> Pragovi** |
| Omogućivanje pragovi | Odaberite želite li omogućiti pragovi.  Detalje potražite u članku [Uobičajene postavke](#thresholds) . |


## <a name="two-numbers--list-part"></a>Dvaju brojeva i dijelu popisa

Zaglavlje ima dvaju brojeva na kojoj je prikazan zbroj zapise iz upita za pretraživanje zasebnom zapisnika.  Popis prikazuje gornjoj deset rezultata upita s grafikon koji označava relativni vrijednost numerički stupac ili njegov promjena tijekom vremena.

![Dvaju brojeva i prikaz popisa](media/log-analytics-view-designer/view-two-numbers-list.png)

| Postavka | Opis |
|:--|:--|
| **Općenito** |
| Naslov grupe | Tekst koji se prikazuje pri vrhu prikaza. |
| Nova grupa | Odaberite za stvaranje nove grupe u prikazu počevši od trenutnog prikaza. |
| Ikona | Slikovna datoteka da biste prikazali pokraj rezultata u zaglavlju.
| Pomoću ikone | Odaberite kako bi prikaz ikone. |
| **Naslov** |
| Legende | Tekst koji se prikazuje pri vrhu zaglavlje. |
| Upit | Upit da biste pokrenuli za zaglavlje.  Prikazat će se broj broj zapisa je vratio upit. |
| **Popis** |
| Upit | Upit da biste pokrenuli za popis.  Prikazat će se prve dvije svojstva za prvo deset zapise u rezultatima.  Prvo svojstvo mora biti tekstne vrijednosti i svojstvo drugi brojčana vrijednost.  Stupci automatski stvaraju na relativnu vrijednosti numerički stupac.<br><br>Sortiranje zapisa na popisu pomoću naredbe za sortiranje u upitu.  Korisnik može kliknuti vidjeti sve da biste pokrenuli upit i vraćanje svih zapisa. |
| Skrivanje grafikonu | Odaberite da biste onemogućili grafikonu s desne strane numerički stupac. |
| Omogućivanje minigrafikona | Odaberite da biste prikazali minigrafikona umjesto vodoravna traka.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Boja | Boja traka ili minigrafikona. |
| Postupak | Postupak za minigrafikon.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Naziv razdjelnik vrijednost | Znak razdjelnika ako želite analizirati svojstvo teksta u više vrijednosti.  Detalje potražite u članku [Uobičajene postavke](#name-value-separator) . |
| Upit za navigaciju | Upit da biste pokrenuli kada korisnik odabere stavku na popisu.  Detalje potražite u članku [Uobičajene postavke](#navigation-query) . |
| **Popis** | **> Naslova stupaca** |
| Ime | Tekst koji se prikazuje pri vrhu prvi stupac na popisu. |
| Vrijednost | Tekst koji se prikazuje pri vrhu drugog stupca na popisu. |
| **Popis** | **> Pragovi** |
| Omogućivanje pragovi | Odaberite želite li omogućiti pragovi.  Detalje potražite u članku [Uobičajene postavke](#thresholds) . |

## <a name="donut--list-part"></a>Popis & prstenastih dijela

Zaglavlje prikazuje samo jedan broj sažeta iz vrijednost stupca u upitu zapisnika.  Na prstenastih grafički prikazuje rezultate gornje tri zapisa.

![Prikaz prstenastih i popisa](media/log-analytics-view-designer/view-donut-list.png)

| Postavka | Opis |
|:--|:--|
| **Općenito** |
| Naslov grupe | Tekst koji se prikazuje pri vrhu pločice. |
| Nova grupa | Odaberite za stvaranje nove grupe u prikazu počevši od trenutnog prikaza. |
| Ikona | Slikovna datoteka da biste prikazali pokraj rezultata u zaglavlju. |
| Pomoću ikone | Odaberite kako bi prikaz ikone. |
| **Zaglavlje** |
| Naslov | Tekst koji se prikazuje pri vrhu zaglavlje.
| Podnaslov | Tekst koji se prikazuje ispod naslova pri vrhu zaglavlje.
| **Prstenastih** |
| Upit | Upit da biste pokrenuli za na prstenastih.  Prvo svojstvo mora biti tekstne vrijednosti i svojstvo drugi brojčana vrijednost. |
| **Prstenastih** |  **> Centar za** |
| Tekst | Tekst koji se prikazuje u odjeljku vrijednosti unutar na prstenastih. |
| Postupak | Postupak da biste izvršili u svojstvu vrijednost za zbrajanje jednu vrijednost.<br><br>-Zbroj: Dodajte vrijednosti sve zapise.<br>– Postotak: Postotak zapisa vratila vrijednosti u **rezultat vrijednosti koje se koriste u operaciji centar za** Ukupno zapisa u upitu. |
| Rezultat vrijednosti koje se koriste u Centar za postupak | Po želji kliknite znak plus da biste dodali jednu ili više vrijednosti.  Rezultati upita bit će ograničeni na zapisa koji sadrže vrijednosti nekretnina koju navedete.  Ako se dodaju nikakve vrijednosti, svi zapisi uključeni u upitu. |
| **Dodatne mogućnosti** | **> Boja** |
| Boja 1<br>Boja 2<br>Boja 3 | Odaberite boju za svaku od vrijednosti prikazane u na prstenastih. |
| **Dodatne mogućnosti** | **> Preslikavanje Napredno boja** |
| Vrijednost polja | Upišite naziv polja da biste ga prikazali kao neku drugu boju, ako je obuhvaćen na prstenastih. |
| Boja | Odaberite boju za polje jedinstveni. |
| **Popis** |
| Upit | Upit da biste pokrenuli za popis.  Prikazat će se broj broj zapisa je vratio upit. |
| Skrivanje grafikonu | Odaberite da biste onemogućili grafikonu s desne strane numerički stupac. |
| Omogućivanje minigrafikona | Odaberite da biste prikazali minigrafikona umjesto vodoravna traka.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Boja | Boja traka ili minigrafikona. |
| Postupak | Postupak za minigrafikon.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Naziv razdjelnik vrijednost | Znak razdjelnika ako želite analizirati svojstvo teksta u više vrijednosti.  Detalje potražite u članku [Uobičajene postavke](#name-value-separator) . |
| Upit za navigaciju | Upit da biste pokrenuli kada korisnik odabere stavku na popisu.  Detalje potražite u članku [Uobičajene postavke](#navigation-query) . |
| **Popis** | **> Naslova stupaca** |
| Ime | Tekst koji se prikazuje pri vrhu prvi stupac na popisu. |
| Vrijednost | Tekst koji se prikazuje pri vrhu drugog stupca na popisu. |
| **Popis** | **> Pragovi** |
| Omogućivanje pragovi | Odaberite želite li omogućiti pragovi.  Detalje potražite u članku [Uobičajene postavke](#thresholds) . |

## <a name="two-timelines--list-part"></a>Dva dijela vremenskih crta i popisa

Zaglavlje prikazuje rezultate dva zapisnika upita tijekom vremena kao stupčasti grafikoni s oblačićem prikazuje samo jedan broj sažeta iz vrijednost stupca u upitu zapisnika.  Popis prikazuje gornjoj deset rezultata upita s grafikon koji označava relativni vrijednost numerički stupac ili njegov promjena tijekom vremena.

![Prikaz dvije vremenske crte i popisa](media/log-analytics-view-designer/view-two-timelines-list.png)

| Postavka | Opis |
|:--|:--|
| **Općenito** |
| Naslov grupe | Tekst koji se prikazuje pri vrhu pločice. |
| Nova grupa | Odaberite za stvaranje nove grupe u prikazu počevši od trenutnog prikaza. |
| Ikona | Slikovna datoteka da biste prikazali pokraj rezultata u zaglavlju. |
| Pomoću ikone | Odaberite kako bi prikaz ikone. |
| **Najprije grafikona<br>drugoj grafikona** |
| Legende | Tekst koji se prikazuje u odjeljku oblačić za prvi niz. |
| Boja | Boju koju želite koristiti za stupce u nizu. |
| Upit | Upit da biste pokrenuli za prvi niz.  Broj broj zapisa preko svake vremenski interval će predstavljati stupcima grafikona. |
| Postupak | Postupak da biste izvršili u svojstvu vrijednost za sažimanje jedne vrijednosti za oblačić.<br><br>– Zbroj: Zbroj vrijednosti iz svih zapisa.<br>-Prosjek: Prosjek vrijednosti iz svih zapisa.<br>-Zadnjeg uzorak: Vrijednost iz zadnjeg interval uključen u grafikonu.<br>– Prvi primjer: Vrijednosti iz prvog interval uključen u grafikonu.<br>-Broj: Brojanje svih zapisa je vratio upit.|
| **Popis** |
| Upit | Upit da biste pokrenuli za popis.  Prikazat će se broj broj zapisa je vratio upit. |
| Skrivanje grafikonu | Odaberite da biste onemogućili grafikonu s desne strane numerički stupac. |
| Omogućivanje minigrafikona | Odaberite da biste prikazali minigrafikona umjesto vodoravna traka.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Boja | Boja traka ili minigrafikona. |
| Postupak | Postupak za minigrafikon.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Upit za navigaciju | Upit da biste pokrenuli kada korisnik odabere stavku na popisu.  Detalje potražite u članku [Uobičajene postavke](#navigation-query) . |
| **Popis** | **> Naslova stupaca** |
| Ime | Tekst koji se prikazuje pri vrhu prvi stupac na popisu. |
| Vrijednost | Tekst koji se prikazuje pri vrhu drugog stupca na popisu. |
| **Popis** | **> Pragovi** |
| Omogućivanje pragovi | Odaberite želite li omogućiti pragovi.  Detalje potražite u članku [Uobičajene postavke](#thresholds) . |

## <a name="information-part"></a>Dio podataka

Zaglavlje prikazuje statički tekst i neobavezna vezu.  Popis prikazuje jednu ili više stavki s statički tekst i naslov.

![Prikaz informacija](media/log-analytics-view-designer/view-information.png)

| Postavka | Opis |
|:--|:--|
| **Općenito** |
| Naslov grupe | Tekst koji se prikazuje pri vrhu pločice. |
| Nova grupa | Odaberite za stvaranje nove grupe u prikazu počevši od trenutnog prikaza. |
| Boja | Boja pozadine za zaglavlje. |
| **Zaglavlje** |
| Slika | Slikovna datoteka da biste prikazali u zaglavlju. |
| Oznaka | Tekst koji se prikazuje u zaglavlju. |
| **Zaglavlje** | **> Veze** |
| Oznaka | Tekst veze. |
| URL-a | URL za vezu. |
| **Stavke informacija** |
| Naslov | Tekst koji se prikazuje za naslov svake stavke. |
| Sadržaj | Tekst koji se prikazuje za svaku stavku. |


## <a name="line-chart-callout--list-part"></a>Linijski grafikon, oblačić i dijelu popisa

Zaglavlje prikazuje linijski grafikon s više nizova iz zapisnika upita s vremenom i Oblačić s sažete vrijednosti.  Popis prikazuje gornjoj deset rezultata upita s grafikon koji označava relativni vrijednost numerički stupac ili njegov promjena tijekom vremena.

![Linijski grafikon, oblačić i prikaza popisa](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Postavka | Opis |
|:--|:--|
| **Općenito** |
| Naslov grupe | Tekst koji se prikazuje pri vrhu pločice. |
| Nova grupa | Odaberite za stvaranje nove grupe u prikazu počevši od trenutnog prikaza. |
| Ikona | Slikovna datoteka da biste prikazali pokraj rezultata u zaglavlju. |
| Pomoću ikone | Odaberite kako bi prikaz ikone. |
| **Zaglavlje** |
| Naslov | Tekst koji se prikazuje pri vrhu zaglavlje. |
| Podnaslov | Tekst koji se prikazuje ispod naslova pri vrhu zaglavlje. |
| **Linijski grafikon** |
| Upit | Upit da biste pokrenuli za linijski grafikon.  Prvo svojstvo mora biti tekstne vrijednosti i svojstvo drugi brojčana vrijednost.  To je obično upit koji koristi ključnu riječ **mjere** za zbrajanje rezultate.  Ako upit koristi ključnu riječ **interval** osi na grafikonu će koristiti ovu vremenski interval.  Ako je upit sadrže ključnu riječ **interval** svaki sat intervalima koriste za os x. |
| **Linijski grafikon** | **> Oblačića** |
| Oblačić naslov | Tekst za prikaz iznad vrijednosti oblačić. |
| Naziv skupa podataka | Vrijednost svojstva za niz koji želite koristiti za vrijednost oblačić.  Ako nema niz nije naveden, koriste se sve zapise iz upita. |
| Postupak | Postupak da biste izvršili u svojstvu vrijednost za sažimanje jedne vrijednosti za oblačić.<br><br>-Prosjek: Prosjek vrijednosti iz svih zapisa.<br>– Count Brojanje svih zapisa je vratio upit.<br>-Zadnjeg uzorak: Vrijednost iz zadnjeg interval uključen u grafikonu.<br>-Max: Maksimalna vrijednost s vremenskim razmacima uključeni u grafikonu.<br>-Min: Minimalne vrijednosti s vremenskim razmacima uključeni u grafikonu.<br>– Zbroj: Zbroj vrijednosti iz svih zapisa. |
| **Linijski grafikon** | **> Os Y** |
| Korištenje Logaritamska skala | Odaberite da biste koristili Logaritamsko mjerilo za os y. |
| Jedinice | Navedite jedinicu vrijednosti koji je vratio upit.  Ti podaci se koristi da biste prikazali oznake na grafikonu koji označava vrstu vrijednosti, a po želji za pretvaranje vrijednosti.  Jedinica Vrsta određuje kategoriju jedinice i definira trenutna jedinica vrsta vrijednosti koje su dostupne.  Ako odaberete vrijednost u pretvoriti, a zatim numeričke vrijednosti se pretvaraju u trenutnoj jedinici vrsti Pretvori u upišite. |
| Prilagođene naljepnice | Tekst koji se prikazuje za os Y uz natpis za vrstu jedinicu.  Ako nema oznake nije naveden, prikazuje se samo vrsta jedinicu. |
| **Popis** |
| Upit | Upit da biste pokrenuli za popis.  Prikazat će se broj broj zapisa je vratio upit. |
| Skrivanje grafikonu | Odaberite da biste onemogućili grafikonu s desne strane numerički stupac. |
| Omogućivanje minigrafikona | Odaberite da biste prikazali minigrafikona umjesto vodoravna traka.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Boja | Boja traka ili minigrafikona. |
| Postupak | Postupak za minigrafikon.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Naziv razdjelnik vrijednost | Znak razdjelnika ako želite analizirati svojstvo teksta u više vrijednosti.  Detalje potražite u članku [Uobičajene postavke](#name-value-separator) . |
| Upit za navigaciju | Upit da biste pokrenuli kada korisnik odabere stavku na popisu.  Detalje potražite u članku [Uobičajene postavke](#navigation-query) . |
| **Popis** | **> Naslova stupaca** |
| Ime | Tekst koji se prikazuje pri vrhu prvi stupac na popisu. |
| Vrijednost | Tekst koji se prikazuje pri vrhu drugog stupca na popisu. |
| **Popis** | **> Pragovi** |
| Omogućivanje pragovi | Odaberite želite li omogućiti pragovi.  Detalje potražite u članku [Uobičajene postavke](#thresholds) . |

## <a name="line-chart--list-part"></a>Linijski grafikon i popis dijela

Zaglavlje prikazuje linijski grafikon s više nizova iz zapisnika upita tijekom vremena.  Popis prikazuje gornjoj deset rezultata upita s grafikon koji označava relativni vrijednost numerički stupac ili njegov promjena tijekom vremena.

![Linijski grafikon i popis prikaz](media/log-analytics-view-designer/view-line-chart-callout-list.png)

| Postavka | Opis |
|:--|:--|
| **Općenito** |
| Naslov grupe | Tekst koji se prikazuje pri vrhu pločice. |
| Nova grupa | Odaberite za stvaranje nove grupe u prikazu počevši od trenutnog prikaza. |
| Ikona | Slikovna datoteka da biste prikazali pokraj rezultata u zaglavlju. |
| Pomoću ikone | Odaberite kako bi prikaz ikone. |
| **Zaglavlje** |
| Naslov | Tekst koji se prikazuje pri vrhu zaglavlje. |
| Podnaslov | Tekst koji se prikazuje ispod naslova pri vrhu zaglavlje. |
| **Linijski grafikon** |
| Upit | Upit da biste pokrenuli za linijski grafikon.  Prvo svojstvo mora biti tekstne vrijednosti i svojstvo drugi brojčana vrijednost.  To je obično upit koji koristi ključnu riječ **mjere** za zbrajanje rezultate.  Ako upit koristi ključnu riječ **interval** osi na grafikonu će koristiti ovu vremenski interval.  Ako je upit sadrže ključnu riječ **interval** svaki sat intervalima koriste za os x. |
| **Linijski grafikon** | **> Os Y** |
| Korištenje Logaritamska skala | Odaberite da biste koristili Logaritamsko mjerilo za os y. |
| Jedinice | Navedite jedinicu vrijednosti koji je vratio upit.  Ti podaci se koristi da biste prikazali oznake na grafikonu koji označava vrstu vrijednosti, a po želji za pretvaranje vrijednosti.  Jedinica Vrsta određuje kategoriju jedinice i definira trenutna jedinica vrsta vrijednosti koje su dostupne.  Ako odaberete vrijednost u pretvoriti, a zatim numeričke vrijednosti se pretvaraju u trenutnoj jedinici vrsti Pretvori u upišite. |
| Prilagođene naljepnice | Tekst koji se prikazuje za os Y uz natpis za vrstu jedinicu.  Ako nema oznake nije naveden, prikazuje se samo vrsta jedinicu. |
| **Popis** |
| Upit | Upit da biste pokrenuli za popis.  Prikazat će se broj broj zapisa je vratio upit. |
| Skrivanje grafikonu | Odaberite da biste onemogućili grafikonu s desne strane numerički stupac. |
| Omogućivanje minigrafikona | Odaberite da biste prikazali minigrafikona umjesto vodoravna traka.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Boja | Boja traka ili minigrafikona. |
| Postupak | Postupak za minigrafikon.  Detalje potražite u članku [Uobičajene postavke](#sparklines) . |
| Naziv razdjelnik vrijednost | Znak razdjelnika ako želite analizirati svojstvo teksta u više vrijednosti.  Detalje potražite u članku [Uobičajene postavke](#name-value-separator) . |
| Upit za navigaciju | Upit da biste pokrenuli kada korisnik odabere stavku na popisu.  Detalje potražite u članku [Uobičajene postavke](#navigation-query) . |
| **Popis** | **> Naslova stupaca** |
| Ime | Tekst koji se prikazuje pri vrhu prvi stupac na popisu. |
| Vrijednost | Tekst koji se prikazuje pri vrhu drugog stupca na popisu. |
| **Popis** | **> Pragovi** |
| Omogućivanje pragovi | Odaberite želite li omogućiti pragovi.  Detalje potražite u članku [Uobičajene postavke](#thresholds) . |

## <a name="stack-of-line-charts-part"></a>Stoga dijela linijski grafikoni

Prikazuje tri zasebna linijski grafikoni s više nizova iz zapisnika upita tijekom vremena.

![Stoga linijskih grafikona](media/log-analytics-view-designer/view-stack-line-charts.png)

| Postavka | Opis |
|:--|:--|
| **Općenito** |
| Naslov grupe | Tekst koji se prikazuje pri vrhu pločice. |
| Nova grupa | Odaberite za stvaranje nove grupe u prikazu počevši od trenutnog prikaza. |
| Ikona | Slikovna datoteka da biste prikazali pokraj rezultata u zaglavlju. |
| **Grafikon 1<br>grafikona 2<br>grafikona 3** | **> Zaglavlje** |
| Naslov | Tekst koji se prikazuje pri vrhu grafikona. |
| Podnaslov | Tekst koji se prikazuje ispod naslova pri vrhu grafikona. |
| **Grafikon 1<br>grafikona 2<br>grafikona 3** | **Linijski grafikon** |
| Upit | Upit da biste pokrenuli za linijski grafikon.  Prvo svojstvo mora biti tekstne vrijednosti i svojstvo drugi brojčana vrijednost.  To je obično upit koji koristi ključnu riječ **mjere** za zbrajanje rezultate.  Ako upit koristi ključnu riječ **interval** osi na grafikonu će koristiti ovu vremenski interval.  Ako je upit sadrže ključnu riječ **interval** svaki sat intervalima koriste za os x. |
| **Grafikon** | **> Os Y** |
| Korištenje Logaritamska skala | Odaberite da biste koristili Logaritamsko mjerilo za os y. |
| Jedinice | Navedite jedinicu vrijednosti koji je vratio upit.  Ti podaci se koristi da biste prikazali oznake na grafikonu koji označava vrstu vrijednosti, a po želji za pretvaranje vrijednosti.  Jedinica Vrsta određuje kategoriju jedinice i definira trenutna jedinica vrsta vrijednosti koje su dostupne.  Ako odaberete vrijednost u pretvoriti, a zatim numeričke vrijednosti se pretvaraju u trenutnoj jedinici vrsti Pretvori u upišite. |
| Prilagođene naljepnice | Tekst koji se prikazuje za os Y uz natpis za vrstu jedinicu.  Ako nema oznake nije naveden, prikazuje se samo vrsta jedinicu. |

## <a name="common-settings"></a>Uobičajene postavke
U sljedećim odjeljcima opisuju zajedničke postavke za nekoliko dijelova vizualizaciju.

### <a name="name-value-separator">Naziv razdjelnik vrijednost</a>
Znak razdjelnika ako želite analizirati svojstvo tekst iz popisa upita u više vrijednosti.  Ako navedete razdjelnika, možete unijeti nazivi za svako polje odvojene isti graničnik u okvir naziv.

Na primjer, razmotrite svojstvo *mjesto* uvrštene vrijednosti kao što su *Redmond sastavnih 41* i *S Building12*.  Možete navesti – za ime i razdjelnik vrijednost i *Grad sastavnih* naziv.  To će raščlaniti svake vrijednosti u dva svojstva pod nazivom *grada* i *sastavnih*. 

### <a name="navigation-query">Upit za navigaciju</a>
Upit da biste pokrenuli kada korisnik odabere stavku na popisu.  Da biste uključili vidjet ćete da sintaksa za stavke koje je korisnik odabrati pomoću *{odabrane stavke}* .

Ako, na primjer, upit ima li stupac pod nazivom *računala* i navigacije upita je *{odabrane stavke}*, upita kao što su *računala = "Mojeracunalo"* želite pokrenuti kada korisnik odabere računala.  Ako je upit navigacijskoj *Vrsta = događaj {odabrane stavke}* zatim upit *Vrsta = događaj računala = "Mojeracunalo"* želite pokrenuti.

### <a name="sparklines">Minigrafikona</a>
Minigrafikon je small linijski grafikon koji prikazuje vrijednost unos popisa tijekom vremena.  Za vizualizaciju dijelove s popisa, možete odabrati želite li prikazati vodoravno trakom koja upućuje relativni vrijednost numerički stupac ili minigrafikona koji označava njegov vrijednosti tijekom vremena.

U sljedećoj su tablici opisane postavke za minigrafikone.

| Postavka | Opis |
|:--|:--|
| Omogućivanje minigrafikona | Odaberite da biste prikazali minigrafikona umjesto vodoravna traka. |
| Postupak | Ako je omogućeno minigrafikona, to je operaciju koja će se izvoditi na svakom svojstvo na popisu da biste izračunali vrijednosti za minigrafikon.<br><br>-Zadnje uzorak: Zadnje vrijednost niza putem razdoblje.<br>-Max: Maksimalna vrijednost niza putem razdoblje.<br>-Min: Minimalna vrijednost niza putem razdoblje.<br>– Zbroj: Zbroj vrijednosti niza na odabranom vremenskom razdoblju.<br>-Sažetak: Koristi ista naredba mjera kao upit u zaglavlju. |

### <a name="thresholds">Pragovi</a>
Pragovi omogućuju prikaz boja ikone pokraj svake stavke na popisu što vam omogućuje brzi vizualni indikator stavki koje premašuju određenu vrijednost ili ulaze unutar određenog raspona.  Ako, na primjer, zelenu ikonu za stavke s prihvatljiva vrijednost, žutu ako je vrijednost u rasponu koji označava upozorenje i crvene boje možete prikazati ako premašuje vrijednosti pogreške.

Kada omogućite pragovi dijela, morate navesti jednu ili više pragovi.  Ako je vrijednost artikla veći od vrijednosti praga i manja od sljedeću vrijednost praga, zatim ta se boja koristi.  Ako je stavka je veći od zatim najveću vrijednost praga, postavlja se te boje.   

Svaki skup praga sadrži jedan praga **Zadana**je vrijednost.  To je skup ako nema vrijednosti su prekoračena je boja.  Možete dodati ili ukloniti pragovi tako da kliknete na **+** ili gumb **x** .

U sljedećoj tablici opisane postavke za tresholds.

| Postavka | Opis |
|:--|:--|
| Omogućivanje pragovi | Odaberite za prikaz boja ikonu s lijeve strane svake vrijednosti koji označava njegov stanja odnosu navedeni pragovi. |
| Ime | Naziv da biste odredili vrijednosti praga. |
| Prag | Vrijednost za praga.  Boja stanja za svaku stavku popisa se postavlja boje najveću vrijednost praga premašuje vrijednost stavke.  Postoji jedan zadani Prag je boja ako se premaše nema vrijednosti praga. |
| Boja | Boja za vrijednosti praga. |


## <a name="next-steps"></a>Daljnji koraci

- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) za podršku upite u dijelovima vizualizaciju.