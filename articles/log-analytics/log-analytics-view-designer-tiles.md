<properties
    pageTitle="Prijava analitički prikaz dizajnera pločica referenca | Microsoft Azure"
    description="Dizajner prikaza u analize zapisnika omogućuje stvaranje prilagođenih prikaza na konzoli OMS koje sadrže različite vizualizacije podataka u spremištu OMS. U ovom se članku objašnjava postavki za svaku pločice dostupan za korištenje u prilagođene prikaze."
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
    ms.date="09/27/2016"
    ms.author="bwren"/>

# <a name="log-analytics-view-designer-tile-reference"></a>Zapisnik analitički prikaz dizajnera pločica referenca
Dizajner prikaza u analize zapisnika omogućuje stvaranje prilagođenih prikaza na konzoli OMS koje sadrže različite vizualizacije podataka u spremištu OMS. U ovom se članku objašnjava postavki za svaku pločice dostupan za korištenje u prilagođene prikaze.

Ostali članci dostupne za dizajner prikaza su:

- [Dizajner prikaza](log-analytics-view-designer.md) – pregled dizajner prikaza i postupke za stvaranje i uređivanje prilagođene prikaze.
- [Referenca dio vizualizacije](log-analytics-view-designer-parts.md) – pregled postavki za svaku pločice dostupan za korištenje u prilagođene prikaze. 


U sljedećoj su tablici navedene različite vrste pločice filtre za prikaz.  U odjeljcima opisuju svaku vrstu pločica u pojedinosti i njihova svojstva.

