<properties
   pageTitle="Upravljanje sliku virtualnog računala na trgovine Windows Azure | Microsoft Azure"
   description="Detaljni vodič o upravljanju sliku virtualnog računala na trgovine Windows Azure nakon početnog publikacije."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="Azure"
   ms.workload="na"
   ms.date="08/03/2016"
   ms.author="hascipio;"/>

# <a name="post-production-guide-for-virtual-machine-offers-in-the-azure-marketplace"></a>Nakon radnog vodič za virtualnog računala ponude u Azure Marketplace

U ovom se članku objašnjava kako ažurirati uživo ponudu virtualnog računala u Azure Marketplace. I vodi vas na postupak dodavanja jednog ili više nove SKU-ove ponude za postojeće i uklonite uživo ponudu virtualnog računala ili SKU iz trgovine Azure Marketplace.

Kada ponudu/SKU je kopirana bez postavljanja [Azure portalu](http://portal.azure.com), ne možete promijeniti polja dolje:

- **Nude identifikator:** [Portal za objavljivanje -> virtualnim strojevima -> Odaberi tu ponudu -> kartica VM slike -> nude identifikator]
- **SKU identifikator:** [Portal za objavljivanje -> virtualnim strojevima -> Odabir -> tu ponudu SKU-ove -> kartica dodavanje SKU]
- **Prostor za naziv programa publisher:** [Portal za objavljivanje -> virtualnim strojevima -> Vodič vođenje kroz nam o svojoj tvrtki (pronaći u odjeljku "Korak 2 registrirati vaše tvrtke") -> -> kartica prostora za naziv programa Publisher -> prostor naziva]

Kada se ponuda/SKU nalazi se u [Trgovine Windows Azure](http://azure.microsoft.com/marketplace), ne možete promijeniti polja dolje:

- **Nude identifikator:** [Portal za objavljivanje -> virtualnim strojevima -> Odaberi tu ponudu -> kartica VM slike -> nude identifikator]
- **SKU identifikator:** [Portal za objavljivanje -> virtualnim strojevima -> Odabir -> tu ponudu SKU-ove -> kartica dodavanje SKU]
- **Prostor za naziv programa publisher:** [Portal za objavljivanje -> virtualnim strojevima -> Vodič -> kartica prostora za naziv programa Publisher Recite nam o vaše tvrtke (pronaći u odjeljku korak 2 registrirati) -> prostor naziva]
- **Priključci** [Portal za objavljivanje -> virtualnim strojevima -> Odaberi tu ponudu -> kartica VM slike -> Otvori priključke]
- **Cijene promjena navedenih SKU(s)**
- **Promjena modela navedenih SKU(s) za naplatu**
- **Uklanjanje regije navedene SKU(s) za naplatu**
- **Promjena na disku podataka broj navedenih SKU(s)**



## <a name="1-how-to-update-the-technical-details-of-a-sku"></a>1. kako ažurirati tehničke pojedinosti SKU

Možete dodati nove verzije navedenih SKU i ponovno objaviti tu ponudu slijedeći korake navedene u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com).
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu **VM SLIKE** .
4. Iz odjeljka **SKU-ove** karticu **VM SLIKE** , pronađite SKU koji želite ažurirati.
5. Nakon toga, dodajte novi broj verzije se SKU, a zatim kliknite gumb **"+"** . Novu verziju treba oblika x.y.z, a gdje su X i Y Z cijelim brojevima. Samo mora biti rastuća Promjena verzije.
6. U okvir **OS VHD URL** dodati potpis zajednički pristup URI stvorene za operacijski sustav VHD i spremite željene promjene.

    >[AZURE.IMPORTANT] Koje nije moguće povećanje/smanjenje disk podataka broj navedenih SKU-om. Morate stvoriti nove SKU u tom slučaju. Pogledajte odjeljak [3. Kako dodati nove SKU u odjeljku navedenih ponudu](#3-how-to-add-a-new-sku-under-a-live-offer) detaljne upute.

7. Kada unesete promjene, idite na karticu **OBJAVLJIVANJE** , a zatim kliknite gumb **AUTOMATSKE PRIPREMNA**. Detaljne upute o testiranju tu ponudu u okruženje za pripremna pogledajte ovu [vezu](marketplace-publishing-vm-image-test-in-staging.md)
8. Kada u pripremna testirate tu ponudu, idite na kartici **OBJAVLJIVANJE** u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img01_07.png)

## <a name="2-how-to-update-the-non-technical-details-of-an-offer-or-a-sku"></a>2. kako ažurirati nisu tehničke prirode detalja ponude ili SKU

Možete ažurirati u nisu tehničke prirode (marketing, pravne, podržava, kategorija) detalja uživo ponudu ili SKU u Azure Marketplace.

### <a name="21-update-the-offer-description-and-logos"></a>2.1 ažuriranje ponudu opisa i logotipa

Možete ažurirati Detalji o ponudi i ponovno objaviti tu ponudu slijedeći korake u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com).
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu **MARKETINŠKE** .
4. Kliknite gumb za **ENGLESKI (SAD)** .
5. Na izborniku na lijevoj strani strane, kliknite karticu **DETALJI** . U odjeljku *Opis* na kartici **pojedinosti** možete ažurirati naslov ponuda, nude sažetak, nude dugo sažetak i spremite željene promjene.

    >[AZURE.NOTE] Provjerite pobrinuti sljedeće dok ažurirate SKU detalje.
    **Unesite udvostručeni tekst u odjeljku opis ponude i opis SKU-om. Unesite udvostručeni tekst u odjeljku naslov SKU i ponuda dugo sažetka. Unesite udvostručeni tekst u odjeljku naslov SKU i sažetak ponudu.**

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img02.1_05.png)

