<properties
   pageTitle="Implementacija StorSimple virtualne polja 3 – postavljanje virtualnog uređaja kao datotečnom poslužitelju"
   description="Pomoću ovog praktičnog vodiča treći implementacije StorSimple virtualne polja upućuje postavljanje virtualnog uređaja kao datotečnom poslužitelju."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor=""/>

<tags
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/26/2016"
   ms.author="alkohli"/>

# <a name="deploy-storsimple-virtual-array---set-up-as-file-server"></a>Implementacija polja virtualne StorSimple – postavljanje gore kao datotečnom poslužitelju

![](./media/storsimple-ova-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Uvod 

Ovaj se članak odnosi Microsoft Azure StorSimple virtualne polja (poznat i kao StorSimple lokalnog virtualnog uređaja ili StorSimple virtualnog uređaja) izvodi ožujak 2016 Općenito dostupan (GA) izdanje. U ovom se članku opisuje kako izvesti početnog postavljanja, registrirati poslužitelj datoteka StorSimple, dovršite postavljanje uređaja i stvaranje i povezivanje s razlozima za odabir dionice. Ovo je zadnji članka u nizu vodiči za implementaciju potrebne za potpuno implementaciju sustava virtualne polja kao poslužitelj datoteka ili poslužitelju komponente iSCSI.

Postupak instalacije i konfiguracije može potrajati oko 10 minuta da biste dovršili.


## <a name="setup-prerequisites"></a>Preduvjeti za instalaciju

Prije konfigurirati i postavljanje uređaja virtualne StorSimple, učinite sljedeće:

-   Imate dodjeli virtualnog uređaja i s njom povezano kao detaljne [dodjele resursa StorSimple virtualne polja u Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) ili [dodjele resursa StorSimple virtualne polja u VMware](storsimple-ova-deploy2-provision-vmware.md).

-   Imate ključa za registraciju servisa StorSimple Upravitelj servisa koji ste stvorili za upravljanje StorSimple virtualne uređajima. Dodatne informacije potražite u članku [Korak 2: Registracija ključ servisa](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) za StorSimple virtualne polja.

-   Ako je drugi ili kasnije virtualne uređaj koji se registrirate za postojeći servis za Upravitelj StorSimple, imat ćete ključa za šifriranje podataka servisa. Ovaj ključ je generirana pri prvom uređaju uspješno je registriran na taj servis. Ako ste izgubili ovog ključa, potražite u članku [se ključa za šifriranje podataka servisa](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) za vaše StorSimple virtualne polja.

## <a name="step-by-step-setup"></a>Detaljnog postavljanja

Pomoću sljedećih detaljne upute za postavljanje i konfiguriranje StorSimple virtualnog uređaja.

## <a name="step-1-complete-the-local-web-ui-setup-and-register-your-device"></a>Korak 1: Dovršavanje postavljanja korisničkog Sučelja lokalne web i registrirati uređaju 


#### <a name="to-complete-the-setup-and-register-the-device"></a>Da biste dovršili postavljanje i registrirali uređaj

1.  Otvorite prozor preglednika i povezivanje s lokalnom web korisničkog Sučelja. Vrsta: 

    `https://<ip-address of network interface>`

    Pomoću URL veze u prethodnom koraku. Vidjet ćete poruku pogreške koja ukazuje da postoji problem sa sigurnosnim certifikatom web-mjesta. Kliknite **Nastavi da biste ovu web-stranicu**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image2.png)

1.  Prijavite se na webu UI virtualnog uređaja kao **StorSimpleAdmin**. Unesite lozinku administratora uređaja koji ste promijenili u koraku 3: pokretanje virtualnog uređaja u [dodjele resursa StorSimple virtualne polja u Hyper-V](storsimple-ova-deploy2-provision-hyperv.md) ili [dodjele resursa StorSimple virtualne polja u VMware](storsimple-ova-deploy2-provision-vmware.md).

    ![](./media/storsimple-ova-deploy3-fs-setup/image3.png)

