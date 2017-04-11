<properties
    pageTitle="Podrška za zajedničko korištenje (CORS) resursa unakrsno Origin | Microsoft Azure"
    description="Saznajte kako omogućiti CORS podršku za Microsoft Azure servise za pohranu."
    services="storage"
    documentationCenter=".net"
    authors="cbrooks"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="cbrooks"/>

# <a name="cross-origin-resource-sharing-cors-support-for-the-azure-storage-services"></a>Zajedničko korištenje (CORS) podrška za servise za Azure pohranu resursa za više izvora

Počevši od verzije 2013-08-15, servise za Azure pohranu podržava zajedničko korištenje više Origin resursa (CORS) za servise Blob, tablice, reda čekanja i datoteke. CORS je HTTP značajka koja omogućuje web-aplikacije pokretanje u odjeljku jednu domenu da biste pristupili resursa u drugu domenu. Web-preglednici implementirati sigurnosnog ograničenja naziva [isti polazište pravila](http://www.w3.org/Security/wiki/Same_Origin_Policy) koja onemogućuje web-stranice iz pozivanja API-ji u drugu domenu; CORS omogućuje siguran način da biste omogućili jednu domenu (domenu izvor) da biste pozvali API-ji u drugu domenu. Pogledajte [CORS specifikacija](http://www.w3.org/TR/cors/) detalje o CORS.

Možete postaviti pravila CORS pojedinačno za svaku servise za pohranu tako da nazovete [Postaviti svojstva Blob servisa](https://msdn.microsoft.com/library/hh452235.aspx), [Postavite svojstva reda čekanja servisa](https://msdn.microsoft.com/library/hh452232.aspx)i [Postaviti svojstva tablice servisa](https://msdn.microsoft.com/library/hh452240.aspx). Nakon postavljanja CORS pravila za servis, a zatim je ispravno čija je autentičnost provjerena zahtjev protiv usluge iz druge domene će se računati za određivanje hoće li je dopušteno prema pravilima koje ste naveli.

>[AZURE.NOTE] Imajte na umu da CORS nije mehanizam provjere autentičnosti. Bilo koji zahtjev protiv resursa za pohranu kada je omogućen CORS mora imati ispravnu autorizaciju potpis ili mora izvršiti protiv javno resursa.

## <a name="understanding-cors-requests"></a>Objašnjenje CORS zahtjeva

Zahtjev za CORS iz domene origin može se sastojati od dva zasebna zahtjeva:

- Prije isporuke zahtjev, koje upite ograničenja CORS nameće servis. Zahtjev za prije isporuke potreban je osim ako je zahtjev za metodu [jednostavan način](http://www.w3.org/TR/cors/), što znači da GET, LAKŠI ili objavu.

- Stvarni zahtjev, u odnosu na željeni resursa.

### <a name="preflight-request"></a>Zahtjev za provjeru prije isporuke

Upiti prije isporuke zahtjev CORS ograničenja koja su uspostavljena za servis za pohranu vlasnik računa. Web-pregledniku (ili druge korisnički agent) šalje zahtjev za mogućnosti koji sadrži zaglavlja zahtjev, postupak i polazište domene. Servis za pohranu vrednuje svrhu operacija na temelju unaprijed konfigurirana skup pravila CORS koji određuju koji polazište domene, postupcima zahtjev i zaglavlja zahtjeva može biti navedeno stvarni zahtjev protiv resursa za pohranu.

Ako CORS omogućen za servis, a postoji CORS pravilo koje odgovara prije isporuke zahtjev, servis Odgovori s Šifra stanja 200 (u redu), a uključuje potrebna kontrola pristupa zaglavlja u odgovoru.

Ako CORS nije omogućen za servis ili nijedno pravilo CORS odgovara prije isporuke zahtjev, servis će odgovor na poruke uz Šifra stanja 403 (zabranjeno).

Ako je zahtjev za mogućnosti ne smije sadržavati potrebne CORS zaglavlja (Origin i pristup-kontrola-zahtjev-način zaglavlja), servis će odgovor na poruke uz Šifra stanja 400 (neispravan zahtjev).

Imajte na umu da prije isporuke zahtjev procjenjuje protiv servisu (Blob, reda čekanja i tablice), a ne na traženi resurs. Vlasnik računa potrebno je omogućiti CORS kao dio svojstva računa servisa za zahtjev za uspjeti.

### <a name="actual-request"></a>Stvarni zahtjev

Kada je korisnik prihvati zahtjev za provjeru prije isporuke i vraća odgovor, web-pregledniku će isporuka stvarni zahtjev na resursa za pohranu. Web-pregledniku će odbiti stvarni zahtjev odmah ako zahtjev za prije isporuke odbijena.

Zahtjev za stvarno se smatra običan zahtjev na servis za pohranu. Prisutnosti izvor zaglavlja označava da je zahtjev za CORS zahtjev, a zatim je servis provjerit će pravila podudaranja CORS. Ako se pronađe podudarajući kontakt, zaglavlja za kontrolu pristupa dodaje odgovor se i poslati klijent. Ako rezultat ne pronađe, Access kontrola CORS zaglavlja se vraćaju.

## <a name="enabling-cors-for-the-azure-storage-services"></a>Omogućivanje CORS za servise za pohranu za Azure

Pravila CORS postavljene na razini usluge da morate omogućiti ili onemogućiti CORS za svaki servis (Blob, reda čekanja i tablice) zasebno. Prema zadanim postavkama CORS nije omogućen za svaki servis. Da biste omogućili CORS, prvo morate postaviti svojstva odgovarajuće servisa pomoću verzije 2013 08 15 ili noviji, i dodati pravila za CORS svojstva servisa. Dodatne informacije o radi omogućivanja i onemogućivanja CORS za uslugu i da biste postavili pravila CORS pogledajte [Postaviti svojstva Blob servisa](https://msdn.microsoft.com/library/hh452235.aspx), [Postavite svojstva reda čekanja servisa](https://msdn.microsoft.com/library/hh452232.aspx)i [Postaviti svojstva tablice servisa](https://msdn.microsoft.com/library/hh452240.aspx).

Evo primjera jedan CORS pravilo navedeno putem operacije postaviti svojstva servisa:

    <Cors>    
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
            <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
            <MaxAgeInSeconds>200</MaxAgeInSeconds>
        </CorsRule>
    <Cors>

Svaki element uvrstiti u pravilo CORS je opisan u nastavku:

- **AllowedOrigins**: domene origin dopušteno poslati zahtjev za servis za pohranu putem CORS. Domenu izvor je domena iz koje potječe zahtjev. Imajte na umu da izvor mora biti u velika i mala slova podudaranja s polazište koje šalje dobi korisnika u servisu. Možete koristiti zamjenske znakove "*" da biste omogućili sve polazište domene zahtjeva putem CORS. U gornjem primjeru domene [http://www.contoso.com](http://www.contoso.com) i [http://www.fabrikam.com](http://www.fabrikam.com) olakšavaju zahtjeve protiv servisa pomoću CORS.

- **AllowedMethods**: metode (HTTP zahtjev glagoli) domenu izvor mogu koristiti za zahtjev za CORS. U gornjem primjeru dopušteno samo STAVI i GET zahtjeve.

- **AllowedHeaders**: zaglavlja zahtjeva koji domenu izvor navesti na zahtjev za CORS. U gornjem primjeru dopušteno sva metapodataka zaglavlja počevši od x-ms-meta-podataka, x-ms-meta-cilja, a x-ms-meta-abc. Imajte na umu da zamjenski znak "*" označava početak bilo kojeg zaglavlja s prefiksom navedeni dopuštena.

- **ExposedHeaders**: zaglavlja odgovora koja se može poslati odgovor na zahtjev za CORS i koji se prikazuje u pregledniku da biste izdavača zahtjev. U gornjem primjeru pregledniku je uputio da bi se prikazale sve zaglavlja počevši od x-ms-metapodataka.

- **MaxAgeInSeconds**: zahtjev za vrijeme maksimalno povećali pregledniku treba predmemoriju prije isporuke mogućnosti.

Servise za Azure pohranu podržava određivanje mjestu zaglavlja **AllowedHeaders** i **ExposedHeaders** elemente. Da biste omogućili kategorije zaglavlja, možete odrediti zajednički prefiks toj kategoriji. Na primjer, određivanje *x ms meta** kao naslov prefixed uspostavlja pravilo koje odgovara sva zaglavlja koje započinju s x ms meta.

Sljedeća ograničenja primjenjuju se na CORS pravila:

- Možete navesti do pet CORS pravila po servis za pohranu (Blob, tablice i reda čekanja).
- Maksimalna veličina sve CORS pravila postavke na zahtjev, isključujući XML oznake smije biti 2 KB.
- Duljina dopuštene zaglavlja, prikazanog zaglavlje ili dopušteni polazište ne smije prijeći 256 znakova.
- Dopuštene zaglavlja i omogućenog zaglavlja možda će biti nešto od sljedećeg:
  - Literal zaglavlja, gdje naziv točno zaglavlja nije naveden, kao što su **x-ms-meta-obrađuju**. Najviše 64 slovni zaglavlja možda biti naveden na zahtjev.
  - Mjestu zaglavlja, gdje se navedeni prefiks zaglavlja kao što je **x ms-meta-podataka***. Određivanje prefiks na taj način omogućuje ili izlaže bilo koje zaglavlje koji počinje s prefiksom danu. Maksimalno dva zaglavlja mjestu može biti naveden na zahtjev.
- Metoda (ili glagoli HTTP) navedena u elementu **AllowedMethods** mora biti usklađen sa metoda podržava Azure servis za pohranu API-ji. Podržanih metoda su brisanje, GET, ZAGLAVLJE, PISMA, objavu, mogućnosti i STAVI.

## <a name="understanding-cors-rule-evaluation-logic"></a>Objašnjenje logike za procjenu CORS pravila

Kada servis za pohranu primi zahtjev prije isporuke ili stvarnih, vrednuje taj zahtjev na temelju pravila CORS ste uspostavili servisa putem odgovarajuću operaciju postaviti svojstva servisa. Pravila CORS vrednuju se u redoslijedu kojim su postavljeni u tijelu zahtjeva operacije postaviti svojstva servisa.

Pravila CORS su koji se izračunava na sljedeći način:

1. Najprije domenu izvor zahtjeva se provjerava u odnosu domene za **AllowedOrigins** element na popisu. Ako domenu izvor uključen je na popisu ili sve domene koje su dopuštene s zamjenski znak "*", pravila procjenu prihod. Ako domenu izvor nije uključen, zatim zahtjev neće uspjeti.

2. Nakon toga način (ili glagolski HTTP) zahtjeva se provjerava u odnosu metode **AllowedMethods** element na popisu. Ako je metoda se nalazi na popisu, procjene pravila nastavlja; Ako nije, pa ne uspije zahtjev.

3. Ako zahtjev udovoljava pravila u svojoj domeni polazište i njegov način, to pravilo je odabran u tijeku vrednuju se zahtjeva i dalje pravila. Prije nego što možete uspije zahtjev, međutim, bilo kojeg zaglavlja koji je naveden u zahtjevu za su provjerena prema zaglavlja **AllowedHeaders** element na popisu. Ako zaglavlja poslane ne odgovaraju dopuštene zaglavlja, neće uspjeti zahtjev.

Budući da se pravila se obrađuju redoslijedom postoje u tijelu zahtjeva, najbolje prakse preporučujemo da na popisu, najprije Odredite pravila za većinu ograničenja vezana uz drugačijeg izvora tako da ih najprije vrednuju. Navedite pravila koje su manje ograničenja – na primjer, pravilo kojim se omogućuje sve drugačijeg izvora – na kraj popisa.

### <a name="example--cors-rules-evaluation"></a>Primjer – CORS pravila procjenu

Sljedeći primjer prikazuje tijelo djelomične zahtjev za operaciju da biste postavili CORS pravila za servise za pohranu. Potražite u članku [Postavljanje svojstva Blob servisa](https://msdn.microsoft.com/library/hh452235.aspx), [Postaviti svojstva reda čekanja servisa](https://msdn.microsoft.com/library/hh452232.aspx)i [Postaviti svojstva tablice servisa](https://msdn.microsoft.com/library/hh452240.aspx) pojedinosti o izgradnje zahtjev.

    <Cors>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>PUT,HEAD</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>*</AllowedOrigins>
            <AllowedMethods>PUT,GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-blob-content-type, x-ms-blob-content-disposition</AllowedHeaders>
        </CorsRule>
        <CorsRule>
            <AllowedOrigins>http://www.contoso.com</AllowedOrigins>
            <AllowedMethods>GET</AllowedMethods>
            <MaxAgeInSeconds>5</MaxAgeInSeconds>
            <ExposedHeaders>x-ms-*</ExposedHeaders>
            <AllowedHeaders>x-ms-client-request-id</AllowedHeaders>
        </CorsRule>
    </Cors>

Nakon toga razmotrite sljedeće zahtjeva za CORS:

Zahtjev||| Odgovor||
---|---|---|---|---
**Način** |**Polazište** |**Zahtjev za zaglavlja** |**Pravilo Match** |**Rezultat**
**POSTAVLJANJE** | http://www.contoso.com |x-ms-blob-vrste sadržaja | Prvo pravilo |Uspjeh
**POČETAK** | http://www.contoso.com| x-ms-blob-vrste sadržaja | Drugo pravilo |Uspjeh
**POČETAK** | http://www.contoso.com| x-ms-blob-vrste sadržaja | Drugo pravilo | Pogreška

Zahtjev za prvu odgovara prvo pravilo – domenu izvor odgovara dopuštene drugačijeg izvora, metodu usklađuje dopuštene metode i zaglavlje odgovara dopuštene zaglavlja – i tako uspješnog.

Zahtjev za drugi ne odgovara prvo pravilo jer metodu odgovara dopuštene metode. Je, no odgovara drugo pravilo tako da ga slijedi.

Zahtjev za treći odgovara drugo pravilo na svojim domenu izvor i metoda tako da se vrednuju se daljnje pravila. Međutim, *zaglavlja x-ms-– zahtjev-id klijenta* nije dozvoljen prema drugo pravilo tako da ne uspije zahtjev, bez obzira na činjenica da bi ste dopustili semantiku treće pravilo da biste uspjeti.

>[AZURE.NOTE] Iako ovaj primjer pokazuje manje ograničenja pravilo prije neku veća ograničenja, Općenito, najbolje je najprije popis najčešće ograničenja pravila.

## <a name="understanding-how-the-vary-header-is-set"></a>Objašnjenje kako je postavljeno razlikovanje zaglavlja

Zaglavlje *razlikovanje* je standardni HTTP/1.1 zaglavlja koji se sastoji od skupa polja za zaglavlja zahtjev koji savjeta preglednika ili korisnički agent o kriterijima koji su odabrani poslužitelj za obradu zahtjeva. U zaglavlju *razlikovanje* uglavnom se koristi za predmemoriranje proxy poslužitelji, preglednika i CDN-ovi, koji koristite da biste odredili kako predmemoriju odgovor. Detalje potražite u članku specifikacija za [razlikovanje zaglavlja](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).

Kada preglednik ili neki drugi korisnički agent predmemorira odgovor na zahtjev CORS, domenu izvor predmemoriranih će se kao izvor dopuštene. Kada drugi domene problemi isti zahtjev za resursa za pohranu dok je aktivna mogućnost predmemorije, korisnički agent dohvaća predmemorirani origin domene. Drugi domene ne odgovara predmemorirani domene tako da zahtjev ne uspijeva kada bi inače uspjeti. U određenim slučajevima Azure prostora za pohranu postavlja zaglavlje razlikovanje **polazište** da biste korisnički agent da biste poslali zahtjev za kasnije CORS sa servisom prilikom traženja domene razlikuje se od predmemorirane polazište uputiti.

Azure prostora za pohranu postavlja zaglavlje *razlikovanje* **Origin** stvarni GET-GLAVE zahtjeva u sljedećim slučajevima:

- Kada origin zahtjev za potpuno je podudaran dopuštene origin definira CORS pravila. Da biste se točno podudaranje, pravilo CORS može obuhvaćati zamjenski znak "*" znakova.

- Postoji bez pravilo koje odgovara polazište zahtjev, ali CORS je omogućen za servis za pohranu.

U slučaju gdje se zahtjev za GET-GLAVE podudara CORS pravila koja omogućuje sve drugačijeg izvora, odgovor upućuje na to da sve drugačijeg izvora dopušteno i predmemoriju korisnički agent Dopusti daljnji zahtjevi s bilo koje domene polazište dok je aktivna mogućnost predmemoriju.

Imajte na umu da načine koji nisu GET-GLAVE zahtjeva, za servise za pohranu neće postaviti zaglavlje razlikovanje jer odgovore na ove načine su predmemorirani po korisničkih agenata.

Sljedeća tablica pokazuje kako Azure prostora za pohranu će odgovoriti na zahtjeve za GET-GLAVE koji se temelji na prethodno spomenuta slučajeva:

Zahtjev|Postavke računa i rezultat izvođenja pravila|||Odgovor|||
---|---|---|---|---|---|---|---|---
**Zaglavlje izvor izlaganje na zahtjev** | **CORS pravila za taj servis** | **Postoji Matching pravila koja omogućuje sve drugačijeg izvora (*)** | **Postoji odgovarajući pravilo za polazište točno podudaranje** | **Odgovor sadrži razlikovanje zaglavlja postavljena na izvor** | **Odgovor obuhvaća pristup-kontrola – dopuštena-polazište: "*"** | **Odgovor sadrži Access-kontrola-izložen-zaglavlja**
ne|ne|ne|ne|ne|ne|ne
ne|Da|ne|ne|Da|ne|ne
ne|Da|Da|ne|ne|Da|Da
Da|ne|ne|ne|ne|ne|ne
Da|Da|ne|Da|Da|ne|Da
Da|Da|ne|ne|Da|ne|ne
Da|Da|Da|ne|ne|Da|Da


## <a name="billing-for-cors-requests"></a>Naplata CORS zahtjeva za

Uspješan prije isporuke zahtjeva za naplatu ako ste omogućili CORS za sve servise za pohranu za vaš račun (tako da nazovete [Postaviti svojstva Blob servisa](https://msdn.microsoft.com/library/hh452235.aspx), [Postavite svojstva servisa reda čekanja](https://msdn.microsoft.com/library/hh452232.aspx)ili [Postaviti svojstva tablice servisa](https://msdn.microsoft.com/library/hh452240.aspx)). Da biste minimizirali naknade, razmislite o postavku **MaxAgeInSeconds** element pravila CORS veliki da bi korisnički agent predmemorira zahtjev.

Nije uspjelo prije isporuke zahtjeve neće naplaćivati.

## <a name="next-steps"></a>Daljnji koraci

[Postavljanje svojstava Blob servisa](https://msdn.microsoft.com/library/hh452235.aspx)

[Postavljanje svojstava za servis reda čekanja](https://msdn.microsoft.com/library/hh452232.aspx)

[Postavljanje svojstava za servis tablice](https://msdn.microsoft.com/library/hh452240.aspx)

[W3C unakrsno polazište resursa specifikacija za zajedničko korištenje](http://www.w3.org/TR/cors/)