6. U odjeljku *LOGOTIPA* na kartici **pojedinosti** možete ažurirati logotipa. Međutim, pobrinite se da logotipa slijede [smjernice za Azure Marketplace](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (potražite u odjeljku korak 1: Navedite trgovine marketinške sadržaj -> Detalji o -> Smjernice za logotip trgovine Azure).

    >[AZURE.NOTE] Ikona glavni nije obavezno. Možete odabrati da ne glavni ikona prijenos. Međutim, nakon prijenosa glavni ikona zatim postoji bez dodjele resursa da biste izbrisali iz objavljivanje portal. U tom slučaju morate slijediti [smjernice ikona glavni](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content) (pogledajte odjeljak korak 1: Navedite trgovine marketinške sadržaj -> Detalji o -> dodatne smjernice za logotip natpis glavni).

7. Kada unesete promjene, idite na karticu **OBJAVLJIVANJE** , a zatim kliknite gumb **AUTOMATSKE PRIPREMNA**. Detaljne upute o testiranju tu ponudu u okruženje za pripremna pogledajte ovu [vezu](marketplace-publishing-vm-image-test-in-staging.md).
8. Kada u pripremna testirate tu ponudu, idite na kartici **OBJAVLJIVANJE** u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img02.1_08.png)

### <a name="22-update-the-sku-description"></a>2.2. Ažurirajte opis SKU

Možete ažurirati SKU pojedinosti i ponovno objaviti tu ponudu slijedeći korake u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com)
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu **MARKETINŠKE** .
4. Kliknite gumb za **ENGLESKI (SAD)** .
5. Na izborniku na lijevoj strani strane, kliknite karticu **TARIFE** . U odjeljku *SKU-ove* kartice **TARIFE** ažuriranje SKU naslov, SKU sažetka i detalja SKU opis i spremite željene promjene.

    >[AZURE.NOTE] Provjerite pobrinuti sljedeće dok ažurirate SKU detalje. **Unesite udvostručeni tekst u odjeljku opis ponude i opis SKU-om. Unesite udvostručeni tekst u odjeljku naslov se SKU i ponuda dugo sažetka. Unesite udvostručeni tekst u odjeljku naslov SKU i sažetak ponudu.**

