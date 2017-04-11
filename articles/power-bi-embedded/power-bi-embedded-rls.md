<properties
   pageTitle="Sigurnost na razini retka s Power BI ugrađeni"
   description="Detalje o sigurnosti na razini retka s Power BI ugrađeni"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="row-level-security-with-power-bi-embedded"></a>Redak razinu sigurnosti s Power BI ugrađeni

Da biste ograničili korisničkog pristupa određeni podataka u izvješću ili skup podataka dopuštanje višestrukih različitim korisnicima da biste koristili isti izvješće tijekom seeing različitih vrsta se poslužite retka razinu sigurnosti (RLS). Power BI ugrađene sada podržava skupove podataka s RLS.

![](media\power-bi-embedded-rls\pbi-embedded-rls-flow-1.png)

Da biste iskoristili RLS, važno je razumjeti tri glavna koncepata; Korisnici, uloge i pravila. Pogledajmo o čemu pozornije razmotrite svaki:

**Korisnici** – to su stvarne krajnjim korisnicima prikaz izvješća. U dodatku Power BI ugrađene korisnici su otkrije svojstvo korisničkog imena u Token za aplikacije.

**Uloge** – korisnici pripadaju uloge. Uloge je spremnik za pravila, a možete se s nazivom kao što su "Voditelj prodaje" ili "Prodajni predstavnik". U dodatku Power BI ugrađene korisnici prepoznaju se po svojstvo uloge u Token za aplikacije.

**Pravila** – uloge se pravila, a ta pravila su stvarne filtri koji ćete primijeniti na podatke. To može biti jednostavno kao "država = sad" ili nešto slično dinamičnije.

### <a name="example"></a>Primjer

Do kraja ovog članka ćemo pružati primjera RLS za izradu, a koji koristi unutar ugrađene aplikacije. Našem primjeru koristi [Maloprodaja analizu oglednu](http://go.microsoft.com/fwlink/?LinkID=780547) datoteku PBIX.

![](media\power-bi-embedded-rls\pbi-embedded-rls-scenario-2.png)

Oglednim maloprodaja analiza prikazuje Prodaja za sve trgovine u određeni maloprodaja lanac. Bez RLS, bez obzira na to koji regionalni Upravitelj prijave i prikaza izvješća, vidjet ćete iste podatke. Upravljanje viši je odredio svaki Upravitelj Okrug trebali biste vidjeti samo prodaje za sprema se upravljanje, a da biste to učinili, koristimo RLS.

RLS je autor u Power BI Desktop. Prilikom otvaranja je skup podataka i izvješća, ne možemo možete prijeći na prikaz dijagrama da biste vidjeli sheme:

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-3.png)

Evo nekoliko savjeta koje možete obratite pozornost na to s ovom shemom:

-   Sve mjere, kao što je **Ukupna prodaja**, spremaju se u tablice činjenica **prodaje** .
-   Postoje četiri dodatne dimenzije povezane tablice: **stavke**, **vrijeme**, **pohrane**i **Okrug**.
-   Strelice na crte odnosa određuju koji vam način filtre možete teče iz jedne tablice u drugu. Ako, na primjer, ako je filtar na **vrijeme [Datum]**, u shemi trenutne ga želite samo filtrirati dolje vrijednosti u tablici **Prodaja** . Nema tablica bi utjecati filtar Budući da sva strelice na crte odnosa točke u tablici prodaje i ne odmah.
-   **Regionalni** tablica pokazuje tko je upravitelj za svaki Okrug:

    ![](media\power-bi-embedded-rls\pbi-embedded-rls-district-table-4.png)

Ako smo **Regionalni Upravitelj** stupca u tablici Okrug Primjena filtra, a ako koji korisnik prikazom izvješća filtriranje rezultata, filtar će filtrirati prema dolje **pohrane** i **Prodaja** tablice da biste prikazali samo podatke za taj određeni regionalni Upravitelj na temelju ovu shemu.

Evo kako:

1.  Na kartici Modeliranje kliknite **Upravljanje ulogama**.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-modeling-tab-5.png)