| Pločica | Opis |
|:--|:--|
| [Broj](#number-tile) | Jedan broj prikazuje broj zapisa iz upita. |
| [Dvaju brojeva.](#two-numbers-tile) | Prikaz brojanja zapise iz dvije različite upita je dvaju brojeva jedan. |
| [Prstenastih](#donut-tile) | Prstenastih grafikon koji se temelji na upit s vrijednost zbroja u centru za. |
| [Linijski grafikon i oblačića](#line-chart-amp-callout-tile) | Linijski grafikon koji se temelji na upitu i oblačića sažetak vrijednošću. |
| [Linijski grafikon](#line-chart-tile) | Linijski grafikon koji se temelje na upitu. |
| [Dvije vremenske crte](#two-timelines-tile) | Stupčasti grafikon s dva niza u svakom na temelju zaseban upit. |



## <a name="number-tile"></a>Broj pločica

Pločica **broj** prikazuje samo jedan broj na kojoj je prikazan zbroj zapise iz zapisnika upita i natpis.

![Broj pločica](media/log-analytics-view-designer/tile-number.png)

| Postavka | Opis |
|:--|:--|
| Ime        | Tekst koji se prikazuje pri vrhu pločice. |
| Opis | Tekst koji se prikazuje u odjeljku naziv pločice.    |
| **Pločica** |
| Legende | Tekst koji se prikazuje u odjeljku vrijednost. |
| Upit | Upit za pokretanje.  Prikazat će se broj broj zapisa je vratio upit. |
| **Napredno** |  **> Provjera toka podataka** |
| Omogućeno | Ako provjera toka podataka mora biti omogućen za pločicu, odaberite.  To omogućuje zamjenske poruku ako podaci nisu dostupni za pločice.  Ovo se obično koristi za pružanje poruke tijekom razdoblja privremene kada je instaliran prikaza i dolaze podaci dostupni. |
| Upit | Upit za pokretanje da biste provjerili je li podaci su dostupni za prikaz.  Ako je upit vraća bez rezultata, pa se prikaže poruka umjesto vrijednosti iz glavnog upita. |
| Poruka | Poruka za prikaz ako je upit za potvrdu toka podataka vraća bez podataka.  Ako unesete ne prikazuje se poruka, prikazuje se *Izvodi procjenu* . |
| **Vremenski Interval** |
| Trajanje | Trajanje od trenutnog datuma za razdoblje upita.  Ako, na primjer, ako je naveden **7 dana** , zatim upit ograničeno je na zapise stvorene iz 7 dana na trenutni datum. |
| Offset kraj podataka | Neobavezni pomak iz trenutne podatke koje želite koristiti za razdoblje glavni upita.  Ako, na primjer, ako **dan -1** se koristi za **Pomak datuma završetka** i **7 dana** koristi za **trajanje**, zatim upit ograničeno je na zapisi stvoreni od 8 dana da biste jučer. |

## <a name="two-numbers-tile"></a>Pločica dvaju brojeva.

Pločica **Dva broja** prikazuje dvaju brojeva na kojoj je prikazan zbroj zapise iz dvije različite zapisnika upita i natpis za svaku.

![Pločica dvaju brojeva.](media/log-analytics-view-designer/tile-two-numbers.png)

| Postavka | Opis |
|:--|:--|
| Ime        | Tekst koji se prikazuje pri vrhu pločice. |
| Opis | Tekst koji se prikazuje u odjeljku naziv pločice.    |
| **Prvi element** |
| Legende | Tekst koji se prikazuje u odjeljku vrijednost. |
| Upit | Upit za pokretanje.  Prikazat će se broj broj zapisa je vratio upit. |
| **Drugi pločica** |
| Legende | Tekst koji se prikazuje u odjeljku vrijednost. |
| Upit | Upit za pokretanje.  Prikazat će se broj broj zapisa je vratio upit. |
| **Napredno** | **> Provjera toka podataka** |
| Omogućeno | Ako provjera toka podataka mora biti omogućen za pločicu, odaberite.  To omogućuje zamjenske poruku ako podaci nisu dostupni za pločice.  Ovo se obično koristi za pružanje poruke tijekom razdoblja privremene kada je instaliran prikaza i dolaze podaci dostupni. |
| Upit | Upit za pokretanje da biste provjerili je li podaci su dostupni za prikaz.  Ako je upit vraća bez rezultata, pa se prikaže poruka umjesto vrijednosti iz glavnog upita. |
| Poruka | Poruka za prikaz ako je upit za potvrdu toka podataka vraća bez podataka.  Ako unesete ne prikazuje se poruka, prikazuje se *Izvodi procjenu* . |
| **Vremenski Interval** |
| Trajanje | Trajanje od trenutnog datuma za razdoblje upita.  Ako, na primjer, ako je naveden **7 dana** , zatim upit ograničeno je na zapise stvorene iz 7 dana na trenutni datum. |
| Offset kraj podataka | Neobavezni pomak iz trenutne podatke koje želite koristiti za razdoblje glavni upita.  Ako, na primjer, ako **dan -1** se koristi za **Pomak datuma završetka** i **7 dana** koristi za **trajanje**, zatim upit ograničeno je na zapisi stvoreni od 8 dana da biste jučer. |

## <a name="donut-tile"></a>Pločica prstenastih

Pločica **prstenastih** prikazuje samo jedan broj sažeta iz vrijednost stupca u upitu zapisnika.  Na prstenastih grafički prikazuje rezultate gornje tri zapisa.

![Pločica prstenastih](media/log-analytics-view-designer/tile-donut.png)

| Postavka | Opis |
|:--|:--|
| Ime        | Tekst koji se prikazuje pri vrhu pločice. |
| Opis | Tekst koji se prikazuje u odjeljku naziv pločice.    |
| **Prstenastih** |
| Upit | Upit da biste pokrenuli za na prstenastih.  Prvo svojstvo mora biti tekstne vrijednosti i svojstvo drugi brojčana vrijednost.  To je obično upit koji koristi ključnu riječ **mjere** za zbrajanje rezultate. |
| **Prstenastih** | **> Centar za** |
| Tekst | Tekst koji se prikazuje u odjeljku vrijednosti unutar na prstenastih. |
| Postupak | Postupak da biste izvršili u svojstvu vrijednost za zbrajanje jednu vrijednost.<br><br>-Zbroj: Dodajte vrijednosti svi zapisi s vrijednošću svojstvo.<br>– Postotak: Postotak summed vrijednosti iz zapisa pomoću svojstva vrijednost u usporedbi s summed vrijednosti sve zapise. |
| Rezultat vrijednosti koje se koriste u Centar za postupak | Po želji kliknite znak plus da biste dodali jednu ili više vrijednosti.  Rezultati upita bit će ograničeni na zapisa koji sadrže vrijednosti nekretnina koju navedete.  Ako se dodaju nikakve vrijednosti, od svih zapisa nalaze se u upitu. |
| **Prstenastih** | **> Dodatne mogućnosti** |
| Boje | Boja da bi se prikazao za svaku od tri gornji svojstva.  Ako želite navesti zamjensku bojama za vrijednosti nekretnina određene koristiti napredne mapiranja boja. |
| Mapiranje dodatne boje | Prikazuje boju za vrijednosti nekretnina određene.  Ako je vrijednost koju navedete u gornjoj tri, Zamjenska boja se prikazuju umjesto standardna boja.  Ako je svojstvo nije prva tri, zatim željenu boju se ne prikazuje. |
| **Napredno** | **> Provjera toka podataka** |
| Omogućeno | Ako provjera toka podataka mora biti omogućen za pločicu, odaberite.  To omogućuje zamjenske poruku ako podaci nisu dostupni za pločice.  Ovo se obično koristi za pružanje poruke tijekom razdoblja privremene kada je instaliran prikaza i dolaze podaci dostupni. |
| Upit | Upit za pokretanje da biste provjerili je li podaci su dostupni za prikaz.  Ako je upit vraća bez rezultata, pa se prikaže poruka umjesto vrijednosti iz glavnog upita. |
| Poruka | Poruka za prikaz ako je upit za potvrdu toka podataka vraća bez podataka.  Ako unesete ne prikazuje se poruka, prikazuje se *Izvodi procjenu* . |
| **Vremenski Interval** |
| Trajanje | Trajanje od trenutnog datuma za razdoblje upita.  Ako, na primjer, ako je naveden **7 dana** , zatim upit ograničeno je na zapise stvorene iz 7 dana na trenutni datum. |
| Offset kraj podataka | Neobavezni pomak iz trenutne podatke koje želite koristiti za razdoblje glavni upita.  Ako, na primjer, ako **dan -1** se koristi za **Pomak datuma završetka** i **7 dana** koristi za **trajanje**, zatim upit ograničeno je na zapisi stvoreni od 8 dana da biste jučer. |

## <a name="line-chart-tile"></a>Linijski grafikon pločica

Pločica **linijski grafikon** prikazuje linijski grafikon s više nizova iz zapisnika upita tijekom vremena.  

![Linijski grafikon i oblačića pločica](media/log-analytics-view-designer/tile-line-chart.png)

| Postavka | Opis |
|:--|:--|
| Ime        | Tekst koji se prikazuje pri vrhu pločice. |
| Opis | Tekst koji se prikazuje u odjeljku naziv pločice.    |
| **Linijski grafikon** |  
| Upit | Upit da biste pokrenuli za linijski grafikon.  Prvo svojstvo mora biti tekstne vrijednosti i svojstvo drugi brojčana vrijednost.  To je obično upit koji koristi ključnu riječ **mjere** za zbrajanje rezultate.  Ako je upit koristi ključnu riječ **interval** osi na grafikonu će koristiti ovaj vremenski interval.  Ako je upit sadrže ključnu riječ **interval** svaki sat intervalima koriste za os x. |
| **Linijski grafikon** | **> Os Y** |
| Korištenje Logaritamska skala | Odaberite da biste koristili Logaritamsko mjerilo za os y. |
| Jedinice | Navedite jedinicu vrijednosti koji je vratio upit.  Ti podaci se koristi da biste prikazali oznake na grafikonu koji označava vrstu vrijednosti, a po želji za pretvaranje vrijednosti.  **Jedinica vrsta** određuje kategoriju jedinice koji definira **Trenutna jedinica vrsta** vrijednosti koje su dostupne.  Ako odaberete vrijednost u **pretvorili** numeričke vrijednosti se pretvaraju u **Trenutnoj jedinici** vrsti vrstu **Pretvori u** . |
| Prilagođene naljepnice | Tekst koji se prikazuje za os Y uz natpis za vrstu jedinicu.  Ako nema oznake nije naveden, prikazuje se samo vrsta jedinicu. |
| **Napredno** | **> Provjera toka podataka** |
| Omogućeno | Ako provjera toka podataka mora biti omogućen za pločicu, odaberite.  To omogućuje zamjenske poruku ako podaci nisu dostupni za pločice.  Ovo se obično koristi za pružanje poruke tijekom razdoblja privremene kada je instaliran prikaza i dolaze podaci dostupni. |
| Upit | Upit za pokretanje da biste provjerili je li podaci su dostupni za prikaz.  Ako je upit vraća bez rezultata, pa se prikaže poruka umjesto vrijednosti iz glavnog upita. |
| Poruka | Poruka za prikaz ako je upit za potvrdu toka podataka vraća bez podataka.  Ako unesete ne prikazuje se poruka, prikazuje se *Izvodi procjenu* . |
| **Vremenski Interval** |
| Trajanje | Trajanje od trenutnog datuma za razdoblje upita.  Ako, na primjer, ako je naveden **7 dana** , zatim upit ograničeno je na zapise stvorene iz 7 dana na trenutni datum. |
| Offset kraj podataka | Neobavezni pomak iz trenutne podatke koje želite koristiti za razdoblje glavni upita.  Ako, na primjer, ako **dan -1** se koristi za **Pomak datuma završetka** i **7 dana** koristi za **trajanje**, zatim upit ograničeno je na zapisi stvoreni od 8 dana da biste jučer. |


## <a name="line-chart--callout-tile"></a>Linijski grafikon i oblačića pločica

**Linijski grafikon i oblačića** pločica prikazuje linijski grafikon s više nizova iz zapisnika upita s vremenom i Oblačić s sažete vrijednosti.  

![Linijski grafikon i oblačića pločica](media/log-analytics-view-designer/tile-line-chart-callout.png)

| Postavka | Opis |
|:--|:--|
| Ime        | Tekst koji se prikazuje pri vrhu pločice. |
| Opis | Tekst koji se prikazuje u odjeljku naziv pločice.    |
| **Linijski grafikon** |  
| Upit | Upit da biste pokrenuli za linijski grafikon.  Prvo svojstvo mora biti tekstne vrijednosti i svojstvo drugi brojčana vrijednost.  To je obično upit koji koristi ključnu riječ **mjere** za zbrajanje rezultate.  Ako je upit koristi ključnu riječ **interval** osi na grafikonu će koristiti ovaj vremenski interval.  Ako je upit sadrže ključnu riječ **interval** svaki sat intervalima koriste za os x. |
| **Linijski grafikon** | **> Oblačića** |
| Oblačić | Tekst naslova da biste prikazali iznad vrijednosti oblačić. |
| Naziv skupa podataka | Vrijednost svojstva za niz koji želite koristiti za vrijednost oblačić.  Ako nema niz nije naveden, koriste se sve zapise iz upita. |
| Postupak | Postupak da biste izvršili u svojstvu vrijednost za sažimanje jedne vrijednosti za oblačić.<br>-Prosjek: Prosjek vrijednosti iz svih zapisa.<br><br>-Broj: Brojanje svih zapisa je vratio upit.<br>-Zadnje uzorak: Vrijednost iz zadnjeg interval uključen u grafikonu.<br>-Max: Maksimalna vrijednost s vremenskim razmacima uključeni u grafikonu.<br>-Min: Minimalna vrijednost s vremenskim razmacima uključeni u grafikonu.<br>– Zbroj: Zbroj vrijednosti iz svih zapisa. |
| **Linijski grafikon** | **> Os Y** |
| Korištenje Logaritamska skala | Odaberite da biste koristili Logaritamsko mjerilo za os y. |
| Jedinice | Navedite jedinicu vrijednosti koji je vratio upit.  Ti podaci se koristi da biste prikazali oznake na grafikonu koji označava vrstu vrijednosti, a po želji za pretvaranje vrijednosti.  **Jedinica vrsta** određuje kategoriju jedinice koji definira **Trenutna jedinica vrsta** vrijednosti koje su dostupne.  Ako odaberete vrijednost u **pretvorili** numeričke vrijednosti se pretvaraju u **Trenutnoj jedinici** vrsti vrstu **Pretvori u** . |
| Prilagođene naljepnice | Tekst koji se prikazuje za os Y uz natpis za vrstu jedinicu.  Ako nema oznake nije naveden, prikazuje se samo vrsta jedinicu. |
| **Napredno** | **> Provjera toka podataka** |
| Omogućeno | Ako provjera toka podataka mora biti omogućen za pločicu, odaberite.  To omogućuje zamjenske poruku ako podaci nisu dostupni za pločice.  Ovo se obično koristi za pružanje poruke tijekom razdoblja privremene kada je instaliran prikaza i dolaze podaci dostupni. |
| Upit | Upit za pokretanje da biste provjerili je li podaci su dostupni za prikaz.  Ako je upit vraća bez rezultata, pa se prikaže poruka umjesto vrijednosti iz glavnog upita. |
| Poruka | Poruka za prikaz ako je upit za potvrdu toka podataka vraća bez podataka.  Ako unesete ne prikazuje se poruka, prikazuje se *Izvodi procjenu* . |
| **Vremenski Interval** |
| Trajanje | Trajanje od trenutnog datuma za razdoblje upita.  Ako, na primjer, ako je naveden **7 dana** , zatim upit ograničeno je na zapise stvorene iz 7 dana na trenutni datum. |
| Offset kraj podataka | Neobavezni pomak iz trenutne podatke koje želite koristiti za razdoblje glavni upita.  Ako, na primjer, ako **dan -1** se koristi za **Pomak datuma završetka** i **7 dana** koristi za **trajanje**, zatim upit ograničeno je na zapisi stvoreni od 8 dana da biste jučer. |

## <a name="two-timelines-tile"></a>Pločica dvije vremenske crte

Pločica **dvije vremenske crte** prikazuje rezultate dva zapisnika upita tijekom vremena kao stupčastim grafikonima.  Prikazat će se oblačić za svaki niz.  

![Pločica dvije vremenske crte](media/log-analytics-view-designer/tile-two-timelines.png)

| Postavka | Opis |
|:--|:--|
| Ime        | Tekst koji se prikazuje pri vrhu pločice. |
| Opis | Tekst koji se prikazuje u odjeljku naziv pločice.    |
| Prvi grafikon   
| Legende | Tekst koji se prikazuje u odjeljku oblačić za prvi niz.
| Boja | Boju koju želite koristiti za stupce u prvom nizu.
| Upit za grafikon | Upit da biste pokrenuli za prvi niz.  Broj broj zapisa preko svake vremenski interval će predstavljati stupcima grafikona.
| Postupak | Postupak da biste izvršili u svojstvu vrijednost za sažimanje jedne vrijednosti za oblačić.<br><br>-Prosjek: Prosjek vrijednosti iz svih zapisa.<br>-Broj: Brojanje svih zapisa je vratio upit.<br>-Zadnje uzorak: Vrijednost iz zadnjeg interval uključen u grafikonu.<br>-Max: Maksimalna vrijednost s vremenskim razmacima uključeni u grafikonu.
| **Drugi grafikon** |
| Legende | Tekst koji se prikazuje u odjeljku u oblačiću drugog niza.
| Boja | Boju koju želite koristiti za stupce u drugom nizu.
| Upit za grafikon | Upit da biste pokrenuli za drugog niza.  Broj broj zapisa preko svake vremenski interval će predstavljati stupcima grafikona.
| Postupak | Postupak da biste izvršili u svojstvu vrijednost za sažimanje jedne vrijednosti za oblačić.<br><br>-Prosjek: Prosjek vrijednosti iz svih zapisa.<br>-Broj: Brojanje svih zapisa je vratio upit.<br>-Zadnje uzorak: Vrijednost iz zadnjeg interval uključen u grafikonu.<br>-Max: Maksimalna vrijednost s vremenskim razmacima uključeni u grafikonu. |
| **Napredno** | **> Provjera toka podataka** |
| Omogućeno | Ako provjera toka podataka mora biti omogućen za pločicu, odaberite.  To omogućuje zamjenske poruku ako podaci nisu dostupni za pločice.  Ovo se obično koristi za pružanje poruke tijekom razdoblja privremene kada je instaliran prikaza i dolaze podaci dostupni. |
| Upit | Upit za pokretanje da biste provjerili je li podaci su dostupni za prikaz.  Ako je upit vraća bez rezultata, pa se prikaže poruka umjesto vrijednosti iz glavnog upita. |
| Poruka | Poruka za prikaz ako je upit za potvrdu toka podataka vraća bez podataka.  Ako unesete ne prikazuje se poruka, prikazuje se *Izvodi procjenu* . |
| **Vremenski Interval** |
| Trajanje | Trajanje od trenutnog datuma za razdoblje upita.  Ako, na primjer, ako je naveden **7 dana** , zatim upit ograničeno je na zapise stvorene iz 7 dana na trenutni datum. |
| Offset kraj podataka | Neobavezni pomak iz trenutne podatke koje želite koristiti za razdoblje glavni upita.  Ako, na primjer, ako **dan -1** se koristi za **Pomak datuma završetka** i **7 dana** koristi za **trajanje**, zatim upit ograničeno je na zapisi stvoreni od 8 dana da biste jučer. |

## <a name="next-steps"></a>Daljnji koraci

- Informirajte se o [zapisniku pretraživanja](log-analytics-log-searches.md) za podršku upite u pločice.
- Dodavanje [Dijelova vizualizacije](log-analytics-view-designer-parts.md) prilagođenog prikaza.