6. Kada unesete promjene, idite na karticu **OBJAVLJIVANJE** , a zatim kliknite gumb **AUTOMATSKE PRIPREMNA**. Detaljne upute o testiranju tu ponudu u okruženje za pripremna pogledajte ovu vezu
7. Kada u pripremna testirate tu ponudu, idite na kartici **OBJAVLJIVANJE** u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img02.2_07.png)

### <a name="23-change-the-existing-links-or-add-new-links"></a>2.3 promijenite postojeće veze ili dodajte nove veze

Možete promijeniti postojeće veze ili dodavanje nove veze i zatim ponovno objaviti tu ponudu slijedeći korake u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com)
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu **MARKETINŠKE** .
4. Kliknite gumb za **ENGLESKI (SAD)** .
5. Na izborniku za strani s lijeve strane kliknite na kartici **veze** .
6. Ako želite da biste dodali novu vezu, zatim u odjeljku *veze* sekciju kliknite gumb **DODAJ VEZU** . Otvorit će se dijaloški okvir *"Dodaj vezu"* . U ovom dijaloškom okviru možete dodati vezu na naslov i URL polja i spremite željene promjene. Možete unijeti bilo koju vezu koja sadrži podatke koji vam mogu pomoći klijente.
7. Ako želite ažurirati ili izbrisali postojeće veze, zatim odaberite odgovarajuću vezu i kliknite gumb Uređivanje ili gumb Izbriši sukladno tome.

    >[AZURE.NOTE] Provjerite funkcionira li su veze koje ste unijeli u tom odjeljku ispravno, kao što je ove veze se provjeriti vaš zahtjev tijekom radnog.

8. Kada unesete promjene, idite na karticu **OBJAVLJIVANJE** , a zatim kliknite gumb **AUTOMATSKE PRIPREMNA**. Detaljne upute o testiranju tu ponudu u okruženje za pripremna pogledajte ovu [vezu](marketplace-publishing-vm-image-test-in-staging.md).
9. Kada u pripremna testirate tu ponudu, idite na kartici **OBJAVLJIVANJE** u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img02.3_09-01.png)

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img02.3-2.png)

### <a name="24-change-an-existing-sample-image-or-add-a-new-sample-image"></a>2.4 promijeniti postojeću sliku uzorak ili dodajte novu sliku uzorka

Možete promijeniti postojeće ogledne slike ili dodali novi ogledne slike i zatim ponovno objaviti tu ponudu slijedeći korake u nastavku:

>[AZURE.NOTE] Slika primjera samo jedno se prikazuje u [https://portal.azure.com](https://portal.azure.com).

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com)
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu **MARKETINŠKE** .
4. Kliknite gumb za **ENGLESKI (SAD)** .
5. Na izborniku na lijevoj strani strane, kliknite karticu **OGLEDNE SLIKE** .
6. Ako želite da biste dodali novu sliku uzorka, u odjeljku *Ogledne slike* kliknite gumb za **PRIJENOS odgovora NOVU SLIKU** , a zatim spremite promjene.

    >[AZURE.NOTE] Slika primjera uključujući korak nije obavezan.

7. Ako želite ažurirati ili izbrisati postojeću sliku uzorka, zatim pronađite odgovarajući uzorak slika i zatim kliknite gumb **ZAMIJENITI SLIKU** ili gumb Izbriši sukladno tome.

8. Kada unesete promjene, idite na karticu **OBJAVLJIVANJE** , a zatim kliknite gumb **AUTOMATSKE PRIPREMNA**. Detaljne upute o testiranju tu ponudu u okruženje za pripremna pogledajte ovu [vezu](marketplace-publishing-vm-image-test-in-staging.md).
9. Kada u pripremna testirate tu ponudu, idite na kartici **OBJAVLJIVANJE** u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img02.4_09.png)

### <a name="25-update-the-legal-content"></a>2,5 ažuriranje pravnog sadržaja