1.  Će biti preusmjereni na stranicu **Početna stranica** . Ova stranica opisuje različite postavke možete konfigurirati i registrirati virtualnog uređaja sa servisom StorSimple Manager. Imajte na umu **mrežne postavke**, **Postavke proxyja Web**i **postavke vremena** nisu obavezni. Samo potrebne postavke su **Postavke uređaja** i **oblak**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image4.png)

1.  Na stranici **postavke mreže** u odjeljku **mreža sučelja**podataka 0 će se automatski konfigurirati za vas. Svaki sučelje mreže postavljena prema zadanim postavkama automatski dohvatiti IP adresa (DHCP). Dakle, je IP adresa, podmreže i pristupnika dodijelit će se automatski (za IPv4 i IPv6).

    ![](./media/storsimple-ova-deploy3-fs-setup/image5.png)

    Ako ste dodali više od jedne mreže sučelja tijekom dodjeljivanja uređaj, možete ih konfigurirati ovdje. Imajte na umu mrežno sučelje možete konfigurirati kao IPv4 samo ili kao IPv4 i IPv6. IPv6 ne podržava samo konfiguracije.

1.  DNS poslužitelji potrebni su jer kada uređaj pokuša možete komunicirati s davateljima usluga za pohranu na cloud ili da biste riješili uređaju koriste prema nazivu kada konfigurirati kao datotečnom poslužitelju. Na stranici **postavke mreže** , u odjeljku **DNS poslužitelji**:

    1.  Primarnih i sekundarnih DNS poslužitelj će se automatski konfigurirati. Ako se odlučite za konfiguriranje statičke IP adrese, možete odrediti DNS poslužitelji. Za visoke dostupnosti, preporučujemo da konfigurirate primarne i sekundarne DNS poslužitelj.

    2.  Kliknite **Primijeni**. To će se primijeniti i Provjerite valjanost postavki mreže.

2.  Na stranici **Postavke uređaja** :

    1.  Dodijelite jedinstveni **naziv** na uređaj. Ovaj naziv može sadržavati najviše 1-15 znakova i mogu sadržavati slova, brojeve i spojnice.

    2.  Kliknite ikonu **datotečnom poslužitelju** ![](./media/storsimple-ova-deploy3-fs-setup/image6.png) za **vrste** uređaja koje stvarate. Poslužitelj datoteka omogućuju stvaranje zajedničke mape.

    3.  Dok je uređaj datotečnom poslužitelju, morate uključiti uređaj s domenom. Unesite **naziv domene**.

    1.  Kliknite **Primijeni**.

2.  Pojavit će se dijaloški okvir. Unesite vjerodajnice za domenu u navedenom obliku. Kliknite ikonu provjeri. Provjerit će se vjerodajnice domene. Prikazat će se poruka o pogrešci ako nisu pravilne vjerodajnice.

    ![](./media/storsimple-ova-deploy3-fs-setup/image7.png)

1.  Kliknite **Primijeni**. To će se primijeniti i Provjerite valjanost postavki uređaja.

    ![](./media/storsimple-ova-deploy3-fs-setup/image8.png)

    > [AZURE.NOTE]
    > 
    > Provjerite je li vaš virtualne polja je u vlastitom organizacijsku jedinicu (OU) za Active Directory i primijenjeno ili nasljeđuju bez objekti pravilnika grupe (GPO). Pravilnik grupe mogu instalirati programe kao što je antivirusni softver na polja virtualne StorSimple. Instaliranja dodatnog softvera nije podržan, a može dovesti do oštećenja podataka. 

