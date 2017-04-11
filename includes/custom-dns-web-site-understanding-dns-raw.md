Sustav naziva domena (DNS) koristi se za pronalaženje resursa na Internetu. Ako, na primjer, kada unesite web-adresu aplikacije u pregledniku ili kliknite vezu na web-stranici, koristi DNS za domenu Prevedite IP adresa. IP adresa je sort programa kao što su poštansku adresu, ali nije vrlo Ljudski prikladnu. Na primjer, je mnogo lakše zapamtiti DNS naziv kao što je **contoso.com** nego što je zapamtiti IP adresu kao što su 192.168.1.88 ili 2001:0:4137:1f67:24a2:3888:9cce:fea3.

U DNS sustavu temelji se na *zapise*. Zapisi pridružiti određene *naziv*, primjerice **contoso.com**IP adresa ili neki drugi naziv DNS-a. Kada aplikacija, kao što je web-preglednik, pretražuje ime i prezime u DNS-a, pronalazi zapis i koristi neku drugu pokazuje na kao adresu. Ako je vrijednost upućuje na IP adresu, web-pregledniku će koristiti tu vrijednost. Ako se pokazuje na neki drugi naziv DNS-a, zatim aplikacija ima da biste ponovno učinili razlučivost. Konačni sve razlučivanje naziva će se zaustaviti u IP adresa.

Kada stvorite web-aplikaciju programa u aplikaciji servisa, DNS naziva se automatski dodjeljuju web-aplikaciji. Naziv se sastoji od ** &lt;yourwebappname&gt;. azurewebsites.net**. Postoji i virtualna IP adresa dostupan za korištenje prilikom stvaranja DNS zapise tako da možete stvoriti zapise koji vode na **. azurewebsites.net**, ili možete prijeći se s IP adresom.

> [AZURE.NOTE] Ako izbrišete i ponovno stvorite web-aplikaciju programa ili promijenite način aplikacije servisa za planiranje **slobodan** nakon je postavljena **Osnovni**, **zajedničko korištenje**ili **standardni**promijenit će IP adresa web-aplikacije.

Postoje i više vrsta zapisa, svaka s vlastite funkcije i ograničenja, ali web-aplikacijama smo samo brigu o dva, *A* i *CNAME* zapisa.

###<a name="address-record-a-record"></a>Adresni zapis (zapis)

A zapis karata domenu, kao što je **contoso.com** ili **www.contoso.com**, *ili domene zamjenskih znakova* kao što su ** \*. contoso.com**, s IP adresom. U slučaju web app u aplikaciji servisa, virtualne IP servis ili iz određene IP adresa koje ste kupili za web-aplikacije.

Pogodnosti A zapis putem CNAME zapis koji su:

* Mapiranje korijensku domenu, kao što je **contoso.com** IP adresa; Mnoge registrara dopustiti samo ovaj pomoću na zapise

* Imate jedan unos koji koristi zamjenski znak, kao što su ** \*. contoso.com**, koje želite rukovati zahtjeva za više podmjesta domene kao što su **mail.contoso.com**, **blogs.contoso.com**ili **www.contso.com**.

> [AZURE.NOTE] Budući da je A zapis mapirana statičke IP adrese, ne može automatski riješiti promjene IP adresa web-aplikacije. IP adresa za uporabu s zapisa se nudi prilikom konfiguriranja postavki naziv prilagođene domene za web-aplikacije; No tu vrijednost može promijeniti ako izbrišete i ponovno stvorite web-aplikaciju programa ili promijenite način aplikacije servisa za planiranje u pozadinu **slobodan**.

###<a name="alias-record-cname-record"></a>Zapis pseudonima (CNAME zapis)

CNAME zapis mapira *DNS naziv, kao što su **mail.contoso.com** ili **www.contoso.com**,* naziv druge domene (Kanonski). U slučaju aplikacije servisa web-aplikacijama je naziv Kanonski domene na ** &lt;yourwebappname >. azurewebsites.net** naziv domene web-aplikacije. Nakon stvaranja, stvara se CNAME pseudonima za u ** &lt;yourwebappname >. azurewebsites.net** naziv domene. Unos CNAME će riješiti IP adresu na ** &lt;yourwebappname >. azurewebsites.net** naziva domene automatski u pa ako se promijeni IP adresa web-aplikacije, nije potrebno ništa poduzimati.

> [AZURE.NOTE] Neke registrara domena samo omogućuju da biste mapirali poddomene prilikom korištenja CNAME zapis, kao što je **www.contoso.com**, a ne korijenski naziva, primjerice **contoso.com**. Dodatne informacije o CNAME zapisa potražite u dokumentaciji registrar, <a href="http://en.wikipedia.org/wiki/CNAME_record">stavka Wikipedije CNAME zapis</a>ili <a href="http://tools.ietf.org/html/rfc1035">IETF domenama – implementaciji i specifikacija</a> dokumenta.

###<a name="web-app-dns-specifics"></a>Specifičnosti DNS-a za web app

A zapis pomoću web-aplikacije potrebno je najprije stvoriti jednu od sljedećih TXT zapisi:

* **Za korijensku domenu** - A DNS TXT zapis **@** da biste ** &lt;yourwebappname&gt;. azurewebsites.net**.

* **Za određene poddomenu** - A DNS naziva ** &lt;poddomenu >** za ** &lt;yourwebappname&gt;. azurewebsites.net**. Na primjer, **blogove** ako je A zapis za **blogs.contoso.com**.

* **Za zamjenskih sub-dodmains** - A DNS TXT zapis *** za ** &lt;yourwebappname&gt;. azurewebsites.net**.

Da biste potvrdili da ste vlasnik domene koju pokušavate koristiti se koristi TXT zapisa. Ovo je osim stvaranja A zapisa koja pokazuje na virtualna IP adresa web-aplikacije.

Možete pronaći IP adresa i **. azurewebsites.net** imena za web-aplikacije pomoću sljedećih koraka:

1. U pregledniku otvorite [Azure Portal](https://portal.azure.com).

2. U plohu **Web-aplikacije** kliknite naziv web-aplikacije, a zatim odaberite **prilagođenih domena** na dno stranice.

    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)

3. U plohu **prilagođenih domena** vidjet ćete virtualna IP adresa. Spremanje ove informacije kako će se koristiti za stvaranje DNS zapisa

    ![](./media/custom-dns-web-site/virtual-ip-address.png)

    > [AZURE.NOTE] Ne možete koristiti prilagođenih naziva domena s web-aplikacijama **slobodno** , a morate nadograditi tarifu aplikacije servisa za **zajedničko korištenje**, **Osnovni**, **standardne**ili **Premium** sloju. Dodatne informacije o planu aplikacije servisa za cijene razine, uključujući kako promijeniti cijene sloju web-aplikacije, pročitajte članak [upute za promjenu veličine web-aplikacije](../articles/web-sites-scale.md).
