# <a name="quality-criteria-for-pull-request-review"></a>Kvaliteta kriterija za istaknuti zahtjev za pregled

Ove kriterije namijenjeni za autora koji je stvoriti i održavati tehničkih članaka i povlačite zahtjev pregledavatelji koji pregledavaju sadržaja istaknuti zahtjeva. Ako je zahtjev za istaknuti ispunjavate uvjete za [Automatsko spajanje](contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically), će se pregledavati pregledavatelja zahtjev Ljudski povlačite. Istaknuti zahtjev pregledavatelji obično pregledajte samo što je nove ili promijenjene. Istaknuti zahtjev pregledavatelji računaju se promjene u zahtjevu za povlačite prema blokiranja i ne blokira kvalitete pregled stavke navedene u ovom članku.

## <a name="blocking-content-quality-items"></a>Blokiranje stavke kvaliteti sadržaja

Ažuriranja u zahtjevu za povlačite moraju biti usklađene sa sljedećim kriterijima spojiti. Istaknuti pregledavateljima za zahtjev za slanje povratnih informacija u istaknuti zahtjev za komentare za te stavke i vrstu `#hold-off` u zahtjevu za povlačite da biste se vratili na vas (autor) s povratne informacije.

| Kategorija | Stavka za pregled kvalitete |
|----------|---------------------|
|Preduvjeti| "Jeste li spremni-na-spajanja" i oznake "uspjela provjera valjanosti" dodjeljuju se na PR.|
|Preduvjeti| Zahtjev za istaknuti ne blokira sukob spajanja.|
|Integritet repo|    Istaknuti zahtjev sadrži bez očite sadržaja nedostatke.|
|Integritet repo|    Istaknuti zahtjev ne sadrži ugrađeni repo ili nijednu neobično, strani.|
|Integritet repo |Zahtjev za istaknuti sadrži manje od 100 promijenjene datoteke osim ako se Cijena namjerno ažurira granu izdanje iz matrice. (Zaista, mora sadržavati Daleki manje od onog koji je Cijena, ali ako nakon 100 datoteka promijenjene GitHub ne prikaže na diffs).|
|Dodjela naziva |Nazivi datoteka za nove datoteke, slijedite [smjernice za imenovanja datoteka](file-names-and-locations.md).|
|Dodjela naziva |Nove mape uvedena u prati repo [imenovanja smjernice za mapu](file-names-and-locations.md#folder-names-in-the-repo).|
|Sadržaj    |U članku se Tehnički dokument i stoga u odgovarajuće kanala sadržaja. Pogledajte na [što Kamo idu smjernice](content-channel-guidance.md).|
|Sadržaj    |Predmet u Tehnički dokument je prikladno za tehničke članka. Pogledajte na [što Kamo idu smjernice](content-channel-guidance.md).|
|Sadržaj    |Članak sadrži uvodni odlomka i proceduralne ili konceptualni tijelo sadržaja. U članku mora sadržavati potrebne ili potpuno sadržaj stajati vlastitu kao članka. Ne smije biti mali dio informacije. (Iznimke: temu "Ograničenja" Ako je u kontekstu velike članka svih ograničenja usluga.)|
|Sadržaj| Elemente koji se mora biti numerirane popise numeriranja, elemente koji se moraju unordered popise s grafičkim oznakama. To je osnovni upotrebljivosti.|
|Sadržaj| Nepoznatu ili novel grafika, informacijska arhitektura ili strukture ili pogrešno nestandardne dizajna moraju biti vetted s pregledavatelja Cijena potencijalnog klijenta. Ekipa koje su eksperimentiranja s nove stvari moraju imati problema i rješenja crtanja ili plan ovlasti za procjenu eksperimenata.|
|Funkcija/dizajna web-mjesta| Switchers koriste se samo za prijelaz preko više verzija iste članka.|
|Funkcija/dizajna web-mjesta| Naslovi switchered članci sadrže podatke koje svakog članka iz druge članke u skupu switchered.|
|Funkcija/dizajna web-mjesta| Ručno autor kazala nije dopušteno. U članku morate koristiti H2s za njegova sadržaja na stranici.|
|Funkcija/dizajna web-mjesta| Ako postoje zaglavlja H2, u članku sadrži najmanje dva H2 naslova. Korištenje jedan naslov H2 stvara članka na jednom stavkom tablicu sadržaja. Naslovi H2 mora koristiti prije H3 naslovi da biste bili sigurni da se stvara tablicu sadržaja.|
|Markdown| HTML: Izvora sadržaja sadrži HTML na razini bloka – manji umetnute HTML je dopušteno – primjerice eksponent, indeks, posebnih znakova i ostale pomoćne stvari koje ne možete raditi s markdown. HTML tablice dopuštene samo ako tablica sadrži popisa s grafičkim oznakama ili numerirani popis, ali to je obično znak sadržaj treba pojednostavnjeni tako izvorišnog web-mjesta mogu biti zadan u markdown.|
|Markdown   |Prilagođeni markdown elemente koji se koristi primjereno. Ex: Bilješke su programiranja pomoću na AZURE. Imajte na UMU nastavak, nije u obliku običnog teksta.|
|SEO    |Na "& #124; Potreban je Microsoft Azure"identifikator za web-mjesta|
|SEO    |Naslov H1 sadrži potrebne informacije opisuje sadržaj članka, da biste ga razlikovati od druge Azure članke i mapiranje vjerojatno kupca ključne riječi. Na primjer "Pregled" kao naslov H1 je na nije uspjelo.|
|Terminologija| Korištenje OKVIRA kraticu, V1 ili V2 kao reference na klasični i resursima implementacije modelima je blokiranja terminologija vezana uz stavku.|
|Slika |Animirani GIF ne prihvaćaju u na repo.|
|Slika | Očisti razlučivost slike, besplatna pogrešno napisane riječi i sadrže bez osobni podaci | 
|Pripremna|Pretpregled članak mora biti čistu na pripremna. Ne smije sadržavati probleme očite oblikovanja: <br><br>-Grafičkim ili brojčanim oznakama popisa koji će se prikazati kao odlomka <br> -Kod u kod blok pojavljuju se djelomično u blok koda i djelomično izvan nje <br>-Numeriranih koraka broj netočan zbog neispravan uvlaka|

## <a name="non-blocking-content-quality-items"></a>Osobe koje nisu blokiranje sadržaja kvalitete stavki

Stavke, povlačite zahtjev pregledavatelji pružaju povratne informacije i upute za daljnji rad s rješenja u zahtjev za kasnije istaknuti autora. Međutim, ovaj povratne informacije blokirati odluku da želite spojiti. Autori treba daljnji rad unutar 3 dana tvrtke s rješenja.

| Kategorija | Stavka za pregled kvalitete |
|----------|---------------------|
|Sadržaj|Članci moraju imati "sljedeći koraci" na kraju s 1-3 odgovarajuću i atraktivnog sljedeće korake. Kratki tekst mora biti uvršteni koji omogućuje kupac razumijevanje Zašto se odnose na sljedeće korake. (Novih članaka samo). Primjer: <https://azure.microsoft.com/en-us/documentation/articles/xplat-cli-install/><br>![](media/contributor-guide-pr-criteria/nextstepsexample.PNG)|
|Sadržaj|Pravopis, gramatika i ostalih problema pisanje - istaknuti zahtjev pregledavatelji možda pošaljite povratne informacije o nekoliko manje probleme kao koje nisu-blokiranje povratne informacije. Ako postoje problemi s više od nekoliko Lektorsko, pregledavatelji prijavite zahtjeva za uređivanje članka za publikacije nakon uređivanja.|
|Slika|Slike pomoću odgovarajuće oblačić stil i boju, a snimke zaslona pomoću odgovarajuće stil obruba i rezervirano mjesto. [Potražite u članku smjernice za slike](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Slika|Slika sadržavati zamjenski tekst. [Potražite u članku smjernice za slike](https://github.com/Azure/azure-content/blob/master/contributor-guide/create-images-markdown.md).|
|Funkcija/dizajna web-mjesta|Naslovi H2, pri prikazivanju u tablicu sadržaja na stranici treba najbolje prelama tako da više od 2 retka. Dulji naslove provjerite u članku TOC teže za pregled.|
|Stil konvencije|Svi naslovi i zaglavlja su prvo slovo rečenice veliko, po Azure stil.|
|Postupak|Ako zahtjev za istaknuti nije ste jednostavno konfigurirana da biste im PRmerger Automatizacija, povlačite pregledavateljima za zahtjev za slanje povratnih informacija autora o tome kako koristiti grana tako da se ne može automatski spojiti promjene. Potražite [u članku bontonu Cijena](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-pull-request-etiquette.md#in-a-hurry-submit-prs-that-can-be-accepted-automatically).|
|Postupak|Kad brisanje ili preimenovanje članka, provjerite je li slijedite postupak. Povlačenje zahtjev pregledavatelji treba dodati sljedeći komentar i veza u komentaru:<br><br>*Provjerite je li iza postupka u vodiču za suradnika za brisanje članci: <https://github.com/Azure/azure-content/blob/master/contributor-guide/retire-or-rename-an-article.md> .*|

## <a name="related"></a>Odnosi

- [Povlačenje bontonu zahtjev i najbolje prakse za Microsoft suradnika](contributor-guide-pull-request-etiquette.md)

- [Povlačenje Automatizacija zahtjev komentara](contributor-guide-pull-request-comments.md)