1.  (Nije obavezno) konfigurirajte web-proxy poslužitelj. Iako web-konfiguracija proxy nije obavezan, imajte na umu da ako koristite proxy poslužitelj za web, možete samo ga konfigurirati ovdje.

    ![](./media/storsimple-ova-deploy3-fs-setup/image9.png)

    Na stranici **Web proxy** :

    1.  Navedite **web-URL proxy poslužitelja** u ovom obliku: *http://&lt;glavno računalo IP adresa ili FDQN&gt;: broj priključka*. Imajte na umu da HTTPS URL-ova nisu podržane.

    2.  Navedite **provjeru autentičnosti** kao **Osnovni** ili **ništa**.

    3.  Ako pomoću provjere autentičnosti i morat ćete unijeti **korisničko ime** i **lozinku**.

    4.  Kliknite **Primijeni**. To će provjeriti i primijenite postavke proxyja konfigurirani web.

1.  (Nije obavezno) konfigurirati postavke za svoj uređaj, kao što su vremenske zone i NTP poslužitelje primarnih i sekundarnih datuma i vremena. NTP poslužitelje potrebni su jer uređaj, morate sinkronizirati vrijeme tako da možete uspješnoj vaše oblaka davateljima usluga.

    ![](./media/storsimple-ova-deploy3-fs-setup/image10.png)

    Na stranici **postavke vremena** :

    1.  S padajućeg popisa odaberite **vremensku zonu** na temelju zemljopisnu lokaciju u kojem se uvodi uređaj. Zadanu vremensku zonu za svoj uređaj je pst datoteke. Uređaja koristit će se ovaj vremenske zone za sve operacije zakazano.

    2.  Određivanje **primarnog NTP poslužitelja** za svoj uređaj ili prihvatite zadane vrijednosti time.windows.com. Provjerite dopušta li mrežom NTP promet prenesite iz vaše podatkovnog centra s Internetom.

    3.  Po želji možete navesti **sekundarnog NTP poslužitelja** za svoj uređaj.

    4.  Kliknite **Primijeni**. To će se provjeriti i primjenu postavki konfiguriranog vremena.