Možete ažurirati pravnog sadržaja i ponovno objaviti tu ponudu slijedeći korake u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com)
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu **MARKETINŠKE** .
4. Kliknite gumb za **ENGLESKI (SAD)** .
5. Na izborniku na lijevoj strani strane, kliknite karticu **pravne** . U odjeljku *pravne* možete ažurirati na pravila/Uvjeti korištenja. Unesite ili zalijepite pravila/uvjeta u tekstni okvir *Uvjete korištenja* i spremite željene promjene.
6. Ograničenje broja znakova za pravne uvjete korištenja je 1,000,000 znakova.
7. Kada unesete promjene, idite na karticu **OBJAVLJIVANJE** , a zatim kliknite gumb **AUTOMATSKE PRIPREMNA**. Detaljne upute o testiranju tu ponudu u okruženje za pripremna pogledajte ovu [vezu](marketplace-publishing-vm-image-test-in-staging.md)
8. Kada u pripremna testirate tu ponudu, idite na kartici **OBJAVLJIVANJE** u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img02.5_08.png)

### <a name="26-update-the-support-information"></a>2,6 informacije o podršci za ažuriranje

Možete ažurirati informacije o podršci i ponovno objaviti tu ponudu slijedeći korake u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com)
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu za **podršku** .
4. U odjeljku *Inženjerskim obratite* **PODRŠCI** kartice možete ažurirati pojedinosti o kontaktu. Te detalje koriste se za internu komunikaciju između partnera i Microsoft samo.
5. U odjeljku *Korisničke podrške* na kartici **podršku** možete ažurirati podršku detalje o kontaktu kao što su **ime, e-pošte, telefona** i **URL podrške**. Te detalje koriste se za internu komunikaciju između partnera i Microsoft samo.

    >[AZURE.NOTE] Ako želite podržavaju samo e-pošte, navedite sustava telefonskog broja u odjeljku **Korisničkoj podršci** . U ovom slučaju navedeni e-pošte koristit će se.

6. Kada unesete promjene, idite na karticu **OBJAVLJIVANJE** , a zatim kliknite gumb **AUTOMATSKE PRIPREMNA**. Detaljne upute o testiranju tu ponudu u okruženje za pripremna pogledajte ovu [vezu](marketplace-publishing-vm-image-test-in-staging.md)
7. Kada u pripremna testirate tu ponudu, idite na kartici **OBJAVLJIVANJE** u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img02.6_07.png)

### <a name="27-update-the-categories"></a>2.7 ažuriranje kategorije

Možete ažurirati u odjeljku kategorije za tu ponudu i ponovno objaviti tu ponudu slijedeći korake u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com)
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu **kategorije** .
4. U odjeljku *kategorije* ažuriranje kategorije za tu ponudu i spremite željene promjene. Možete odabrati do pet kategorija za galeriju trgovine Windows Azure.
5. Kada unesete promjene, idite na karticu **OBJAVLJIVANJE** , a zatim kliknite gumb **AUTOMATSKE PRIPREMNA**. Detaljne upute o testiranju tu ponudu u okruženje za pripremna pogledajte ovu [vezu](marketplace-publishing-vm-image-test-in-staging.md)
6. Kada u pripremna testirate tu ponudu, idite na kartici **OBJAVLJIVANJE** u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img02.7_06.png)

## <a name="3-how-to-add-a-new-sku-under-a-listed-offer"></a>3. kako dodati nove SKU u odjeljku navedenih ponudu

