<properties
   pageTitle="Implementacija tu ponudu trgovine Windows Azure | Microsoft Azure"
   description="Dodatne informacije o i voditi kroz upute za implementaciju ponudu – virtualnog računala slike, servis za razvojne inženjere, podatkovnog servisa, itd., Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/02/2016"
   ms.author="hascipio" />

# <a name="deploy-your-offer-to-the-azure-marketplace"></a>Implementacija tu ponudu trgovine Windows Azure
Kada budete zadovoljni tu ponudu (to jest, što testirate scenarija, marketing sadržaj, itd.) i spremni ste pokretanje zahtjeva **automatske proizvodnje** na kartici **Objavljivanje** .  

1. Četiri koraka u odjeljku u PRIKAZU stranice u objavljivanje portal mora biti dovršeni i zelena. Za ponude virtualnog računala, provjerite je li da se sljedećih smjernica iza.

    ![Crtanje][img-pubportal-walkthru-checked]

2. Odaberite karticu **Objavi** na popisu s lijeve strane.

    ![Crtanje][img-pubportal-menu-publish]

3. Kliknite gumb **zahtjev za odobrenje da biste na radni**. Kada se dogode zahtjev, tim za odobrenje izvršava konačni pregled, a zatim tu ponudu bit će dostupni u trgovine Windows Azure.

    ![Crtanje][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] U slučaju virtualnim strojevima prilikom klika na gumb zahtjev za odobrenje da biste na radni, sljedeće se izvode iza scene. Moći ćete prikaz tijeka svakog koraka na kartici OBJAVLJIVANJE u objavljivanje portal. Morate potvrdite ovu stranicu u pravilnim intervalima (dok je za status prikazana vrijednost "Listed") da biste saznali sve pogreške koje je potrebno ispravak na kraj.

> - Zahtjev za radni isprva odlazak u tim certifikata koji je provjera valjanosti na vhd. Međutim, ako ažurirate već navedenih ponudu, a imate samo marketinške promjena zahtjev, zatim korak ustanova preskače.
> - Zahtjev na sljedeći korak, dođite tima sadržaja provjere valjanosti u čijem se provjeriti marketinške sadržaj ponudu.
> - Ako su prema gore navedenim uputama uspješan, ponudu je odobreno u radnog. Trenutno stanje postaju "na popisu" portal za objavljivanje. Međutim, ovaj status "Listed" impliciraju dovršetka postupka. Sljedeći koraci prije moraju biti dovršeno ponudu dostupan je u trgovine Windows Azure.
> - Kada ponudu odobri u proizvodnje u koraku gore, replikacije ponudu će se početi preko svih Azure podatkovnim centrima. Obično traje 24-48hours za replikaciju da biste dovršili no može potrajati i do tjedan ovisno o veličini u vhd. Međutim, ako ažurirate ponude za već popisu i ima dodan samo marketinške promjena, zatim na replikacije je brže.
> - Po dovršetku na replikacije zatim ponudu bit će dostupni u trgovine Windows Azure.

> Uvijek možete izbrisati ponudu dok je u statusom **Skica** (odnosno nikad **automatske pripremna** ili **Pritisni za proizvodnju**). Na kartici **Povijest** , kliknite gumb **Odbaci skica** pri dnu stranice da biste izbrisali skicu.


## <a name="production-checklist-for-all-virtual-machine-offers"></a>Proizvodne kontrolnog popisa za sve ponude virtualnog računala

- Provjerite radite li s partnerom Certificirano za Microsoft Azure
- Na kartici SKU-ove mogućnost stavke "Sakrij ovaj SKU iz trgovine jer je moraju uvijek biti kupili putem predloška rješenja" mora biti označene kao da samo ako se SKU je dio predloška rješenja. U svim drugim slučajevima, tu mogućnost moraju uvijek biti označen kao ne.
- Imajte na umu: Mijenjati na SKU vidljivost postavljanje kada se SKU nalazi. Ne podržavamo tu funkciju.
- Provjerite je li da se logotipa drži smjernice logotip trgovine Windows Azure dolje.
- Opis ponude i SKU bi trebalo biti iste.
- SKU-naslov i ponuditi dugo sažetak bi trebalo biti iste.
- SKU naslov i ponuditi sažetak bi trebalo biti iste.
- Naslovi SKU moraju biti jednake za ponudu s više SKU-ove.

**Smjernice za logotip Azure Marketplace**

- Azure dizajn ima palete jednostavne boja. Broj primarni i sekundarni boja na zadržati logotipa niske.
- Boje teme portala za Azure su bijele i crne. Dakle izbjegavanje ove boje kao boje pozadine vaše logotipa. Koristite neki boju koja bi vaše logotipa izravne poruke na portalu za Azure. Preporučujemo da se jednostavno primarne boje. Ako koristite prozirnu pozadinu, zatim provjerite je li logotip/tekst bijel ili crn.
- Korištenje prijelaza boja na pozadini na logotip.
- Izbjegavajte smještanje teksta, čak i vaše tvrtke ili naziv marke na logotip.
- Izgled i dojam logotipa mora biti 'paušalni i izbjegavajte prijelaza.
- Logotip mora biti Rastegnuto.

**Dodatne smjernice za logotip glavni:**

- Logotip glavni nije obavezno. U programu publisher možete odabrati ne da biste prenijeli glavni logotip. **No jednom prenesenu ikonu glavni ne može izbrisati iz objavljivanje portal. U tom trenutku partnera moraju pratiti trgovine Windows Azure smjernice za glavni ikone još ponudu nisu odobrene radnog.**
- Zaslonsko ime Publisher, SKU naslov i ponuda dugo sažetak prikazuju se u bijela fontom. Dakle izbjegavajte zadržavanjem sve svijetlu boju pozadine ikonu glavni. Crna, bijeli i Prozirna pozadina nije dopuštena za glavni ikone.
- U programu publisher prikazati ime, SKU naslov, ponudu dugo sažetka i gumb Stvori ugrađeni programski unutar logotip glavni kada ponudu dolazi navedenih. Tako da ne biste trebali unositi tekst prilikom dizajniranja glavni logotip. Samo ostavite prazan prostor na desnoj strani jer tekst (odnosno Prikaz programa Publisher ime, naslov SKU dugo sažetak ponudu) uvrstiti programski tako da nam iznad nje. Prazan prostor za tekst mora biti 415 x 100 s desne strane (i pomaka po 370px s lijeve strane).


## <a name="additional-production-checklist-for-already-listed-virtual-machine-offers"></a>Nudi dodatne radnog kontrolni popis za već navedenih virtualnog računala

- Provjerite postoji li već ponude s istim nazivom ponudu iz vaše tvrtke. Ako je da, trebali biste dodati novu verziju se SKU u ponudu za postojeće umjesto stvaranja novu ponudu duplicirane.
- Podatkovni disk mijenjati između dvije verzije iste SKU-om.
- Trgovine Windows Azure ne podržava cijene promjena navedenih SKU-ove kao što je to utječe naplata postojeće korisnike. Provjerite da ne mijenjate cijene navedenih SKU-ove u kojem je dostupna na SKU regijama. Međutim, možete dodati nove SKU-ove ili dodajte nove područja postojeće SKU.


## <a name="next-steps"></a>Daljnji koraci
Nakon što ponudu ide uživo, testirajte scenariji klijenta za sve ugovore i funkcije funkcionirao u radnom okruženju kao testirati i provjeriti valjanost pripremna okruženje za provjeru valjanosti.

## <a name="see-also"></a>Vidi također
- [Početak rada: kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]:media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]:media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]:media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png