1.  Konfiguriranje postavki oblaka za svoj uređaj. U ovom ćete koraku će dovršili konfiguraciju lokalnim uređajem, a zatim registrirali uređaj sa servisom za Upravitelj StorSimple.

    1.  Unos **ključa za registraciju** koju ste dobili u [Korak 2: Registracija ključ servisa](storsimple-ova-deploy1-portal-prep.md#step-2-get-the-service-registration-key) za StorSimple virtualne polja.

    2.  Preskočite ovaj korak ako je ovo prvi uređaj registrirate za taj servis, a zatim prijeđite na sljedeći korak. Ako to ne prvi uređaj koji se registrirate za taj servis, morat ćete unijeti **ključa za šifriranje podataka servisa**. Ovaj ključ nužan je pomoću ključa Registracija servisa da biste registrirali dodatnih uređaja sa servisom StorSimple Manager. Dodatne informacije potražite da biste dobili [ključa za šifriranje podataka servisa](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) na vašem lokalnom web-mjestu korisničkog Sučelja.

    3.  Kliknite **Registracija**. To će ponovnim pokretanjem uređaja. Možda ćete morati pričekati 2-3 minuta prije uređaj uspješno registriran. Nakon ponovnog pokretanja uređaja koje odvest će stranica za prijavu.

        ![](./media/storsimple-ova-deploy3-fs-setup/image13.png)
    

1.  Vratite se na portal Azure klasični. Na stranici **uređaja** , provjerite je li uređaj je uspješno povezan sa servisom traženjem status. Status uređaja mora biti **aktivna**.

![](./media/storsimple-ova-deploy3-fs-setup/image12.png)

## <a name="step-2-complete-the-required-device-setup"></a>Korak 2: Dovršavanje postavljanja potrebna uređaja

Da biste dovršili konfiguraciju uređaja StorSimple uređaj, morate:

-   Odaberite račun za pohranu za povezivanje s uređajem.

-   Odaberite postavke šifriranja za podatke koji se šalju u oblaku.

[Azure klasični portal](https://manage.windowsazure.com/) za dovršavanje postavljanja potrebna uređaj poduzeti sljedeće korake.

#### <a name="to-complete-the-minimum-device-setup"></a>Da biste dovršili postavljanje minimalne uređaju

1.  Na stranici **uređaja** odaberite uređaj koji ste upravo stvorili. Ovaj uređaj bi prikazivati kao **aktivna**. Kliknite strelicu prema nazivu uređaja, a zatim kliknite **Brzi početak rada**.

2.  Kliknite **Postavljanje dovršeno uređaju** da biste pokrenuli čarobnjak za konfiguriranje uređaja.

3.  U čarobnjaku za uređaj Konfiguriraj na stranici **Osnovne postavke** , učinite sljedeće:

    1.  Odredite račun za pohranu koja će se koristiti s uređajem. Možete odabrati postojeći račun za pohranu u ovu pretplatu s padajućeg popisa ili odredite **Dodaj više** da biste odabrali račun neku drugu pretplatu.

    2.  Definiranje postavki šifriranja podataka-na-ostatak (AES šifriranje) koja će se slati u oblak. Da biste šifrirali podataka, provjerite kombinirani okvir da biste **omogućili ključa za šifriranje oblaka za pohranu**. Unesite šifriranje prostora za pohranu oblaka koja sadrži 32 znaka. Ponovno je unesite ključ da biste je potvrdili. Koristit će se ključ 256-bitni AES pomoću korisnički definiranih ključa za šifriranje.

    3.  Kliknite ikonu provjeri ![](./media/storsimple-ova-deploy3-fs-setup/image15.png).

        ![](./media/storsimple-ova-deploy3-fs-setup/image16.png)

Ažurirat će se sada postavkama. Kada postavke uspješno su ažurirani, gumb postavljanje dovršeno uređaj će biti zasivljene. Vratit ćete stranicu za **Brzo pokretanje** uređaja.

 ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)


> [AZURE.NOTE]                                                              
>
> Sve ostale postavke uređaja u bilo kojem trenutku možete izmijeniti tako da pristupite stranici **Konfiguracija** .

## <a name="step-3-add-a-share"></a>Korak 3: Dodavanje zajedničke mape

[Portal za Azure klasični](https://manage.windowsazure.com/) da biste stvorili zajednički poduzeti sljedeće korake.

#### <a name="to-create-a-share"></a>Stvaranje zajedničke mape

1.  Na stranici **Brzi Start** uređaja kliknite **Dodaj na zajedničko korištenje**. Pokreće se čarobnjak za zajedničko korištenje za dodavanje.

    ![](./media/storsimple-ova-deploy3-fs-setup/image17.png)

1.  Na stranici **Osnovne postavke** učinite sljedeće:

    1.  Navedite jedinstveni naziv za svoje se zajednički koristi. Naziv mora biti niz koji sadrži znakove 3 do 127.

    2.  (Neobavezno) Unesite opis za zajedničko korištenje. Opis će olakšati prepoznavanje vlasnici zajedničko korištenje.

    3.  Odaberite vrstu korištenje za zajedničko korištenje. Vrsta korištenja može biti **Tiered** ili **lokalno prikvačene**, s tiered se zadano. Za radnih opterećenja koji zahtijevaju lokalne jamstva, malo latencies i noviji performanse, odaberite zajedničko korištenje **lokalno prikvačene** . Za sve druge podatke, odaberite zajedničko korištenje **Tiered** .

    Lokalno prikvačene zajedničko korištenje thickly dodjeli i osigurava primarni podatke na zajedničko korištenje ostaje lokalne na uređaju, a spill u oblak. Tiered zajedničko korištenje s drugim strane je thinly dodijeljeni resursi. Kada stvorite tiered zajedničko korištenje, 10% prostora je dodjeli na lokalni sloju i 90% prostora dodjeli je u oblak. Ako, primjerice, ako dodjeli 1 TB glasnoće, 100 GB bi se nalaziti u lokalnom prostora i 900 GB će se koristiti u oblak kada razine podataka. To shodno podrazumijeva da pokrenete iz lokalne prostora na uređaju, ne dodijelite tiered zajedničko korištenje.

1.  Navedite dodijeljenu kapacitet na zajedničko korištenje. Imajte na umu određenog kapaciteta mora biti manja od dostupan kapacitet. Ako koristite tiered zajedničko korištenje, zajedničko korištenje veličina mora biti između 500 GB i 20 TB. Za lokalno prikvačene zajedničko korištenje odredite veličinu zajedničko korištenje između 50 GB i 2 TB. Dodjela zajednički koristite dostupan kapacitet kao vodič. Ako je dostupan lokalne kapacitet 0 GB, pa vam neće moći će se Dodjela resursa za zajedničko korištenje lokalne ili tiered.

    ![](./media/storsimple-ova-deploy3-fs-setup/image18.png)

1.  Kliknite ikonu sa strelicom ![](./media/storsimple-ova-deploy3-fs-setup/image19.png) da biste prešli na sljedeću stranicu.

1.  Na stranici **Dodatne postavke** dodijeliti dozvole korisnika ili grupu kojoj će pristup ovu zajedničku mapu. Navedite naziv korisniku ili grupi korisnika u *<john@contoso.com>* oblik. Preporučujemo da koristite grupu korisnika (umjesto jednog korisnika) da biste omogućili administratorske ovlasti za pristup te zajedničko korištenje. Nakon što ste dodijelili dozvole ovdje, zatim koristite Windows Explorer da biste izmijenili te dozvole.

    ![](./media/storsimple-ova-deploy3-fs-setup/image20.png)

1.  Kliknite ikonu provjeri ![](./media/storsimple-ova-deploy3-fs-setup/image21.png). Zajedničke mape stvorit će se za navedeni postavke. Prema zadanim postavkama, nadzor i sigurnosno kopiranje će biti omogućen za zajedničko korištenje.

## <a name="step-4-connect-to-the-share"></a>Korak 4: Povezivanje s zajedničko korištenje

Sada ćete povezati share(s) koju ste stvorili u prethodnom koraku. Da biste izveli taj postupak na glavno računalo sustava Windows Server.

#### <a name="to-connect-to-the-share"></a>Povezivanje s zajedničko korištenje

1.  Pritisnite ![](./media/storsimple-ova-deploy3-fs-setup/image22.png) + R. U prozor Pokreni navedite u * \\ * za put zamjenjuje *naziv poslužitelja datoteke* naziv uređaja koji je dodijeljen datotečnom poslužitelju. Kliknite **u redu**.

    ![](./media/storsimple-ova-deploy3-fs-setup/image23.png)

2.  Otvorit će se Explorer. Sada trebali biste moći vidjeti zajedničke stavke koje ste stvorili kao mape. Odaberite i dvokliknite zajedničko korištenje (mapa) da biste pogledali sadržaj.

    ![](./media/storsimple-ova-deploy3-fs-setup/image24.png)

3.  Sada možete dodati datoteke te zajedničko korištenje i preuzeti sigurnosnu kopiju.

![Ikona videoprikaz:](./media/storsimple-ova-deploy3-fs-setup/video_icon.png) **videozapis dostupna**

Pogledajte videozapis da biste saznali kako možete konfigurirati i registrirati StorSimple polja za virtualne kao datotečnog poslužitelja.

> [AZURE.VIDEO configure-a-storsimple-virtual-array]

## <a name="next-steps"></a>Daljnji koraci

Saznajte kako koristiti lokalne web korisničkog Sučelja za [administraciju sustava StorSimple virtualne polja](storsimple-ova-web-ui-admin.md).