U odjeljku uživo ponudu možete dodati nove SKU slijedeći korake navedene u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com).
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku za strani s lijeve strane kliknite na kartici **SKU-ove** . Nakon te kliknite gumb **DODAJ A SKU**.  Otvorit će se novi dijaloški okvir. Unesite identifikatora SKU malim slovima. Potvrdite potvrdni okvir Premjesti-vaše-vlasnik naplate model(BYOL) ako želite objaviti nove SKU s BYOL modelom za naplatu. U suprotnom, poništite potvrdni okvir za BYOL. Nakon te kliknite crtičnih oznaka u dijaloškom okviru Stvaranje nove SKU-om. Jeste li ne odlučite li za naplatu model BYOL za nove SKU-om, zatim naplate modela postavit će se automatski za svaki sat za nove SKU-om. Ako želite omogućiti 30days probna za svaki sat naplate model, zatim kliknite mogućnost "Mjesec" za "dostupna besplatnu probnu verziju?". U suprotnom odaberite "Ne probnu VERZIJU". [Bilješka: mogućnost "koje je besplatnu probnu verziju dostupne?" samo prikazuje se ako niste odabrali BYOL u dijaloškom okviru prilikom stvaranja nove SKU.]

    >[AZURE.IMPORTANT] Mogućnost "Sakrij ovaj SKU iz trgovine jer je moraju uvijek biti kupili putem predloška rješenja" mora biti označene kao "Da" samo ako su odobrene za objavljivanje predloška ponude za rješenja u Azure Marketplace. U suprotnom, tu mogućnost uvijek moraju biti označene kao "Ne".

4. Sada na izborniku na lijevoj strani strane, kliknite karticu **VM SLIKE** te saznati nove SKU koji ste stvorili.
5. Da biste postavili novi SKU, odnose se na KORAK 5 ove [veze](marketplace-publishing-vm-image-creation.md#5-obtain-certification-for-your-vm-image) smjernicama.
6. Da biste dodali marketinških materijala za nove SKU-om, pogledajte odjeljak korak 1: Navedite trgovine marketinške sadržaj -> Detalji o -> točku broja 2 do 5 tu [vezu](marketplace-publishing-push-to-staging.md#step-1-provide-marketplace-marketing-content).
7. Da biste dodali cijene informacije za nove SKU-om, pogledajte odjeljak 2.1. Postavljanje cijenama VM ove [veze](marketplace-publishing-push-to-staging.md#step-2-set-your-prices)
8. Kada unesete promjene, idite na karticu **OBJAVLJIVANJE** , a zatim kliknite gumb **AUTOMATSKE PRIPREMNA**. Detaljne upute o testiranju tu ponudu u okruženje za pripremna pogledajte ovu [vezu](marketplace-publishing-vm-image-test-in-staging.md)
9. Kada u pripremna testirate tu ponudu, idite na kartici **OBJAVLJIVANJE** u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img03_09-01.png)

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img03_09-02.png)

## <a name="4-how-to-change-the-data-disk-count-for-a-listed-sku"></a>4. kako promijeniti broj disk podataka za navedene SKU

Koje nije moguće povećanje/smanjenje disk podataka broj navedenih SKU-om. U ovom slučaju morate stvaranje nove SKU-om. Pogledajte odjeljak [3. Kako dodati nove SKU u odjeljku uživo ponudu](#3-how-to-add-a-new-sku-under-a-live-offer) detaljne upute.

## <a name="5---how-to-delete-a-listed-offer-from-the-azure-marketplace"></a>5. kako izbrisati navedenih ponudu iz trgovine Azure Marketplace

Postoje razne aspekte koji moraju biti izvedena brigu o u slučaju zahtjev da biste uklonili uživo ponudu. Slijedite korake u nastavku da biste dobili upute za podršku da biste uklonili navedenih ponudu iz trgovine Azure Marketplace:

1.  Podizanje podršku karata pomoću ove [veze](https://support.microsoft.com/en-us/getsupport?wf=0&tenant=ClassicCommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=15635&ccsid=635993707583706681)
2.  Kao **"Ponuda za upravljanje"** odaberite vrstu problema, a zatim odaberite kategoriju kao **"Izmjene u ponudu i/ili SKU već nalazi u radnom"**
3.  Slanje zahtjeva

Timu za podršku će vas voditi kroz postupak brisanja ponudu/SKU-om.

>[AZURE.NOTE] Uvijek možete izbrisati ponudu dok je u statusom skica (odnosno nije u ODRŽAVANJE ili PROIZVODNJU) tako da kliknete gumb **ODBACI skica** na kartici **POVIJEST** .

## <a name="6-how-to-delete-a-listed-sku-from-the-azure-marketplace"></a>6. kako izbrisati navedenih SKU iz trgovine Azure Marketplace

Navedenih SKU iz trgovine Azure možete izbrisati tako da slijedite korake navedene u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com).
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Bočno okno lijevoj strani kliknite na kartici **SKU-ove** .
4. Odaberite SKU koji želite izbrisati i kliknite gumb Izbriši protiv te SKU-om.
5. Kada završi, idite na kartici OBJAVLJIVANJE u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti ponudu u Azure Marketplace.
6. Nakon što ponudu ponovno dobiva objavljenim u članku trgovine Windows Azure, se SKU će se izbrisati iz trgovine Azure i portalu Azure.

## <a name="7-how-to-delete-the-current-version-of-a-listed-sku-from-the-azure-marketplace"></a>7. kako izbrisati trenutnu verziju navedenih SKU iz trgovine Azure Marketplace

Trenutnu verziju navedenih SKU iz trgovine Azure možete izbrisati tako da slijedite korake navedene u nastavku. Nakon dovršetka postupka se SKU bit će vraćen prethodne verzije.

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com).
2.  Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3.  Bočno okno lijevoj strani kliknite na karticu **VM SLIKE** .
4.  Odaberite SKU čije trenutnu verziju koju želite izbrisati i kliknite gumb Izbriši u odnosu na toj verziji.
5.  Kada završi, idite na kartici **OBJAVLJIVANJE** u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti ponudu u Azure Marketplace.
6.  Nakon što ponudu ponovno dobiva objavljenim u članku trgovine Windows Azure, trenutnu verziju navedenih SKU će se izbrisati iz trgovine Azure i portalu Azure. Se SKU bit će vraćen prethodne verzije.

## <a name="8-how-to-revert-listing-price-to-production-values"></a>8. kako vratiti stavku cijenu u vrijednosti radnog
Promijenio sam cijene navedenih SKU (ili ste uklonjena naplate regije navedene SKU). Budući da nije podržan u trgovine Windows Azure, želim vratiti svoje promjene na vrijednosti radnog. Kako postići koji?

Slijedite korake navedene u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com).
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu **Određivanje CIJENA** .
4. Na kartici određivanje cijena odaberite područje čije cijene želite vratiti.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img08-04.png)

