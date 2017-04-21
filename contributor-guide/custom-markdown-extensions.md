<properties
    title="required"
    pageTitle="Prilagođeni markdown proširenja koristi u našem tehničkih članaka"
    description="Popis prilagođene markdown proširenja koja omogućuju umetnute videozapise, bilješke i savjete, ponovno iskoristivog sadržaja i druge stavke u azure.microsoft.com tehničkih članaka."
    services=""
    solutions=""
    documentationCenter=""
    authors="tysonn"
    manager="carolz"
    editor=""/>

<tags
    ms.service="contributor-guide"
    ms.devlang=""
    ms.topic="article"
    ms.tgt_pltfrm=""
    ms.workload=""
    ms.date="01/22/2015"
    ms.author="tysonn"/>

## <a name="markdown-for-azuremicrosoftcom"></a>Markdown za Azure.microsoft.com

Općenito markdown savjete potražite u članku [Osnove Markdown](https://help.github.com/articles/markdown-basics/) i naše [markdown cheatsheet](./media/documents/markdown-cheatsheet.pdf?raw=true). Ako vam je potrebna da biste stvorili članak crosslinks u markdown, potražite u članku [povezivanja smjernice] (. / create-links-markdown.md#markdown-syntax-for-acom-relative-links.md/).

Azure.microsoft.com podržava [fenced blokova Šifra](https://help.github.com/articles/github-flavored-markdown/#fenced-code-blocks) i [Isticanje sintaksu](https://help.github.com/articles/github-flavored-markdown/#syntax-highlighting). Međutim, ACOM podržava samo jedan sintaksa isticanje shemu boja, bez obzira na jeziku koji ste naveli u blok koda.

## <a name="custom-markdown-extensions-used-in-our-technical-articles"></a>Prilagođeni markdown proširenja koristi u našem tehničkih članaka

Naš članke korištenje GitHub flavored markdown za većinu oblikovanja članak - odlomke, veze, popisa, naslove, itd. No koristimo prilagođene markdown proširenja moramo bogatiji oblikovanja u prikazanih stranica na azure.microsoft.com. Evo proširenja smo trenutno koristite:

+ [Bilješke i savjeti]
+ [Obuhvaća]
+ [Umetanje videozapisa]
+ [Birači tehnologija i platforme]

## <a name="notes-and-tips"></a>Bilješke i savjeti

Možete odabrati neku od 4 vrsta bilješke i savjete:

- AZURE. NAPOMENA
- AZURE. UPOZORENJE
- AZURE. TIPss
- AZURE. VAŽNO

###<a name="usage"></a>Korištenje
Općenito govoreći, koristite bilješke i savjete pažljivo cijeloj članci. Kada koristite ih, odaberite odgovarajuću vrstu bilješke ili savjeta:

- Koristite AZURE. Imajte na UMU da biste istaknuli neutralni pozitivan podatke i ističe ili nadopunjuju ključne točke glavni tekst. Bilješke pruža informacije koje se odnose samo u određenim slučajevima.

  ![](./media/custom-markdown-extensions/Notes-note.PNG)

- Koristite AZURE. Upozorenje upozorenje korisniku uvjet koji mogu uzrokovati problem u budućnosti. Na primjer, odabir određene mogućnosti ili uspostavljanja određene mogućnosti možda trajno zaključati vam u određenom scenarij.

  ![](./media/custom-markdown-extensions/Notes-warning.PNG)

- Koristite AZURE. Savjet da bi korisnici Primjena tehnike i postupke opisane u okvir tekst za svojim potrebama. Savjet također predložiti druge metode koje možda neće biti očite. Savjeti, no nisu ključni za osnovni razumijevanje teksta.

  ![](./media/custom-markdown-extensions/Notes-tip.PNG)

- Koristite AZURE. Važno je da biste dobili informacije koje su presudne za dovršetak zadatka.

  ![](./media/custom-markdown-extensions/Notes-important.PNG)

Dok te bilješke i savjete podržava blokova Šifra, slike, popise i veze, pokušajte da biste zadržali bilješke i savjete jednostavne i jasan. Ako pronađete samostalno stvaranje složenih bilješke s mnogo oblikovanja, koji bi mogli biti znak morate samo druga sekcija u glavni tekst članka. Možete i previše bilježaka u članak može biti detalj koji vam i teško skeniranje ili čitanje.

###<a name="sample-markdown"></a>Ogledni markdown

Svi uzorci prikaz programa AZURE. IMAJTE NA UMU. Da biste koristili savjet, upozorenje ili važno, zamijenite "NAPOMENE" u na markdown:

    > [AZURE.TIP]

    > [AZURE.WARNING]

    > [AZURE.IMPORTANT]

Jedan odlomak:

    > [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun sustava Microsoft Azure active. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta.

Multiparagraph:

    > [AZURE.NOTE] Da biste dovršili ovaj Praktični vodič, morate imati račun sustava Microsoft Azure active.
    >
    > Ako nemate račun, možete [stvoriti besplatne probne račun](http://www.windowsazure.com/pricing/free-trial/) u samo nekoliko minuta.

## <a name="includes"></a>Obuhvaća

Ponovno iskoristiv tekst u našem spremištu GitHub nalazi se u datoteke koje ćemo poziva "sadrži". Kada odaberete tekst koji treba koristiti u više člancima, uključiti referencu na toj datoteci ponovno iskoristiv podataka. Uvrštavanje sam je datoteka jednostavne markdown (.md). Može sadržavati bilo koji valjani markdown, uključujući tekst, veze i slike. Sve obuhvaćaju markdown datoteke moraju biti [na / obuhvaća direktorija](https://github.com/Azure/azure-content/tree/master/includes) u korijenskoj mapi spremište. Kada je objavljen u članku, uvrštavanje teksta jednostavno je integriran u objavljeni temu.

- Reference na Uključi koristimo određenu sintaksu.

- Medijskih datoteka u se Uključi stavite moraju se stvoriti u mapi media odnose na uključeno. Sadrži medijske sadržaje mape za nalaze u [mapi azure sadržaj/obuhvaća/medijskog sadržaja](https://github.com/Azure/azure-content/tree/master/includes/media). Direktorija medijskog sadržaja mora sadržavati sve slike u svom korijenu. Ako okvir Dodaj slike, zatim odgovarajući direktorij medijskih sadržaja nije potrebna.

###<a name="usage"></a>Korištenje

- Korištenje obuhvaća kad god je potrebno da se pojavi u više člancima isti tekst.

- Obuhvaća namijenjeni su će se koristiti za značajan količine sadržaja - odlomka ili dvije, zajedničke postupak ili zajedničke sekcije. Ih ne možete koristiti za sve što manje od rečenice; **su ne za nazive proizvoda**.

- Provjerite je li sve tekst u programa Uključi napisan dovršeno rečenice ili izraza koji ne ovise na prethodni ili sljedeći teksta u članku koja se odnosi na Uključi. Zanemarivanje ovaj vodič stvara untranslatable niz članka prijelomi lokalizirane iskustvo. 

- Ugrađivanje ne obuhvaća unutar druge obuhvaća. Nisu podržani po DPS sustava za objavljivanje.

- Ne objavljuj medijskih sadržaja između datoteka. Jedinstveni naziv za svaki Uključi i članak koristiti u zasebnu datoteku. Pohranite medijske datoteke u mapi medijskih sadržaja pridružena uključeno.

- Nemojte koristiti za uvrštavanje kao samo sadržaj članka.  Obuhvaća namijenjeni su dodatni sadržaj u ostatku članka.

- Budući da se sve obuhvaća mora biti u na / obuhvaća direktorija, uvijek je put do programa Uključi iz članka

    .. / obuhvaća

- Ponovite filename referenca vezu ili sliku u članak i okvir Dodaj. Dodavanje "-uključuju" da biste reference ili medij filename vezu da biste izbjegli ponavljajuće referenca:

 **Veza pozivanja**

 Promjena: odata.org za: odata.org uvrstiti

 **Referenca za slike**

 Promjena: table.png da biste: tablice include.png

###<a name="sample-markdown"></a>Ogledni markdown
Vidjet ćete da sintaksa za dodavanje se Uključi u dokumentaciji članak je:

    [AZURE.INCLUDE [include-short-name](../includes/include-file-name.md)]

Primjer

    [AZURE.INCLUDE [howto-blob-storage](../includes/howto-blob-storage.md)]

Prvi dio uključeno je naziv Uključi bez put i bez nastavka .md. Drugi dio je relativni put do Uvrsti u na / obuhvaća direktorija, s datotečnim nastavkom .md.

###<a name="rendering"></a>Vizualizacije

Na stranici prikazanih GitHub uključeno će prikazati na sljedeći način:

 [AZURE. UKLJUČITI Nemoderirana-blobova]

U HTML prikazanih na azure.microsoft.com, HTML-a iz na obuhvaća spajaju se u ostatku dokumenta HTML. No HTML će sadržavati HTML komentar s izvornim obuhvaćaju markdown filename i potvrdi raspršivanje GitHub. Komentar uključen za rješavanje problema tako da izvora sadržaja mogu jednostavno prepoznati i pronaći u GitHub:

  ![](./media/custom-markdown-extensions/include.png)


## <a name="embedded-videos"></a>Umetanje videozapisa

Naš tehničkih članaka podržava embeddeded videozapise u tehničkih članaka pod uvjetom da su videozapisa na web-mjestu za [kanal 9](http://channel9.msdn.com/) tvrtke Microsoft. Videozapisi iz kanala 9 morate integrirani [azure.microsoft.com centar za videozapis](http://azure.microsoft.com/documentation/videos/home/). Trenutno ne podržavamo umetnute videozapise servisa YouTube; Ako ste suradnik zajednice, koje su Dobro došli u veza na Youtubeu ako postoji poslana videozapis koji želite istaknuti. Microsoft suradnici trebali biste koristiti centar za videozapisa i kanala 9.

### <a name="usage"></a>Korištenje

- Provjerite je li videozapis u centru za videozapis.

- Kopirajte videozapis ID neslužbeni URL videozapisa na kanalu 9 ili iz centra za videozapis Azure. Ako, na primjer, videozapisa ID videozapisa na [http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/](http://azure.microsoft.com/documentation/videos/azure-scheduler-unusual-schedules/) je **azure raspored-neobično-planira**.

### <a name="syntax"></a>Sintaksa

    > [AZURE.VIDEO video-id-string]

### <a name="rendering"></a>Vizualizacije

Na GitHub: [https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md](https://github.com/Azure/azure-content-pr/blob/master/articles/web-sites-backup.md)

Objavljeni članak: [http://azure.microsoft.com/documentation/articles/web-sites-backup/](http://azure.microsoft.com/documentation/articles/web-sites-backup/)


## <a name="technology-and-platform-selectors"></a>Birači tehnologija i platforme

Pomoću tehnologije i platformu switchers u tehničkih članaka kada vi ste autor više flavors istom članku na adresi razlike u implementaciji preko tehnologije ili platformama. To je obično najčešće primjenjuju na sadržaj mobilne platformu za razvojne inženjere. Trenutno postoje dvije različite vrste Birači, [jednostavno Birači](#simple-selectors) i [dvosmjerni Birači](#two-way-selectors).

Budući da isti markdown birač dolazi u svaku temu u odabir, preporučujemo da stavljanje birač za teme u na Uključi, a zatim upućuju na tom Uključi u svim temama koje koriste isti birač.

###<a id="simple-selectors"></a>Jednostavan Birači

Jednostavan (jednosmjerna) Birači prikazuju se kao skup gumba mogućnosti odmah ispod naslova. Pomoću ovih gumba kada korisnici morate birati teme u jednu platformu ili tehnologija skup, primjerice .NET, Node.js i Java.  Korištenje prilagođenih markdown proširenje za sve Birači – pomoću HTML za Birači.  

Pročitajte članak [Početak rada s koncentratorima obavijesti](http://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started/) da biste vidjeli kako Autor stvara 8 verzijama istom članku, ali korištenih Birači omogućiti navigaciju preko njih sve.

![Primjer jednostavne birač](./media/custom-markdown-extensions/selectors.PNG)

####<a name="syntax"></a>Sintaksa

    > [AZURE.SELECTOR]
    - [Povezivanje natpisa #1](link #1 url)
    - [oznaka veze #2](link #2 url)

Primjer:

    > [AZURE.SELECTOR]
    - [Univerzalni Windows](../articles/notification-hubs-windows-store-dotnet-get-started/)
    - [Windows Phone](../articles/notification-hubs-windows-phone-get-started/)
    - [iOS](../articles/notification-hubs-ios-get-started/)
    - [Android](../articles/notification-hubs-android-get-started/)
    - [Kindle](../articles/notification-hubs-kindle-get-started/)
    - [Baidu](../articles/notification-hubs-baidu-get-started/)
    - [Xamarin.iOS](../articles/partner-xamarin-notification-hubs-ios-get-started/)
    - [Xamarin.Android](../articles/partner-xamarin-notification-hubs-android-get-started/)

#### <a name="rendering"></a>Vizualizacije

Na gornjoj slici prikazuje prikazivanje azure.microsoft.com. Na stranici prikazanih GitHub na Birači prikazuju se kao popis s grafičkim oznakama veze.

###<a id="two-way-selectors"></a>Dvosmjerna Birači

Dvosmjerna Birači omogućuje korisnicima odaberite na teme s matricom dva načina. Ovo je ključna kad Azure tehnologije, kao što su mobilne usluge, podržava više pozadinskog platforme, kao i više klijenata. Imajte na umu sljedeće:

- Dok je dizajniran kao `(Platform | Backend)`, tekst dropwdown sada moguće je prilagoditi.
- Za svaku točku u vašem matrici ne morate stavke popisa, ali samo sadrži stavku koju temu URL-a postoji i nije duplikat.
- Veza može biti bilo koji URL iako se obično drugu temu GitHub.

Potražite u članku [Početak rada s mobilnih usluga](http://azure.microsoft.com/en-us/documentation/articles/mobile-services-ios-get-started/) da biste vidjeli kako Autor stvara 15 verzijama istom članku (9 mobilnu verziju klijentskog platforme i 2 pozadinskog platforme), ali korištenih Birači omogućiti navigaciju preko njih sve. Imajte na umu 3 članke nemaju obje verzije pozadinskog.

![Primjer dvosmjerni Birači](./media/custom-markdown-extensions/selector-list.png)

####<a name="syntax"></a>Sintaksa

    > [AZURE. BIRAČ popisa (Dropdown1 | Dropdown2])     -  [(Dropdown1Text1 | Dropdown2Text1)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text1 | Dropdown2Text2)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text2 | Dropdown2Text3)](../articles/dropdown1-text1-dropdown2-text1.md)
    - [(Dropdown1Text3 | Dropdown2Text4)](../articles/dropdown1-text1-dropdown2-text1.md)

Primjer:

    > [AZURE. BIRAČ popisa (platformu | Pozadinskog])     -  [(iOS | .NET)](./mobile-services-dotnet-backend-ios-get-started-push.md)
    - [(iOS | JavaScript)](./mobile-services-javascript-backend-ios-get-started-push.md)
    - [(Windows univerzalni C# | .NET)](./mobile-services-dotnet-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows univerzalni C# | JavaScript)](./mobile-services-javascript-backend-windows-universal-dotnet-get-started-push.md)
    - [(Windows Phone | .NET)](./mobile-services-dotnet-backend-windows-phone-get-started-push.md)
    - [(Windows Phone | JavaScript)](./mobile-services-javascript-backend-windows-phone-get-started-push.md)
    - [(sa sustavom Android | .NET)](./mobile-services-dotnet-backend-android-get-started-push.md)
    - [(sa sustavom Android | JavaScript)](./mobile-services-javascript-backend-android-get-started-push.md)
    - [(Xamarin iOS | JavaScript)](./partner-xamarin-mobile-services-ios-get-started-push.md)
    - [(Xamarin Android | JavaScript)](./partner-xamarin-mobile-services-android-get-started-push.md)

#### <a name="rendering"></a>Vizualizacije

Na gornjoj slici prikazuje prikazivanje azure.microsoft.com. Na stranici prikazanih GitHub na Birači prikazuju se kao popis s grafičkim oznakama veze.

<!--Anchors-->
[Bilješke i savjeti]: #notes-and-tips
[Obuhvaća]: #includes
[Umetanje videozapisa]: #embedded-videos
[Birači tehnologija i platforme]: #technology-and-platform-selectors

###<a name="contributors-guide-links"></a>Suradnici vodič veze

- [Članak](./../README.md)
- [Indeks članaka upute](./contributor-guide-index.md)
