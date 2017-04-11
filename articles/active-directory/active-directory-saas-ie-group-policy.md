<properties
    pageTitle="Kako implementirati nastavak ploča programa Access za Internet Explorer pomoću pravila grupe | Microsoft Azure"
    description="Kako koristiti pravila grupe za implementaciju dodatak preglednika Internet Explorer za portal Moje aplikacije."
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/16/2016"
    ms.author="markvi"/>

#<a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Kako implementirati nastavak ploča programa Access za Internet Explorer pomoću pravilnika grupe

Pomoću ovog praktičnog vodiča pokazuje kako daljinski instalirajte proširenje za pristup ploče za Internet Explorer na računalima vaših korisnika pomoću pravila grupe. Za Internet Explorer korisnike kojima je potrebna prijava u aplikacije konfiguriranih pomoću [operacijskim sustavom jedinstvenu prijavu](active-directory-appssoaccess-whatis.md#password-based-single-sign-on)potreban je ovaj kućni broj.

Preporučuje se da administratori automatizirati implementacije ovaj kućni broj. U suprotnom će korisnici imati da biste preuzeli i instalirali proširenje sami, koji je mogu pogreške korisnika i zahtijeva administratorske dozvole. Pomoću ovog praktičnog vodiča pokriva jedan način Automatizacija implementacijama softver pomoću pravila grupe. [Dodatne informacije o pravilima grupe.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Proširenje ploče za pristup i dostupna je za [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) i [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998), nijedno koje zahtijevaju administratorske dozvole za instalaciju.

##<a name="prerequisites"></a>Preduvjeti

- Postavite [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx), a ste se pridružili strojeva vaših korisnika na vašu domenu.
- Morate imati dozvolu "Uredi postavke" da biste mogli uređivati objekti pravilnika grupe (GPO). Prema zadanim postavkama, članovi sljedeće sigurnosnih grupa imati tu dozvolu: administratori domene, administratori tvrtke i vlasnika alat za stvaranje pravilnika grupe. [uči više.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

##<a name="step-1-create-the-distribution-point"></a>Korak 1: Stvaranje točke distribuciju.

Najprije morate postaviti instalacijski paket na mrežno mjesto kojemu je moguće pristupiti iz svih strojeva koje želite daljinski instalirajte proširenje na. Da biste to učinili, slijedite ove korake:

1. Prijavite se na poslužitelju u ulozi administratora

2. U prozoru **Upravitelj poslužitelja** , idite na **datoteka i servise za pohranu**.

    ![Otvaranje datoteke i servise za pohranu](./media/active-directory-saas-ie-group-policy/files-services.png)

3. Idite na karticu **zajedničko korištenje** . Zatim na **zadacima** > **Novi zajedničko korištenje...**

    ![Otvaranje datoteke i servise za pohranu](./media/active-directory-saas-ie-group-policy/shares.png)

4. Dovršite **Čarobnjak za novo zajedničko korištenje** i Postavi dozvole da biste bili sigurni da je moguće pristupiti iz strojeva vaših korisnika. [Saznajte više o zajedničko korištenje.](https://technet.microsoft.com/library/cc753175.aspx)

5. Preuzmite paket sljedeće Microsoft Windows Installer (.msi datoteku): [Extension.msi ploča programa Access](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access Panel Extension.msi)

6. Kopirajte instalacijski paket na željeno mjesto na zajedničko korištenje.

    ![Kopiraj .msi datoteku za koju ste zajednički koristi.](./media/active-directory-saas-ie-group-policy/copy-package.png)

8. Provjerite jesu li klijentskim računalima moći pristupiti instalacijski paket iz zajednički koristi. 

##<a name="step-2-create-the-group-policy-object"></a>Korak 2: Stvaranje objekta pravilnika grupe

1. Prijavite se na poslužitelju koji hostira vaša instalacija Active Directory Domain Services (AD DS).

2. U u upravitelju poslužitelja, odaberite **Alati** > **Upravljanje pravilnikom grupe**.

    ![Kliknite Alati > Upravljanje pravilima grupe](./media/active-directory-saas-ie-group-policy/tools-gpm.png)

3. U lijevom oknu prozora za **Upravljanje pravilnikom grupe** , pogledajte hijerarhiju organizacijsku jedinicu (OU) i odrediti u koji dosegu koju želite primijeniti pravilnik grupe. Na primjer, možda ćete htjeti odaberite small Organizacijska Jedinica za implementaciju za nekoliko korisnika za testiranje ili možda odaberite najviše razine Organizacijska Jedinica za implementaciju za cijelu tvrtku ili ustanovu.

    > [AZURE.NOTE] Želite li za stvaranje ili uređivanje vaše tvrtke ili ustanove jedinice (organizacijske jedinice), prijeđite natrag u upravitelju poslužitelja i odaberite **Alati** > **Active Directory korisnicima i računalima**.

4. Nakon što ste odabrali je Organizacijska Jedinica, desnom tipkom miša kliknite ga, a zatim je odaberite **Stvaranje objekta pravilnika GRUPE u ovoj domeni i povezati ga ovdje...**

    ![Stvorite novi objekt pravilnika GRUPE](./media/active-directory-saas-ie-group-policy/create-gpo.png)

5. U naredbeni redak **Novi objekt pravilnika GRUPE** upišite naziv za novi objekt pravilnika grupe.

    ![Naziv novi objekt pravilnika GRUPE.](./media/active-directory-saas-ie-group-policy/name-gpo.png)

6. Desnom tipkom miša kliknite objekt pravilnika grupe koji ste upravo stvorili, a zatim odaberite **Uredi**.

    ![Uređivanje novi objekt pravilnika GRUPE.](./media/active-directory-saas-ie-group-policy/edit-gpo.png)

##<a name="step-3-assign-the-installation-package"></a>Korak 3: Dodjeljivanje instalacijski paket

1. Odredite želite li za implementaciju kućni broj koji se temelji na **Konfiguracije računala** ili **Konfiguracija korisnika**. Proširenje pomoću [konfiguracije računala](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx), instalirat će se na računalu bez obzira na to koja se korisnicima prijavite ga. S druge strane, s [Konfiguracija korisnika](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx), korisnici će imati nastavak instaliran za njih bez obzira na računalu na kojem prijave.

2. U lijevom oknu prozora **Uređivač upravljanja pravila grupe** , otvorite bilo koju od sljedećih puteva mapu, ovisno o vrsti konfiguracije odaberete:
    - `Computer Configuration/Policies/Software Settings/`
    - `User Configuration/Policies/Software Settings/`

3. Desnom tipkom miša kliknite **Instalacija softvera**, a zatim odaberite **Novo** > **paketa...**

    ![Stvaranje novog paketa instalaciju softvera](./media/active-directory-saas-ie-group-policy/new-package.png)

4. Otvorite zajedničku mapu koja sadrži instalacijski paket iz [Korak 1: stvaranje točke raspodjele](#step-1-create-the-distribution-point)odaberite .msi datoteku, a zatim kliknite **Otvori**.

    > [AZURE.IMPORTANT] Ako je zajedničko korištenje nalazi se na isti poslužitelj, provjerite je li pristupate na .msi putem mreže put datoteke, a ne put lokalne datoteke.

    ![Odaberite instalacijski paket zajedničku mapu.](./media/active-directory-saas-ie-group-policy/select-package.png)

5. U naredbeni redak **Implementaciju softvera** odaberite **kome je dodijeljeno** za načina implementacije. Zatim kliknite **u redu**.

    ![Odaberite dodijeljene, a zatim kliknite u redu.](./media/active-directory-saas-ie-group-policy/deployment-method.png)

Proširenje sada implementiran na organizacijsku jedinicu koju ste odabrali. [Saznajte više o instalacija softvera pravilnika grupe.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

##<a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Korak 4: Automatsko Omogućivanje proširenja za Internet Explorer 

Uz pokrenut instalacijski program, svaki proširenja za Internet Explorer mora biti izričito omogućeno prije nego što se može koristiti. Slijedite korake u nastavku da biste omogućili proširenja ploče pristup putem pravila grupe:

1. U prozoru **Uređivač upravljanja pravila grupe** , otvorite bilo koju od sljedećih puteva, ovisno o vrsti konfiguracije koje ste odabrali u [korak 3: dodjeljivanje instalacijski paket](#step-3-assign-the-installation-package):
    - `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

2. Desnom tipkom miša kliknite **Dodatak popis**i odaberite **Uređivanje**.
    ![Uređivanje popisa dodatak.](./media/active-directory-saas-ie-group-policy/edit-add-on-list.png)

3. U prozoru **Dodatka popis** odaberite **omogućeno**. Zatim u odjeljku **Mogućnosti** kliknite **Prikaži …**.

    ![Kliknite Omogući, a zatim kliknite Pokaži...](./media/active-directory-saas-ie-group-policy/edit-add-on-list-window.png)

4. U prozoru **Pokaži sadržaj** poduzeti sljedeće korake:

    1. Za prvi stupac (polje **Naziv vrijednosti** ), kopirajte i zalijepite sljedeće ID klase:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`

    2. Za drugom stupcu (polje **vrijednost** ) unesite sljedeće vrijednosti:`1`

    3. Kliknite **u redu** da biste zatvorili prozor **Pokaži sadržaj** .

    ![Ispunjavanje vrijednosti kao što je navedeno iznad.](./media/active-directory-saas-ie-group-policy/show-contents.png)

5. Kliknite **u redu** da biste primijenili promjene i zatvorite prozor **Dodatka popisa** .

Proširenje sada mora biti omogućen za strojeva odabrane parametra OU. [Dodatne informacije o korištenju pravila grupe da biste omogućili ili onemogućili dodatke preglednika Internet Explorer.](https://technet.microsoft.com/library/dn454941.aspx)

##<a name="step-5-optional-disable-remember-password-prompt"></a>Korak 5 (neobavezno): onemogućite "Zapamti lozinku" upit

Kada korisnici prijaviti na web-mjesta pomoću proširenja ploče programa Access, Internet Explorer može prikazati sljedeći upit s pitanjem "Želite li spremiti svoju lozinku?"

![](./media/active-directory-saas-ie-group-policy/remember-password-prompt.png)

Ako želite da biste korisnicima onemogućili prikazuje upit, slijedite korake u nastavku da biste spriječili automatsko dovršavanje iz plaćanju lozinke:

1. U prozoru **Uređivač upravljanja pravila grupe** , idite na put navedena u nastavku. Imajte na umu da ova postavka konfiguracije dostupna je samo u odjeljku **Konfiguracija korisnika**.
    - `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`

2. Pronađite željenu postavku pod nazivom **uključite značajku samodovršetka za korisnička imena i lozinke u obrascima**.

    > [AZURE.NOTE] Prethodne verzije servisa Active Directory možda popis tu postavku pod nazivom **ne dopuštaju samodovršetka za spremanje lozinki**. Konfiguracija za tu postavku razlikuje se od postavku opisani u ovom ćete praktičnom vodiču.

    ![Imajte na umu da biste to potražite u odjeljku korisničke postavke.](./media/active-directory-saas-ie-group-policy/disable-auto-complete.png)

3. Desnom tipkom miša kliknite iznad postavke pa odaberite **Uređivanje**.

4. U prozoru pod naslovom **uključite značajku samodovršetka za korisnička imena i lozinke u obrascima**odaberite **onemogućene**.

    ![Odaberite Onemogući](./media/active-directory-saas-ie-group-policy/disable-passwords.png)

5. Kliknite **u redu** da biste primijenili te promjene i zatvorite prozor.

Korisnici više neće moći spremiti svoje vjerodajnice ili korištenje samodovršetka da biste pristupili prethodno spremljene vjerodajnice. Međutim, to pravilo korisnicima da biste nastavili koristiti samodovršetka za ostale vrste polja obrasca, kao što su polja za pretraživanje.

> [AZURE.WARNING] Ako je ovo pravilo omogućeno kada korisnici odlučili neke vjerodajnice, to će pravilo za pohranu *ne* poništite vjerodajnice koje već pohranjena.

##<a name="step-6-testing-the-deployment"></a>Korak 6: Testiranje implementaciju

Slijedite korake u nastavku da biste provjerili implementacije proširenje uspio:

1. Ako implementiran pomoću **Konfiguracije računala**, prijavite se u klijentskom računalu koji pripada organizacijsku jedinicu koju ste odabrali u [Korak 2: Stvaranje objekta pravilnika grupe](#step-2-create-the-group-policy-object). Ako implementiran pomoću **Konfiguracija korisnika**, provjerite je li se prijaviti kao korisnik koji pripada toj Organizacijska Jedinica.

2. Može potrajati nekoliko znak dodaci za pravilnik grupe mijenja se u potpunosti ažuriranje s ovog računala. Da biste nametnuli ažuriranja, otvorite prozor **naredbenog retka** , a zatim pokrenite sljedeću naredbu:`gpupdate /force`

3. Morat ćete ponovno pokrenuti računalo da se instalacija odvija. Bootup može potrajati znatno dulje nego uobičajeni tijekom proširenje instalira.

4. Nakon ponovnog pokretanja, otvorite **Internet Explorer**. U gornjem desnom kutu prozora kliknite **Alati** (ikona zupčanika), a zatim odaberite **Upravljanje dodacima**.

    ![Kliknite Alati > Upravljanje dodacima](./media/active-directory-saas-ie-group-policy/manage-add-ons.png)

5. U prozoru za **Upravljanje dodacima** provjerite **Proširenje ploče pristup** je instalirano i da ima li **Status** postavljen na **omogućeno**.

    ![Provjerite je li proširenje ploče pristup instalirana ni omogućena.](./media/active-directory-saas-ie-group-policy/verify-install.png)

## <a name="related-articles"></a>Povezani članci

- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
- [Program access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md)
- [Proširenje ploče programa Access za otklanjanje poteškoća za Internet Explorer](active-directory-saas-ie-troubleshooting.md)