2.  Stvorite novu ulogu **Upravitelj**naziva.  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-6.png)

3.  U tablici **Okrug** unesite sljedeće DAX izraz: **[regionalni Upravitelj] = USERNAME()**  
![](media\power-bi-embedded-rls\pbi-embedded-rls-manager-role-7.png)

4.  Da biste bili sigurni pravila radite, na kartici **Oblikovanje** kliknite **Prikaz kao uloge**, a zatim unesite sljedeće:  
![](media\power-bi-embedded-rls\pbi-embedded-rls-view-as-roles-8.png)

    Izvješća sada prikazati podatke kao da su se prijavili kao **Ma promjena**.

Primjene filtra način smo nije ovdje će filtrirati prema dolje sve zapise u tablicama **Okrug**, **pohrane**i **prodaje** . Međutim, zbog smjer filtar odnose između **prodaje** i **vrijeme**tablice **Prodaja** i **stavke**te **stavke** i **vrijeme** ne filtrira se prema dolje.

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-9.png)

Koje se možda u redu da bi taj zahtjev, ako ne želimo upravitelji da biste pogledali stavke za koje nemaju nijedna Prodaja, ne možemo nije uključite dvosmjerne unakrsno filtriranje za odnos i tijeka filtar sigurnosti u oba smjera. To možete učiniti tako da uredite odnos između **prodaje** i **stavke**, ovako:

![](media\power-bi-embedded-rls\pbi-embedded-rls-edit-relationship-10.png)

Sada filtre možete i teče iz tablice Prodaja u tablici **stavke** :

![](media\power-bi-embedded-rls\pbi-embedded-rls-diagram-view-11.png)

**Napomena** Ako koristite načinu rada DirectQuery za podatke, morat ćete omogućiti Dvosmjeran unakrsno filtriranje tako da odaberete ove dvije mogućnosti:

1.  **Datoteka** -> **Mogućnosti i postavki** -> **Pretpregled značajke** -> **Omogući unakrsno filtriranje u oba smjera za DirectQuery**.
2.  **Datoteka** -> **Mogućnosti i postavki** -> **DirectQuery** -> **Dopusti neograničeni mjere u načinu rada DirectQuery**.


Da biste saznali više o Dvosmjeran unakrsno filtriranje, preuzmite koje [Dvosmjeran unakrsno filtriranje u SQL Server Analysis Services 2016 i Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) .

To prelama kopiju posla koje je potrebno poduzeti u Power BI Desktop, ali postoji jedan više informaciju slobode radni koje je potrebno poduzeti da bi se RLS pravila definirali rad u Power BI ugrađeni. Korisnici su provjere autentičnosti i odobrio aplikacije, a tokeni aplikacije koriste dopustiti pristup Konkretno izvješće dodatka Power BI ugrađene taj korisnik. Power BI ugrađene ne sadrži određene podatke na koji se korisniku. Za RLS rad, morate proći nekoliko dodatnih kontekst kao dio token za aplikaciju:
-   **korisničko ime** (neobavezno) – koristiti s RLS to je niz koji se mogu koristiti za identifikaciju korisnika prilikom primjene pravila RLS. Pogledajte korištenje retka ugrađene razinu sigurnosti pomoću dodatka Power BI
-   **uloge** – niz koji sadrži uloge da biste odabrali kad Primjena pravila sigurnosti na razini retka. Ako prosljeđivanje više uloga mora biti proslijeđena kao niz polja.

Ako postoji svojstvo korisničko ime mora proći i barem jedna vrijednost uloge.

Token punu aplikaciju izgledat će otprilike ovako:

![](media\power-bi-embedded-rls\pbi-embedded-rls-app-token-string-12.png)

Sada s svi dijelovi zajedno, kada se netko prijavi u našem aplikaciju za prikaz izvješća, oni će samo moći vidjeti podatke koje smiju da biste vidjeli, kao što je definirao oglednim sigurnosti na razini retka.

![](media\power-bi-embedded-rls\pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Vidi također
[Sigurnost na razini retka (RLS) s Power](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/)