5. U slučaju SKU-ove s svaki sat naplate modelom, ponovno postavite cijene za sve jezgri kao što su u proizvodnje za odabranu regiju. Za SKU-ove s BYOL naplate modelom, dostupnim se SKU u regiji tako da poništite potvrdni okvir protiv SKU u odjeljku DOSTUPNOST SKU EXTERNALLY-LICENSED (BYOL) (pogledajte snimka u nastavku).

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img08-05.png)

6. Sada kliknite gumb **AUTOPRICE drugih TRŽIŠTIMA koji se TEMELJI na CIJENE u UNITED stanja**.

    >[AZURE.NOTE] Natpis na gumb mogu se razlikovati ovisno o regije koje ste odabrali. Jer smo Sjedinjenih Američkih Država ste odabrali prilikom stvaranja ovog dokumenta, pa gumb je označena kao "Automatsko cijena druge tržištima na temelju cijena u Sjedinjenim Američkim Državama" u nastavku snimku zaslona.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img08-06.png)

7. Otvorit će se čarobnjak za automatsko cijena. Prva stranica prikazuje odabir za osnovnu tržištu. Provjerite svoje i prijelaz na sljedeću stranicu klikom na gumb **"->"** .

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img08-07.png)

8. Mogućnost za odabir jezgri i tarife prikazat će se na stranici 2. Odaberite željeni tarife i u jezgri i kliknite "->" gumb.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img08-08.png)

