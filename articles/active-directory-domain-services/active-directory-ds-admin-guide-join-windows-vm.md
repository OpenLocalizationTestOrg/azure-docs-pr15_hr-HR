<properties
    pageTitle="Azure Active Directory Domain Services: Uključivanje u sustavu Windows Server VM upravljanih domenu | Microsoft Azure"
    description="Pridruživanje virtualnog računala za Windows Server Azure servisa Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/02/2016"
    ms.author="maheshu"/>

# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Uključivanje virtualnog računala za Windows Server upravljanih domeni

> [AZURE.SELECTOR]
- [Azure klasični portala - Windows](active-directory-ds-admin-guide-join-windows-vm.md)
- [PowerShell – Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)

<br>

U ovom se članku objašnjava uključivanje virtualnog računala sa sustavom Windows Server 2012 R2 programa Azure servisa Active Directory Domain Services upravljanih domenu, pomoću portala za Azure klasični.


## <a name="step-1-create-the-windows-server-virtual-machine"></a>Korak 1: Stvaranje virtualnog računala za Windows Server
Slijedite upute navedene u ovom praktičnom vodiču za [Stvaranje virtualnog računala sa sustavom Windows Azure klasični portalu](../virtual-machines/virtual-machines-windows-classic-tutorial.md) . Nije važno da biste bili sigurni da to novostvoreni virtualnog računala pridruženo isti virtualne mreže omogućena Azure servisa Active Directory Domain Services. Mogućnost 'Brzo stvaranje' omogućuju vam da biste se pridružili virtualnog računala da biste virtualne mreže. Stoga ćete morati koristiti mogućnost "Iz galerije" da biste stvorili virtualnog računala.

Izvršite sljedeće korake da biste stvorili virtualni stroj Windows pridruženo virtualne mreže u kojima ste omogućili Azure servisa Active Directory Domain Services.

1. Azure klasični portalu na naredbenoj traci pri dnu prozora kliknite **Novo**.

2. U odjeljku **Izračun**, kliknite **virtualnog računala**, a zatim kliknite **Iz galerije**.

3. Prvi zaslon omogućuje **Odabir slike** za virtualnog računala s popisa dostupnih slika. Odaberite odgovarajuću sliku.

    ![Odaberite sliku](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)

4. Drugi zaslon možete odabrati naziv računala, veličina i administratora korisničko ime i lozinku. Korištenje sloju i veličine koji su potrebni za pokretanje aplikacije ili radno opterećenje. Korisničko ime koji ste odabrali je korisnik lokalni administrator na računalu. Unesite vjerodajnice na domeni korisnički račun u nastavku.

    ![Konfiguriranje virtualnog računala](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)

