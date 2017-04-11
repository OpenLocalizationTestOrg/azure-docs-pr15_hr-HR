# <a name="azure-technical-documentation-contributor-guide"></a>Vodič za Azure tehničku dokumentaciju suradnika

Pronađete spremište GitHub koju se pohranjuju izvora za tehničku dokumentaciju objavljene u centru za dokumentaciju Azure pri [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation).

Ovo spremište sadrži i upute da biste lakše zapisuju u našem tehničku dokumentaciju.  Popis članaka u vodiču za suradnika, potražite u članku [indeksa](https://github.com/Azure/azure-content/blob/master/contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Sudjelovanje u dokumentaciji Azure

Zahvaljujemo na iskazanom interesu u dokumentaciji za Azure.

* [Načini Suradnja](#ways-to-contribute)
* [Ponašanja](#code-of-conduct)
* [O doprinos Azure sadržaja](#about-your-contributions-to-azure-content)
* [Spremište tvrtke ili ustanove](#repository-organization)
* [Korištenje GitHub, brojka i ovo spremište](#use-github-git-and-this-repository)
* [Kako koristiti markdown da biste oblikovali teme](#how-to-use-markdown-to-format-your-topic)
* [Povratne informacije, komentare i podršku](./contributor-guide/feedback-and-comments.md)
* [Dodatni resursi](#more-resources)
* [Indeks članke vodič za sve suradnika](./contributor-guide/contributor-guide-index.md) (koji se otvara nova stranica)

## <a name="ways-to-contribute"></a>Načini Suradnja 

Možete priložiti dokumentaciju [Azure](http://azure.microsoft.com/documentation/) u na nekoliko različitih načina:

* Suradnja na [forumu rasprave](http://social.msdn.microsoft.com/Forums/windowsazure/home).
* Unos komentara Disqus pri dnu članke.
* Možete jednostavno pridonijeti tehničkih članaka u korisničkom sučelju GitHub. Pronađite članak u ovom spremištu ili potražite u članku na [http://azure.microsoft.com/documentation](http://azure.microsoft.com/documentation) i kliknite vezu u članak koji vodi do GitHub izvora članka.
* Ako mijenjate znatno postojeći članak, dodavanje ili promjena slike ili Gostujući novi članak ćete morati fork ovo spremište, instalirajte brojka tulumu, tipkovnica Markdown i Saznajte neke naredbe brojka.

##<a name="code-of-conduct"></a>Ponašanja

Taj projekt ima prihvatile [Microsoft Otvori izvor ponašanja](https://opensource.microsoft.com/codeofconduct/). Dodatne informacije potražite [Kod od njih najčešća pitanja vezana uz](https://opensource.microsoft.com/codeofconduct/faq/) ili kontakt [opencode@microsoft.com](mailto:opencode@microsoft.com) s bilo kojeg dodatna pitanja i komentare.

##<a name="about-your-contributions-to-azure-content"></a>O doprinos Azure sadržaja

###<a name="minor-corrections"></a>Manji ispravaka

Manji ispravke ili clarifications pošaljete za dokumentaciju i kod primjerima u ovom repo prekrivene [Azure web-mjesto uvjete korištenja (uvjeti upotrebe)](http://azure.microsoft.com/support/legal/website-terms-of-use/).


###<a name="larger-submissions"></a>Veće poslanih stavki

Ako pošaljete zahtjev za istaknuti promjenama novi ili značajan dokumentaciju i primjere koda, ćemo poslati komentar u GitHub s pitanjem želite li poslati na online doprinos licencni ugovor (CLA) ako ste u neku od tih grupa:

* Članovi grupe Microsoftove tehnologije otvorena.
* Suradnicima koji ne funkcioniraju za Microsoft.

Ne možemo morate proći mrežnom obrascu smo možete prihvatiti zahtjev za istaknuti.

Sve pojedinosti dostupne su na [http://azure.github.io/guidelines/#cla](http://azure.github.io/guidelines/#cla).

## <a name="repository-organization"></a>Spremište tvrtke ili ustanove

Sadržaj u spremištu azure sadržaj slijedi tvrtke ili ustanove dokumentacije na [Azure.Microsoft.com](http://azure.microsoft.com). Ovo spremište sadrži dvije korijenske mape:

### <a name="articles"></a>\articles

Mapa *\articles* sadrži članke dokumentaciju oblikovan kao markdown datoteke s nastavkom *.md* .

U korijenskom direktoriju su objavljene na Azure.Microsoft.com na putu *http://azure.microsoft.com/documentation/articles/ {članak-naziv-bez-md} /*.

* **Članak nazivi datoteka:** Pogledajte [naš imenovanja smjernice datoteka](./contributor-guide/file-names-and-locations.md).

Članci vlastite mape servisa su objavljene na Azure.Microsoft.com na putu *http://azure.microsoft.com/documentation/articles/service-folder/ {članak-naziv-bez-md} /*

* **Media podmape:** *\Articles* mapa sadrži mapu *\media* korijenski direktorij članak medijske datoteke unutar koje su podmape sa slikama svakog članka.  Mapa servisa sadrži mapu zasebnom medijskih sadržaja za članaka u svakoj mapi servisa. Slika mape članak imenovane su jednak način u članku datoteku minus *.md* datotečni nastavak.

### <a name="includes"></a>\includes

Možete stvoriti ponovno iskoristivog sadržaja sekcije želite uvrstiti u jednu ili više članaka. Prikaz [Prilagođeno dodataka koji se koriste u našem Tehnički sadržaj](./contributor-guide/custom-markdown-extensions.md).

### <a name="markdown-templates"></a>Predlošci \markdown

Ova mapa sadrži naš standardni markdown predložak s oblikovanjem osnovni markdown trebate članka.

### <a name="contributor-guide"></a>\contributor-Guide

Ova mapa sadrži članaka koji su dio vodiča za naše suradnika.  

## <a name="use-github-git-and-this-repository"></a>Korištenje GitHub, brojka i ovo spremište

Informacije o kako priložiti, kako koristiti korisničko Sučelje GitHub da biste slali male promjene i o ogranku i Kloniraj spremište za više značajan doprinos potražite u članku [Instalacija i postavljanje alata za izradu u GitHub](./contributor-guide/tools-and-setup.md).

Ako instalirate GitBash i odabrati rad lokalno, korake za stvaranje nove lokalne rad granu, promjena i slanje opet glavni granu su navedene u [brojka naredbe za novi članak stvaranje ili ažuriranje postojeći članak](./contributor-guide/git-commands-for-master.md)

### <a name="branches"></a>Grana

Preporučujemo vam da stvorite lokalnu grana rad koji opseg određene promjene. Granu mora biti ograničeni na jednom pojam/članak i da biste pojednostavljivanje tijek rada i smanjili mogućnost Spoji sukobe.  Sljedeće trudu su odgovarajuće opseg za nove podružnice:

* Novi članak (i povezane slike)
* Provjera pravopisa i gramatike uređivanja članka.
* Primjena jedne promjena oblikovanja u skupu rezultata velike članaka (npr. novi autorskih prava podnožje).

## <a name="how-to-use-markdown-to-format-your-topic"></a>Kako koristiti markdown da biste oblikovali teme

U člancima u ovom spremištu pomoću GitHub flavored markdown.  Slijedi popis resursa.

- [Osnove markdown](https://help.github.com/articles/markdown-basics/)

- [Ispis markdown cheatsheet](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

- Naš popis markdown uređivača potražite [alata i postavljanje temu](./contributor-guide/tools-and-setup.md#install-a-markdown-editor).

## <a name="article-metadata"></a>Članak metapodataka

Članak metapodataka omogućuje određene radovi azure.microsoft.com web-mjesta, kao što su attribution autor, attribution suradnika, elemenata, članak opise i SEO optimizacije, kao i izvješćivanja Microsoft koristi su za procjenu performanse sadržaja. Tako, metapodatke važno je! [Ovdje navedene upute osiguravanja metapodatka obavlja desno](./contributor-guide/article-metadata.md).

### <a name="labels"></a>Natpisi

Automatiziranog natpise dodjeljuju se da biste izvukli zahtjeve Pridonesite upravljanje istaknuti zahtjev tijeka rada te pomaže znali što se događa sa zahtjevom za istaknuti:

* Doprinos licencni ugovor odnose
    * cla ne – obavezna: promjena je relativno pomoćna i ne zahtijeva da se prijavite na CLA.
    * cla obavezno: opseg promjena je razmjerno velike i zahtijeva da se prijavite na CLA.
    * cla prijavljeni: U suradničke prijavljeni CLA, tako da povlačite zahtjev možete odmah kretanje naprijed na pregled.
* Pillar natpise: naljepnica kao što su PnP, Moderna aplikacije i TDC pomoći kategorizirati zahtjeva za istaknuti tvrtka Interna koju treba pregledati istaknuti u zahtjev za potvrdu.
* Promjena koje su poslane na Autor: autor sadrži primili obavijest na čekanju istaknuti zahtjeva.

## <a name="more-resources"></a>Dodatni resursi

[Indeks vodiča za naše suradnik](./contributor-guide/contributor-guide-index.md) za naše smjernice temama potražite u članku.