9. Stranica 3 prikazuje tržištima/regije. Kliknite gumb Uključi/Isključi sve da biste odabrali sve regije ili ručno potvrdite okvire za regiju. Kliknite gumb "->" da biste premjestili na sljedeću stranicu.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img08-09.png)

10. Stranica 4 prikazuje tečajeve. Kliknite gumb Završi da biste prošli korake. Čarobnjak će vratiti cijene prema vašem odabiru.

11. Sada prijeđite na karticu cijene i kliknite gumb "PRIKAZ SAŽETKA i promjena".
Odaberite "Skice" u odjeljak "Prikaz verzije" i "Radni" u odjeljku "Usporedi s" (pogledajte snimka u nastavku). Ako vidite cijene razlike, znači da cijene sadrži je vratit će vrijednosti radnog uspješno.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img08-11.png)

12. Kada unesete promjene, idite na karticu OBJAVLJIVANJE, a zatim kliknite gumb **AUTOMATSKE PRIPREMNA**. Detaljne upute o testiranju tu ponudu u okruženje za pripremna pogledajte ovu [vezu](marketplace-publishing-vm-image-test-in-staging.md)
13. Kada u pripremna testirate tu ponudu, idite na kartici OBJAVLJIVANJE u objavljivanje portal, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

## <a name="9-how-to-revert-billing-model-to-production-values"></a>9. kako vratiti naplate model radnog vrijednosti
Promijenio sam naplate model navedenih SKU-om. Budući da nije podržan u trgovine Windows Azure, želim vratiti svoje promjene na vrijednosti radnog. Kako postići koji?

Slijedite korake u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com).
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu **SKU-ove** .
4. Kliknite gumb Uređivanje da biste se vratili na naplatu model. Otvorit će se prozor. Potvrdite ili poništite potvrdni okvir **'naplata i licenciranje obavlja vanjsko iz Azure (ili Premjesti vaše vlastite licence)'** sukladno tome.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img09-04.png)

5. Jednom pogledajte odgovor na pitanje 8 u ovom dokumentu da biste vratili natrag cijene.
6. Nakon koji idite na kartici **OBJAVLJIVANJE** u objavljivanje i slanje ponuda za pripremna na portal. Jednom ste gotovi s testiranjem ponudu, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

## <a name="10-how-to-revert-visibility-setting-of-a-listed-sku-to-the-production-value"></a>10. kako vratiti postavci vidljivosti od navedenih SKU vrijednosti radnog

Slijedite korake u nastavku:

1. Prijavite se na [portal za objavljivanje](https://publish.windowsazure.com).
2. Idite na karticu **VIRTUALNIM računalima sustava** , a zatim odaberite tu ponudu.
3. Na izborniku na lijevoj strani strane, kliknite karticu **SKU-ove** .
4. Odaberite vaše SKU i vratiti postavka vidljivosti SKU radnog vrijednosti.

    ![Crtanje](media/marketplace-publishing-vm-image-post-publishing/img10-04.png)

5. Jednom ste gotovi s promjenama, a zatim kliknite gumb **Zahtjev za ODOBRENJE na AUTOMATSKE da BISTE radni** da biste ponovno objaviti tu ponudu u Azure Marketplace.

## <a name="see-also"></a>Vidi također
- [Početak rada: Kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md)
- [Objašnjenje Prodavač uvida izvješćivanje o pogreškama](marketplace-publishing-report-seller-insights.md)
- [Objašnjenje payout izvješća](marketplace-publishing-report-payout.md)
- [Kako promijeniti svoje incentive prodavača davatelju rješenja oblaka](marketplace-publishing-csp-incentive.md)
- [Otklanjanje uobičajenih poteškoća objavljivanje na tržištu](marketplace-publishing-support-common-issues.md)
- [Podrška kao u programu publisher](marketplace-publishing-get-publisher-support.md)
- [Stvaranje lokalne VM slike](marketplace-publishing-vm-image-creation-on-premise.md)
- [Stvaranje virtualnog računala sa sustavom Windows Azure pretpregled portalu](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
