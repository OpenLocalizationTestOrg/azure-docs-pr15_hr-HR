<properties
    pageTitle="Početak rada s katalog podataka | Microsoft Azure"
    description="Kraj do kraja Praktični vodič prezentiranje scenarija i mogućnosti kataloga podataka Azure."
    documentationCenter=""
    services="data-catalog"
    authors="steelanddata"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/20/2016"
    ms.author="spelluru"/>

# <a name="get-started-with-azure-data-catalog"></a>Početak rada s Azure kataloga podataka
Azure katalog podataka je servis potpuno upravljanih oblaka koja služi kao sustav Registracija i sustava otkrivanje za enterprise podataka resursi. Detaljne pregled, potražite u članku [što je Azure kataloga podataka](data-catalog-what-is-data-catalog.md).

Pomoću ovog praktičnog vodiča pojednostavnjuje početak rada s Azure kataloga podataka. Pomoću ovog praktičnog vodiča provedite sljedeće postupke:

| Postupak | Opis |
| :--- | :---------- |
| [Dodjela resursa za katalog podataka](#provision-data-catalog) | U ovom postupku Dodjela resursa ili postavite Azure kataloga podataka. Ovaj korak ne samo ako kataloga nije postavljen prije. Čak i ako postoji više pretplata povezani s računom za Azure možete imati samo jedan katalog podataka po tvrtke ili ustanove (Microsoft Azure Active Directory domain). |
| [Registrirajte se Resursi podataka](#register-data-assets) | U ovom postupku registrirati imovine podataka iz AdventureWorks2014 oglednu bazu podataka pomoću kataloga podataka. Registracija je postupak izdvajanje ključa strukturnih metapodataka kao što su imena, vrsta i mjesta iz izvora podataka i kopiranje te metapodataka u katalog. Izvor podataka i podataka resursi ostaju gdje se nalazili, ali metapodatke koristi katalog da biste ih jednostavnije vidljivim i razumljiv. |
| [Otkrivanje podataka resursi](#discover-data-assets) | U ovom postupku pomoću portala za Azure katalog podataka da biste otkrili imovine podatke koji su registrirani u prethodnom koraku. Nakon registrirani izvora podataka pomoću kataloga podataka Azure, njegove metapodatke indeksirati servis tako da korisnici mogu jednostavno pretraživati za koje su im potrebne podatke. |
| [Dodavanje podataka resursi](#annotate-data-assets) | U ovom postupku imovine podataka navedite primjedbe (informacija kao što su opisa, oznake, dokumentaciju ili stručnjaka). Ove informacije nadopunjuju metapodataka dobivenih iz izvora podataka, a da biste izvor podataka više razumljiv još osoba. |
| [Povezivanje s podacima resursi](#connect-to-data-assets) | U ovom postupku otvorite resursima podataka u integriranom klijentskih alata (kao što su Excel i SQL Server Data Tools) i alata koji nisu integrirani (SQL Server Management Studio). |
| [Upravljanje resursima podataka](#manage-data-assets) | U ovom postupku postavljanja sigurnosti za imovinu podataka. Katalog podataka ne odobravanje pristupa same podatke. Vlasnik izvora podataka kontrola pristupa podacima. <br/><br/> Katalog podataka mogu otkriti izvora podataka i prikaz **metapodataka** vezane uz izvora registriranih u katalogu. Možda postoje situacije, međutim, gdje bi trebala biti vidljiva samo određenim korisnicima ili određene grupe izvora podataka. Za tim slučajevima, možete koristiti katalog podataka vlasništvo imovine registriranih podataka u katalogu i nadzirati vidljivost imovine ste vlasnik. |
| [Uklanjanje podataka resursi](#remove-data-assets) | U ovom postupku Saznajte kako se uklanjaju resursi podataka iz kataloga podataka. |  

## <a name="tutorial-prerequisites"></a>Praktični vodič preduvjeti

### <a name="azure-subscription"></a>Azure pretplate
Da biste postavili Azure katalog podataka, morate biti vlasnik ili suradnika kao Azure pretplate.

Azure pretplate lakše organizirali pristup oblaka resursima servisa kao što su Azure kataloga podataka. Oni i pomoći odrediti koliko je prijavio korištenje resursa, naplatiti i plaćenu. Pretplate može imati na različitim naplati i plaćanju postavljanje da biste imali različite pretplate i različite tarife odjela, project, regionalnog ureda i tako dalje. Svaki servis u oblaku pripada pretplatu, a morate imati pretplatu prije postavljanja Azure kataloga podataka. Dodatne informacije potražite u članku [upravljanje računima, pretplate i administratorske uloge](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Ako nemate pretplatu, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Detalje potražite u članku [Besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/) .

### <a name="azure-active-directory"></a>Azure Active Directory
Da biste postavili Azure katalog podataka, morate biti prijavljeni korisnički račun Azure Active Directory (Azure AD). Morate biti vlasnik ili suradnika kao Azure pretplate.  

Azure AD omogućuje jednostavnu za svoju tvrtku da upravljaju identiteta i access, u oblak i lokalne. Možete koristiti na jedan račun tvrtke ili obrazovne ustanove za prijavu na bilo koje web-aplikacije oblaka ili na lokalnim poslužiteljima. Azure katalog podataka koristi Azure AD za provjeru autentičnosti prijavu. Dodatne informacije potražite u članku [što je Azure Active Directory](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Konfiguracija pravila za Azure Active Directory

Koji se mogu pojaviti situaciji kojima se možete prijaviti na portal Azure katalog podataka, ali kada pokušate prijavite se u alat za registraciju izvora podataka, pojavi poruka o pogrešci koja sprječava prijave. Ta se pogreška može pojaviti kada niste na mreži tvrtke ili kada se povezujete s izvan mreže tvrtke.

Alat za registraciju koristi *provjeru autentičnosti obrazaca* za provjeru valjanosti korisnika s znak dodataka protiv Azure Active Directory. Za uspješan prijavu, administrator Azure Active Directory morate omogućiti provjere autentičnosti obrazaca u *globalni provjere autentičnosti pravila*.

Pomoću pravila globalni provjere autentičnosti Omogući provjeru autentičnosti zasebno za intranet i ekstranet veze, kao što je prikazano na sljedećoj slici. Pogrešaka pri prijavi se može pojaviti ako Provjera autentičnosti obrazaca nije omogućeno za mrežu iz kojeg se povezujete.

 ![Pravila globalni provjere autentičnosti za Azure Active Directory](./media/data-catalog-prerequisites/global-auth-policy.png)

Dodatne informacije potražite u članku [Konfiguriranje pravila za provjeru autentičnosti](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Dodjela resursa za katalog podataka
Možete Dodjela samo jedan katalog podataka po tvrtke ili ustanove (Azure Active Directory domain). Stoga ako vlasnik ili suradnika kao Azure pretplate koji pripada ovu domenu Azure Active Directory već stvorili katalog, neće moći ponovno stvoriti katalog čak i ako imate više pretplata Azure. Da biste provjerili hoće li katalog podataka stvorena je korisnik u vašoj domeni Azure Active Directory, idite na [početnoj stranici Azure katalog podataka](http://azuredatacatalog.com) i provjerite je li vam se prikazuje u katalog. Ako s katalogom već stvorili umjesto vas, preskočite ovaj postupak i prijeđite na sljedeći odjeljak.    

1. Idite na [stranicu servisa katalog podataka](https://azure.microsoft.com/services/data-catalog) i kliknite **Kreni**.

    ![Azure katalog podataka – marketing odredišna stranica](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Prijavite se pomoću korisnički račun koji je vlasnik ili suradnika kao Azure pretplate. Pogledajte sljedeće stranice nakon prijave.

    ![Azure katalog podataka – katalog podataka nakon dodjele resursa](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Unesite **naziv** za katalog podataka, **pretplatu** koju želite koristiti i web- **mjesto** kataloga.
4. Proširite odjeljak **određivanje cijena** i odaberite katalog podataka Azure **edition** (besplatno ili standardna).
    ![Azure katalog podataka – odabir edition](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Proširite **Kataloga korisnika** , a zatim kliknite **Dodaj** da biste dodali korisnike za kataloga podataka. Automatski se dodaju u tu grupu.
    ![Katalog podataka za Azure – korisnika](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Proširite **Kataloga administratori** i kliknite **Dodaj** da biste dodali dodatne administratorima kataloga podataka. Automatski se dodaju u tu grupu.
    ![Katalog podataka za Azure – administratori](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Kliknite **Stvaranje kataloga** da biste stvorili katalog podataka za tvrtku ili ustanovu. Posjetite početnu stranicu za katalog podataka nakon stvaranja.
    ![Katalog podataka Azure – stvoren](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Pronalaženje katalog podataka na portalu za Azure
1. U zasebnom na kartici oblikovanje u web-pregledniku ili u prozoru preglednika zasebnom web idite na [portal za Azure](https://portal.azure.com) i prijavite se pomoću isti račun koji ste koristili za stvaranje kataloga podataka u prethodnom koraku.
2. Odaberite **Pregledaj** , a zatim kliknite **Kataloga podataka**.

    ![Azure katalog podataka – pregled Azure](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) vidite katalog podataka koju ste stvorili.

    ![Azure katalog podataka – kataloga prikaza popisa](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  Kliknite katalog koji ste stvorili. Vidjet ćete plohu **Katalog podataka** na portalu.

    ![Azure katalog podataka – plohu portalu ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. Možete pregledavati svojstva kataloga podataka i ih ažurirati. Na primjer, kliknite **sloju određivanje cijena** i promijenite izdanje.

    ![Azure katalog podataka – cijene sloju](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Works oglednu bazu podataka
U ovom ćete praktičnom vodiču registrirati imovine podataka (tablice) iz AdventureWorks2014 oglednu bazu podataka za modul za baze podataka SQL Server, ali možete koristiti bilo koji podržani izvor ako biste radije da biste radili s podacima koji je poznato i važna za vašu ulogu. Popis podržanih izvora podataka, potražite u odjeljku [Podržani izvori podataka](data-catalog-dsr.md).

### <a name="install-the-adventure-works-2014-oltp-database"></a>Baze podataka tvrtke Adventure Works 2014 OLTP instalacije
Baze podataka za Adventure Works podržava standardnu online transakcije obrada scenariji za bicikl izmišljeni proizvođača (Adventure Works ciklusa), što obuhvaća proizvoda, prodaju i nabavu. U ovom ćete praktičnom vodiču Registrirajte se informacije o proizvodima u katalog podataka Azure.

Da biste instalirali Adventure Works oglednu bazu podataka:

1. Preuzmite [Adventure Works 2014 cijelog baze podataka Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) na CodePlex.
2. Vraćanje baze podataka na vašem računalu, slijedite upute u [Vraćanje sigurnosne kopije baze podataka pomoću SQL Server Management Studio](http://msdn.microsoft.com/library/ms177429.aspx)ili slijedeći ove korake:
    1. Otvorite SQL Server Management Studio i povezati modul za baze podataka SQL Server.
    2. Desnom tipkom miša kliknite **baze podataka** , a zatim kliknite **Vrati bazu podataka**.
    3. U odjeljku **Vraćanje baze podataka**kliknite mogućnost **uređaj** za **izvor** , a zatim **Pregledaj**.
    4. U odjeljku **Odaberite uređaji za sigurnosne kopije**, kliknite **Dodaj**.
    5. Idite u mapu na kojem je već **AdventureWorks2014.bak** datoteka, odaberite datoteku i kliknite **u redu** da biste zatvorili dijaloški okvir **Pronađite datoteku sigurnosne kopije** .
    6. Kliknite **u redu** da biste zatvorili dijaloški okvir **Odabir uređaje za sigurnosno kopiranje** .    
    7. Kliknite **u redu** da biste zatvorili dijaloški okvir **Vrati bazu podataka** .

Sada možete registrirati imovine podataka iz Adventure Works oglednu bazu podataka pomoću kataloga podataka Azure.

## <a name="register-data-assets"></a>Registrirajte se Resursi podataka

U ovom vježbu pomoću alata za registraciju da biste registrirali imovine podataka iz baze podataka za Adventure Works s katalogom. Registracija je postupak izdvajanje ključa strukturnih metapodataka kao što su imena, vrsta i mjesta iz izvora podataka i resursi sadrži i kopiranje te metapodataka u katalog. Izvor podataka i podataka resursi ostaju gdje se nalazili, ali metapodatke koristi katalog da biste ih jednostavnije vidljivim i razumljiv.

### <a name="register-a-data-source"></a>Izvor podataka morate registrirati

1.  Idite na [početnoj stranici Azure katalog podataka](https://azuredatacatalog.com) , a zatim kliknite **Objavi podataka**.

    ![Azure podataka kataloga – objavljivanje podataka gumb](media/data-catalog-get-started/data-catalog-publish-data.png)

2.  Kliknite **Pokreni aplikaciju** za preuzimanje, instaliranje i pokretanje alata za registraciju na vašem računalu.

    ![Azure katalog podataka – gumb za pokretanje](media/data-catalog-get-started/data-catalog-launch-application.png)

3. Na stranici **dobrodošlice** kliknite **Prijava** , a zatim unesite vjerodajnice.    

    ![Azure katalog podataka – stranica dobrodošlice](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

4. Na stranici **Katalog podataka za Microsoft Azure** kliknite **SQL Server** i **Dalje**.

    ![Azure katalog podataka – izvora podataka](media/data-catalog-get-started/data-catalog-data-sources.png)

5.  Unesite svojstva veze za SQL Server za **AdventureWorks2014** (pogledajte u sljedećem primjeru) i kliknite **POVEŽI**.

    ![Azure katalog podataka – postavke veze za SQL Server](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

6.  Registrirajte se metapodataka sredstava podataka. U ovom primjeru registrirali objekata **Radnih/proizvoda** iz AdventureWorks radnog prostora za naziv:

    1. U stablu **Hijerarhije poslužitelja** proširite **AdventureWorks2014** pa kliknite **proizvodnje**.
    2. Odaberite **proizvoda**, **ProductCategory**, **ProductDescription**i **ProductPhoto** pomoću Ctrl + klik.
    3. Kliknite **Premjesti odabrani strelicu** (**>**). Ova akcija premješta sve odabrane objekte u popisu **objekti Registriranje** .

        ![Azure Praktični vodič katalog podataka – pregled i odabir objekata](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
    4. Odaberite **Uključi pretpregled** da biste dodali pretpregled snimku podataka. Snimka sadrži do 20 zapise iz svaku tablicu, a ona se kopira u katalogu.
    5. Odaberite **Uključi podataka profila** da biste uključili snimke statistike objekt podataka profila (na primjer: minimalne, maksimalne i prosječne vrijednosti stupca, broj redaka).
    6. U polje **Dodavanje oznaka** unesite **adventure works, ciklusa**. Ova akcija dodaje oznake pretraživanja za imovine tih podataka. Oznake su odličan način da bi korisnicima pronalaženje izvora podataka registriranih.
    7. Navedite naziv programa **expert** na te podatke (neobavezno).

        ![Azure katalog podataka Praktični vodič – objekata Registriranje](media/data-catalog-get-started/data-catalog-objects-register.png)

    8. Kliknite **REGISTRACIJA**. Azure katalog podataka registrira odabrane objekte. U ovom vježbu registrirane odabrane objekte iz Adventure Works. Alat za registraciju izvlači metapodataka iz podataka resursa i kopira tih podataka u servisa Azure kataloga podataka. Ostaje podataka kojoj se trenutno nalazi i ona ostaje u odjeljku kontrola administratori i pravila trenutni sustav.

        ![Azure katalog podataka – registriranih objekata](media/data-catalog-get-started/data-catalog-registered-objects.png)

    9. Da biste vidjeli svoje objekti izvora podataka registriranih, kliknite **Prikaz Portal**. Na portalu za katalog podataka Azure potvrdite da vidite sve četiri tablice i baze podataka u prikazu rešetke.

        ![Objekti na portalu za Azure kataloga podataka ](media/data-catalog-get-started/data-catalog-view-portal.png)


U ovom vježbu registriran objekte iz Adventure Works oglednu bazu podataka tako da ih može jednostavno otkriti korisnici u vašoj tvrtki ili ustanovi. U sljedećem vježbu Saznajte kako otkriti imovine registriranih podataka.

## <a name="discover-data-assets"></a>Otkrivanje podataka resursi
Otkrivanje u katalog podataka Azure koristi dvije primarni mehanizme: pretraživanje i filtriranje.

Pretraživanje osmišljene intuitivno i naprednih. Prema zadanim postavkama pojmova za pretraživanje se podudara protiv neko svojstvo u katalogu, uključujući primjedbe navedene za korisnika.

Filtriranje namijenjen je nadopunjuju pretraživanje. Možete odabrati određene značajke kao što su stručnjake, vrsta izvora podataka, Vrsta objekta i oznake da biste pogledali imovine odgovarajuće podatke, a da biste ograničili rezultate pretraživanja na odgovarajuće resursi.

Pomoću kombinacija pretraživanja i filtriranja možete brzo prijeći izvora podataka registriranih Azure katalog podataka da biste otkrili imovine podataka morate.

U ovu vježbu pomoću portala za Azure katalog podataka da biste otkrili imovine podataka registriranih u prethodno vježbu. Detalje o sintaksi pretraživanja potražite u članku [Referenca sintaksu za pretraživanje kataloga podataka](https://msdn.microsoft.com/library/azure/mt267594.aspx) .

Slijede nekoliko primjera za otkrivanje imovine podataka u katalogu.  

### <a name="discover-data-assets-with-basic-search"></a>Otkrivanje podataka imovine s Osnovno pretraživanje
Osnovno pretraživanje omogućuje vam pretraživanje kataloga pomoću jednog ili više pojmova za pretraživanje. Rezultati su neki resursi koji odgovaraju na neko svojstvo s jednim ili više uvjete navedene.

1. Kliknite **Polazno** da biste na portalu za Azure kataloga podataka. Ako ste zatvorili web-preglednik, idite na [početnoj stranici Azure kataloga podataka](https://www.azuredatacatalog.com).
2. U okvir za pretraživanje unesite `cycles` i pritisnite **ENTER**.

    ![Azure katalog podataka – Osnovni tekst pretraživanja](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Provjerite da vidite sve četiri tablice i baze podataka (AdventureWorks2014) u rezultatima. Možete se prebacivati između **prikaza rešetke** i **prikaza popisa** pomoću gumba na alatnoj traci kao što je prikazano na sljedećoj slici. Obratite pozornost na to da ključna riječ za pretraživanje istaknuta je u rezultatima pretraživanja jer je **uključena mogućnost za **Isticanje** **. Navedite broj rezultata **po stranici** u rezultatima pretraživanja.

    ![Azure katalog podataka – Osnovni tekst rezultata pretraživanja](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)

    Ploča za **pretraživanje** na lijevoj strani a ploči **svojstava** na desnoj strani. Na ploči **pretraživanja** možete promijeniti kriterije pretraživanja i filtriranja rezultata. Ploča **Svojstva** prikazuje svojstva odabranih objekata na rešetki ili popis.

4. U rezultatima pretraživanja kliknite **proizvoda** . Kliknite **Pretpregled**, **stupce**, **Podataka profila**i kartice **dokumentaciju** ili kliknite strelicu za proširivanje dnu okna.  

    ![Azure katalog podataka – donje okno](media/data-catalog-get-started/data-catalog-data-asset-preview.png)

    Na kartici **Pregled** , vidjet ćete pretpregled podataka u tablici **Product** .  
5. Kliknite karticu **stupaca** da biste pronašli detalje o stupcima (kao što su **naziv** i **Vrsta podataka**) u resursa podataka.
6. Kliknite karticu **Podaci profila** da biste vidjeli Profiliranje podataka (na primjer: broj redaka, veličina podataka ili minimalnu vrijednost u stupcu) u resursa podataka.
7. Filtriranje rezultata, pomoću **filtara** na lijevoj strani. Ako, na primjer, kliknite **tablicu** za **Vrstu objekta**, a vidjeti samo četiri tablice, ne baze podataka.

    ![Azure katalog podataka – filtriranje rezultata pretraživanja](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Otkrivanje podataka imovine s svojstvo određivanje opsega
Određivanje opsega svojstvo olakšava pronalaženje podataka imovine gdje se traženi pojam uparuje s određenog svojstva.

1. Očisti filtar **tablice** u odjeljku **Vrsta objekta** u **filtrima**.  
2. U okvir za pretraživanje unesite `tags:cycles` i pritisnite **ENTER**. Sva svojstva koje možete koristiti za pretraživanje kataloga podataka potražite u članku [Referenca za sintaksu pretraživanje kataloga podataka](https://msdn.microsoft.com/library/azure/mt267594.aspx) .
3. Provjerite da vidite sve četiri tablice i baze podataka (AdventureWorks2014) u rezultatima.  

    ![Katalog podataka – svojstvo određivanje opsega rezultata pretraživanja](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>Spremanje pretraživanja
1. U oknu **pretraživanja** u odjeljku **Trenutno pretraživanje** unesite naziv za pretraživanje, a zatim kliknite **Spremi**.

    ![Azure katalog podataka – spremanje pretraživanja](media/data-catalog-get-started/data-catalog-save-search.png)
2. Provjerite da spremljeno pretraživanje se prikazuje u odjeljku **Spremljena pretraživanja**.

    ![Azure katalog podataka – spremljena pretraživanja](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Odaberite jednu od akcija koje možete provoditi na spremljeno pretraživanje (**Preimenovanje**, **Brisanje**, **Spremi kao zadano** pretraživanje).

    ![Azure katalog podataka – spremili mogućnosti pretraživanja](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Logički operatori
Možete proširili ili suzili pretraživanje s logički operatori.

1. U okvir za pretraživanje unesite `tags:cycles AND objectType:table`, a zatim pritisnite **ENTER**.
2. Provjerite da vidite samo tablice (ne baze podataka) u rezultatima.  

    ![Azure katalog podataka – logičkih operatora u pretraživanju](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Grupiranje u zagradama
Grupiranje u zagrade, možete grupirati dijelovi upita da biste postigli logičke odvajanja, osobito uz logički operatori.

1. U okvir za pretraživanje unesite `name:product AND (tags:cycles AND objectType:table)` i pritisnite **ENTER**.
2. Provjerite da vidite samo tablicu **proizvoda** u rezultatima pretraživanja.

    ![Azure katalog podataka – grupiranja pretraživanja](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Operatori usporedbe
S operatori usporedbe, možete koristiti usporedbe osim jednakosti za svojstva koje sadrže numeričke i datum vrste podataka.

1. U okvir za pretraživanje unesite `lastRegisteredTime:>"06/09/2016"`.
2. Očisti filtar **tablice** u odjeljku **Vrsta objekta**.
3. Pritisnite **ENTER**.
4. Provjerite da vidite tablice **proizvoda**, **ProductCategory**, **ProductDescription**i **ProductPhoto** i AdventureWorks2014 bazu podataka registriranih u rezultatima pretraživanja.

    ![Azure katalog podataka – Usporedba rezultata pretraživanja](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Detaljne informacije o otkrivanje web-mjesto imovine podataka i u okvir za [pretraživanje kataloga podataka sintaksa referenca](https://msdn.microsoft.com/library/azure/mt267594.aspx) za sintaksa za pretraživanje potražite u članku [Kako otkriti imovine podataka](data-catalog-how-to-discover.md) .

## <a name="annotate-data-assets"></a>Dodavanje podataka resursi
U ovu vježbu pomoću portala za katalog podataka Azure unos opaski u njih (Dodaj podatke kao što su opisa, oznake ili stručnjaka) podataka imovine prethodno registriran u katalogu. Primjedbe dodatku izjavi i poboljšati strukturnih metapodataka dobivenih iz izvora podataka tijekom registracije i olakšava imovine podataka puno da biste otkrili i razumijevanje.

U ovom vježbu dodajte opaske jedan podatkovni resursa (ProductPhoto). Podaci resursa ProductPhoto dodajte neslužbeni naziv i opis.  

1.  Idite na [početnoj stranici katalog podataka Azure](https://www.azuredatacatalog.com) i pretraživanje s `tags:cycles` da biste pronašli imovine podataka registriranih.  
2. Kliknite **ProductPhoto** u rezultatima pretraživanja.  
3. Unesite **slike proizvoda** **Neslužbeni naziv** i **fotografije proizvoda za marketinških materijala** za **Opis**.

    ![Azure katalog podataka – opis ProductPhoto](media/data-catalog-get-started/data-catalog-productphoto-description.png)

    **Opis** vas drugi mogu otkriti i razumijevanje Zašto i način korištenja resursa odabranih podataka. Dodavanje dodatnih oznaka i prikazali stupce. Sada možete pokušati pretraživanja i filtriranja da biste otkrili imovine podataka pomoću opisni metapodataka koje ste dodali u katalog.

Možete učiniti i sljedeće na ovoj stranici:

- Dodajte stručnjaka za sredstvo podataka. Kliknite **Dodaj** u područje **stručnjaka** .
- Dodavanje oznaka na razini skup podataka. Kliknite **Dodaj** u područje **oznaka** . Oznake može biti oznaku korisnika ili Pojmovnik oznake. Standardni kataloga za Edition podataka obuhvaća tvrtke rječnika koji olakšava kataloga administratori definiranje taksonomija središnje tvrtke. Korisnici kataloga zatim može dodavati opaske podataka imovine s uvjetima Pojmovnik. Dodatne informacije potražite u članku [upute za postavljanje tvrtke Pojmovnik za mjerodavni označavanje](data-catalog-how-to-business-glossary.md)
- Dodavanje oznaka na razini stupca. Kliknite **Dodaj** u odjeljku **oznake** za stupac koji želite označiti.
- Dodavanje opisa na razini stupca. Unesite **Opis** stupca. Možete pogledati i opis metapodataka dobivenih iz izvora podataka.
- Dodajte **zahtjeva za pristupom** informacije koje korisnicima se pokazuje kako zatražiti pristup resursa podataka.

    ![Azure katalog podataka – Dodavanje oznaka, opisi](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)

- Odaberite karticu **dokumentaciju** i navedite dokumentaciji resursa podataka. S dokumentaciju katalog podataka Azure katalog podataka možete koristiti kao sadržaja spremište da biste stvorili cjelovit narrative imovine podataka.

    ![Azure katalog podataka – kartica dokumentacija](media/data-catalog-get-started/data-catalog-documentation.png)


Resursi za više podataka možete dodati i opaske. Ako, na primjer, odaberite sve podatke resursi ste registrirali i navedite stručnjaka za njih.

![Azure katalog podataka – označiti više podataka resursi](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure katalog podataka podržava crowd predvidivosti pristup za opaske. Bilo koji korisnik katalog podataka možete dodati oznake (korisnika ili Pojmovnik), opis i drugih metapodataka tako da svaki korisnik s perspektivom podataka resursa i njezino korištenje može imati tu perspektive snimljenu i dostupne drugim korisnicima.

Detaljne informacije o opaske imovine podataka potražite u članku [kako označiti imovine podataka](data-catalog-how-to-annotate.md) .

## <a name="connect-to-data-assets"></a>Povezivanje s podacima resursi
U ovu vježbu otvorite imovine podataka iz alata za integriranu klijenta (Excel) i alata koji nisu integrirani (SQL Server Management Studio) pomoću podatke o vezi.

> [AZURE.NOTE] Važno je da ne zaboravite Azure katalog podataka ne nudi pristup izvoru podataka stvarni – je jednostavno olakšava vam da biste otkrili i razumijevanje ga. Kada se povežete s izvorom podataka, klijentska aplikacija odaberete koristi vjerodajnice za Windows ili od vas traži vjerodajnice prema potrebi. Ako vam nije prethodno odobren pristup izvoru podataka, morate dobiti pristup prije povezivanja.

### <a name="connect-to-a-data-asset-from-excel"></a>Povezivanje s podacima resursa iz programa Excel

1. Odaberite **proizvod** iz rezultata pretraživanja. Kliknite **Otvori u** na alatnoj traci, a zatim kliknite **Excel**.

    ![Azure katalog podataka – povezivanje s podacima resursa](media/data-catalog-get-started/data-catalog-connect1.png)
2. Kliknite **Otvori** u skočnom prozoru preuzimanja. Ovaj sučelja mogu se razlikovati ovisno o pregledniku.

    ![Azure katalog podataka – preuzimanje datoteka za povezivanje programa Excel](media/data-catalog-get-started/data-catalog-download-open.png)
3. U prozoru **Microsoft Excel sigurnosno upozorenje** kliknite **Omogući**.

    ![Azure katalog podataka – skočnog sigurnosti programa Excel](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Zadržali zadane postavke u dijaloškom okviru **Uvoz podataka** i kliknite **u redu**.

    ![Azure katalog podataka – uvoz podataka u Excel](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Prikaz izvora podataka u programu Excel.

    ![Azure katalog podataka – tablicu proizvod u programu Excel](media/data-catalog-get-started/data-catalog-connect2.png)

U ovom vježbu povezan s resursi za podatke pronađu pomoću kataloga podataka Azure. Pomoću portala za Azure katalog podataka možete povezati pomoću klijentske aplikacije integriranom izbornik za **Otvaranje u** . Možete povezati s bilo kojom aplikacijom odaberete pomoću podatke o vezi mjesto obuhvaćeno metapodataka resursa. Ako, na primjer, SQL Server Management Studio možete koristiti da biste se povezali s bazom podataka AdventureWorks2014 za pristup podacima u imovine podataka registriranih ovog praktičnog vodiča.

1. Otvorite **SQL Server Management Studio**.
2. U dijaloškom okviru **za povezivanje s poslužitelja** unesite naziv poslužitelja iz okna sa **svojstvima** na portalu za Azure kataloga podataka.
3. Pomoću odgovarajuće provjeru autentičnosti i vjerodajnice za pristup podacima resursa. Ako nemate pristup, koristite informacije u polje **Zatraži pristup** da biste ga.

    ![Azure katalog podataka – zahtjeva za pristupom](media/data-catalog-get-started/data-catalog-request-access.png)

Kliknite **Prikaz nizove veze** za prikaz i kopiranje ADF.NET, ODBC i OLEDB nizove veze u međuspremnik za korištenje u aplikaciji.

## <a name="manage-data-assets"></a>Upravljanje resursima podataka
U ovom ćete koraku potražite upute za postavljanje sigurnosti za imovinu podataka. Katalog podataka ne odobravanje pristupa same podatke. Vlasnik izvora podataka kontrola pristupa podacima.

Katalog podataka možete koristiti da biste otkrili izvore podataka, a da biste pogledali metapodatke povezane s izvorima registriran u katalogu. Možda postoje situacije, međutim, gdje izvore podataka treba vidljiv je samo za određene korisnike ili određene grupe. Ove scenarijima za katalog podataka možete koristiti da biste preuzeli vlasništvo imovine registriranih podataka u katalogu, a zatim kontrolu vidljivost imovine ste vlasnik.

> [AZURE.NOTE] Mogućnosti upravljanja opisane u ovu vježbu su dostupne samo u standardni izdanje programa Azure kataloga podataka, a ne u besplatne Edition.
U katalogu podataka Azure, možete vlasništvo nad podacima resursi, dodavanje dodatnih vlasnici imovine podataka i postaviti vidljivost imovine podataka.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Preuzimanje vlasništva nad podacima resursi i ograničiti vidljivost

1. Idite na [početnoj stranici Azure kataloga podataka](https://www.azuredatacatalog.com). U tekstni okvir za **pretraživanje** unesite `tags:cycles` i pritisnite **ENTER**.
2. Kliknite neku stavku na popisu rezultata, a zatim kliknite **Preuzimanje vlasništva** na alatnoj traci.
3. U odjeljku **Upravljanje** ploči **svojstava** kliknite **Preuzimanje vlasništva**.

    ![Azure katalog podataka – Preuzimanje vlasništva](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Da biste ograničili vidljivost, odaberite **vlasnika i ove korisnika** u odjeljku **vidljivost** , a zatim kliknite **Dodaj**. Unesite adrese e-pošte za korisnika u tekstni okvir i pritisnite **ENTER**.

    ![Azure katalog podataka – ograničavanja pristupa](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Uklanjanje podataka resursi

U ovu vježbu pomoću portala za Azure katalog podataka da biste uklonili Pretpregled podataka iz podataka registriranih resursi i brisanje podataka imovine iz kataloga.

U katalogu podataka Azure, možete je izbrisati pojedinačnih resursa ili brisanje više resursi.

1. Idite na [početnoj stranici Azure kataloga podataka](https://www.azuredatacatalog.com).
2. U tekstni okvir za **pretraživanje** unesite `tags:cycles` i pritisnite **ENTER**.
3. Odaberite stavku na popisu rezultata, a zatim kliknite **Izbriši** na alatnoj traci retka kao što je prikazano na sljedećoj slici:

    ![Azure katalog podataka – brisanje rešetke stavki](media/data-catalog-get-started/data-catalog-delete-grid-item.png)

    Ako koristite prikaz popisa, potvrdite okvir je lijevo od stavke kao što je prikazano na sljedećoj slici:

    ![Azure katalog podataka – brisanje stavki popisa](media/data-catalog-get-started/data-catalog-delete-list-item.png)

    Odaberite resursi više podataka i izbrisati ih kao što je prikazano na sljedećoj slici:

    ![Azure katalog podataka – brisanje više podataka resursi](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] Zadano ponašanje kataloga je omogućiti bilo koji korisnik da biste registrirali svim izvorima podataka, a da biste omogućili bilo koji korisnik da biste izbrisali sve podatke resursa koji je registriran. Mogućnosti upravljanja uključeni u standardni izdanje programa Azure kataloga podataka navedite dodatne mogućnosti za vlasništvo nad resursima ograničavanje koji mogu otkriti resursima i ograničavanje koji možete izbrisati resursi.


## <a name="summary"></a>Sažetak

U ovom ćete praktičnom vodiču istražili ključna mogućnosti kataloga podataka Azure, uključujući Registriranje, opaske, otkrivanje i upravljanje resursima podataka tvrtke. Sad kad dovršite vodič, vrijeme je za početak rada. Možete početi danas Registracija vi i vaš tim ovise o izvore podataka i pozivanje suradnika za korištenje kataloga.

## <a name="references"></a>Reference

- [Kako registrirati podataka resursi](data-catalog-how-to-register.md)
- [Upute za otkrivanje podataka resursi](data-catalog-how-to-discover.md)
- [Kako označiti podataka resursi](data-catalog-how-to-annotate.md)
- [Kako dokument podataka resursi](data-catalog-how-to-documentation.md)
- [Upute za povezivanje s podacima resursi](data-catalog-how-to-connect.md)
- [Kako upravljati podacima resursi](data-catalog-how-to-manage.md)