5. Treći zaslona možete konfigurirati resursa za povezivanje s mrežom, za pohranu i dostupnost. Provjerite je li odaberite virtualne mreže omogućena Azure servisa Active Directory Domain Services na padajućem izborniku **Regije/afinitet grupe/virtualne mreže** . Navedite **Naziv DNS servis oblaka** odgovarajuća virtualnog računala.

    ![Odaberite virtualne mreže za virtualnog računala](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

    > [AZURE.WARNING]
    Provjerite je li uključiti virtualnog računala s istom virtualne u kojima ste omogućili Azure servisa Active Directory Domain Services. Zbog toga virtualnog računala možete ' potražite u članku"domenu i obavljati zadatke kao što su uključivanju domene. Ako odaberete da biste stvorili virtualnog računala u različitim virtualne mreže, povezati virtualne mreže u kojima ste omogućili Azure servisa Active Directory Domain Services te virtualne mreže.

6. Četvrti zaslona omogućuje VM Agent mora instalirati i konfigurirati neke dostupna proširenja.

    ![Gotovo](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)

7. Nakon stvaranja virtualnog računala klasični portal navodi nove virtualnog računala u odjeljku čvor **virtualnim računalima** . Virtualnog računala i u oblaku se automatski pokrene i njihovo stanje prikazuju u obliku **pokrenut**.

    ![Virtualnog računala je s radom](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)


## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Korak 2: Povezivanje s putem lokalnog administratorskog računa virtualnog računala za Windows Server
Sada ćemo povezati na novostvoreni Windows Server virtualnog računala, da biste se pridružili domeni. Pomoću vjerodajnica za lokalni administrator koji ste naveli prilikom stvaranja virtualnog računala s njim povezati.

Izvršite sljedeće korake za povezivanje s virtualnog računala.

1. Idite na **virtualnim strojevima** čvor klasični portalu. Odaberite virtualnog računala koju ste stvorili u koraku 1, a zatim kliknite **Poveži** na naredbenoj traci pri dnu prozora.

    ![Povezivanje sa sustavom Windows virtualnog računala](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Klasični portal traži da otvorite ili spremite datoteku s nastavkom ".rdp", koji se koristi za povezivanje s virtualnog računala. Kliknite da biste otvorili datoteku nakon dovršetka preuzimanja.

3. Kada se zatraži prijava, unesite **lokalne administratorske vjerodajnice**, koje ste naveli prilikom stvaranja virtualnog računala. U ovom primjeru ste, na primjer, koristi 'localhost\mahesh'.

Sada koju treba biti prijavljeni u novostvorenu Windows virtualnog računala pomoću lokalne administratorske vjerodajnice. Sljedeći je korak da biste se pridružili virtualnog računala s domenom.


## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Korak 3: Uključivanje virtualnog računala za Windows Server na domenu upravljanog AAD DS
Izvršite sljedeće korake da biste se pridružili virtualnog računala za Windows Server na AAD DS upravljanih domenu.

1. Povezivanje s poslužiteljem Windows kao što je prikazano u koraku 2. Na početnom zaslonu otvorite **Upravitelj poslužitelja**.

2. Kliknite **Lokalnog poslužitelja** u lijevom oknu prozora upravitelja poslužitelja.

    ![Pokretanje Upravitelja poslužitelja virtualnog računala](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)

3. U odjeljku **Svojstva** kliknite **radna grupa** . Na stranici svojstava **Svojstva sustava** kliknite **Promijeni** da biste se uključili u domeni.

    ![Stranica svojstava sustava](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)

4. Navedite naziv domene servisa Azure AD domene upravlja domene u tekstni okvir **domene** i kliknite **u redu**.

    ![Odredite domene koji se spajaju](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)

5. Se od vas zatraži da unesete vjerodajnice da biste se uključili u domeni. Provjerite je li te vam **vjerodajnica za korisnika pripadaju Kontroler administratori AAD** grupe. Članovi ove grupe imati ovlasti za pridruživanje strojeva upravljanog domene.

    ![Navedite vjerodajnice za domenu spoj](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)

6. Možete navesti vjerodajnice na jedan od sljedećih načina:

    - Oblikovanje UPN: odrediti UPN nastavka za korisničkog računa, kao što je konfiguriran u Azure AD. U ovom se primjeru UPN nastavka za korisnika "teo" je 'bob@domainservicespreview.onmicrosoft.com'.

    - Oblikovanje SAMAccountName: možete navesti naziv računa u obliku SAMAccountName. U ovom primjeru korisnik "teo" će morati unijeti 'CONTOSO100\bob'.

        > [AZURE.NOTE] **Preporučujemo korištenje UPN oblikovanje da biste odredili vjerodajnice.** Na SAMAccountName može biti automatski generirani ako je prefiks UPN korisničke pretjerano dugo (na primjer, "joereallylongnameuser"). Ako više korisnici imaju isti prefiks UPN-a (na primjer, "teo") u klijentu za Azure AD, njihov oblik SAMAccountName možda generirani servis. U tim slučajevima oblik UPN-a mogu se pouzdano da biste se prijavili domeni.

7. Nakon spoj domena je uspješno, prikazat će se sljedeća poruka kojima želite dobrodošlicu domeni. Ponovno pokrenite virtualnog računala za operaciju spoj domene da biste dovršili.

    ![Dobro došli u domenu](./media/active-directory-domain-services-admin-guide/join-domain-done.png)


## <a name="troubleshooting-domain-join"></a>Uključivanje domene za otklanjanje poteškoća
### <a name="connectivity-issues"></a>Problema s povezivanjem
Ako je virtualnog računala nije moguće pronaći domenu, pogledajte sljedeće korake za otklanjanje poteškoća:

- Provjerite je li je Virtualno računalo povezano s istom virtualne u kojoj ste omogućili domenske servise u. Ako nije, virtualnog računala ne može se povezati s domenom i stoga je nije moguće uključiti domene.

- Ako virtualnog računala je povezano s drugom virtualne mreže, provjerite je li virtualne mreže povezani virtualne mreže u kojima ste omogućili domenske servise.

- Pokušajte ping domene koristi taj naziv domene upravljanih domene (primjerice, "ping contoso100.com"). Ako niste uspjeli, pokušajte pomoću naredbe ping IP adresa za domenu prikazati na stranici s kojima ste omogućili Azure servisa Active Directory Domain Services (na primjer, "pomoću naredbe ping 10.0.0.4"). Ako ste moći pomoću naredbe ping IP adrese, ali umjesto domene, DNS možda pogrešno konfigurirana. Možda ne ste konfigurirali IP adrese domene kao DNS poslužitelji za virtualne mreže.

- Pokušajte nikakvu predmemorije za razrješavanje DNS-a na virtualnog računala (' ipconfig/flushdns da biste').

Ako se pojavi dijaloški okvir sa zahtjevom za vjerodajnice da biste se pridružili domenu, nemate problema s povezivanjem.


### <a name="credentials-related-issues"></a>Probleme vezane uz vjerodajnice
Pogledajte sljedeće korake ako imate poteškoća s vjerodajnicama, a ne možete uključiti domene.

- Pokušajte koristiti oblikovanje UPN-a da biste odredili vjerodajnice. SAMAccountName za vaš račun može biti generirani ako postoji više korisnika s prefiksom isti UPN-a u klijentu ili ako je vaš prefiks UPN pretjerano dugi. Stoga ni SAMAccountName oblik za vaš račun mogu se razlikovati od što mogu očekivati ili koristiti u lokalne domene.

- Pokušajte koristiti vjerodajnice za korisnički račun koji pripada grupi "AAD Kontroler administratori" da biste se pridružili strojeva upravljanih domenu.

- Provjerite imate li [omogućena sinkronizacija lozinku](active-directory-ds-getting-started-password-sync.md) skladu korake navedene u vodiču za početak rada.

- Provjerite je li konfigurirano u Azure AD pomoću UPN-a korisnika (na primjer, 'bob@domainservicespreview.onmicrosoft.com') da biste se prijavili.

- Provjerite je li da koju ste pričekala dovoljno dugo sinkronizacije lozinku da biste dovršili kao što je navedeno u vodiču za početak rada.


## <a name="related-content"></a>Srodni sadržaji

- [Azure servisa Active Directory Domain Services – vodič za početak rada](./active-directory-ds-getting-started.md)

- [Administriranje domene upravljanih programa Azure servisa Active Directory Domain Services](./active-directory-ds-admin-guide-administer-domain.md)
