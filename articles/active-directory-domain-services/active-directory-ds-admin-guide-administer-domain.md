<properties
    pageTitle="Azure Active Directory Domain Services: Administriranje upravljanih domene | Microsoft Azure"
    description="Administriranje Azure Active Directory Domain Services upravljanih domene"
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

# <a name="administer-an-azure-active-directory-domain-services-managed-domain"></a>Administriranje domene upravljanih programa Azure Active Directory Domain Services
U ovom se članku objašnjava upravljati domenom upravljanog sustava Azure Active Directory (AD) domenske servise.


## <a name="before-you-begin"></a>Prije početka
Da biste obavljali zadatke navedene u ovom članku, morate:

1. Valjani **Azure pretplatu**.

2. U **imeniku Azure AD** - ili sinkroniziraju s lokalnog imenika ili samo oblak direktorija.

3. **Azure servisa Active Directory Domain Services** mora biti omogućen za Azure AD direktorija. Ako niste učinili, slijedite sve zadatke navedene u [Vodič za početak rada](./active-directory-ds-getting-started.md).

4. **Domene pridruženo virtualnog računala** s kojeg administriranje servisa Azure AD domene upravlja domene. Ako nemate kao što je virtualnog računala, slijedite sve zadatke navedene u članku pod naslovom [Uključivanje virtualnog računala za Windows upravljanih domenu](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Morate vjerodajnice za **korisnički račun koji pripadaju grupi ' AAD Kontroler administratori '** u imeniku za administriranje upravljanih domene.

<br>


## <a name="administrative-tasks-you-can-perform-on-a-managed-domain"></a>Administrativne zadatke koje možete izvršiti na upravljanih domene
Članovi grupe 'AAD Kontroler administratori' odobravaju ovlasti za upravljane domene koje omogućuju obavljati zadatke kao što su:

- Uključivanje strojeva upravljanih domenu.

- Konfiguriranje ugrađeni objekt pravilnika GRUPE za 'AADDC računala' i 'AADDC korisnike' spremnika u upravljanih domene.

- Upravljati DNS-a na upravljanih domeni.

- Stvaranje i upravljati prilagođene organizacijske jedinice (organizacijske jedinice) upravljanih domeni.

- Rast Administrativni pristup računala pridruženo upravljanih domene.


## <a name="administrative-privileges-you-do-not-have-on-a-managed-domain"></a>Administratorska prava nemate upravljanih domeni
Domene upravlja Microsoft, uključujući aktivnosti kao što su zakrpa, nadzor i, izvođenja sigurnosne kopije. Dakle, domena je zaključan prema dolje i nemate ovlasti za izvođenje određene administrativne zadatke na domeni. Nekoliko primjera zadataka koji se ne može izvesti su ispod.

- Ne dobijete domene Administrator ili Administrator u tvrtki ovlasti za upravljane domenu.

- Nije moguće proširiti shemi upravljanih domene.

- Ne možete povezati kontrolera domena za upravljane domene putem udaljene radne površine.

- Upravljani domene ne možete dodati kontrolera domena.


## <a name="task-1---provision-a-domain-joined-windows-server-virtual-machine-to-remotely-administer-the-managed-domain"></a>Zadatak 1 - dodjele virtualni stroj domene pridruženo Windows Server daljinsko administriranje upravljanih domene
Azure upravljanih domene servisa Active Directory Domain Services možete se upravlja pomoću poznatih servisa Active Directory administrativne alate kao što je Active Directory Administrativni centar (ADAC) ili AD PowerShell. Administratori za vanjske korisnike nemaju ovlasti za povezivanje s kontrolera domena upravljanih domeni putem udaljene radne površine. Stoga članovi grupe 'AAD Kontroler administratori' možete upravljati upravljanih domene daljinski pomoću AD Administrativni alati poslužitelja klijenta sustava Windows s računala sa sustavom koji je pridružen upravljanih domenu. Administrativni alati AD možete instalira u sklopu značajke neobavezno Remote Server Administracija Alati (RSAT) na Windows Server i klijentskim računalima pridruženo upravljanih domeni.

U prvi je korak da biste postavili virtualnog računala Windows Server koji je pridružen upravljanih domenu. Upute potražite u članku [Uključivanje virtualnog računala za Windows Server da biste je Azure AD domene upravlja domenskih servisa](active-directory-ds-admin-guide-join-windows-vm.md).

### <a name="remotely-administer-the-managed-domain-from-a-client-computer-for-example-windows-10"></a>Daljinsko administriranje upravljanih domene s klijentskog računala (na primjer, Windows 10)
Upute u ovom članku virtualnog računala za Windows Server da biste saznali više o administraciji AAD DS upravlja domene. Međutim, možete koristiti virtualnog računala za Windows klijenta (na primjer, Windows 10) da biste to učinili.

Možete [instalirati Remote Server Administracija Alati (RSAT)](http://social.technet.microsoft.com/wiki/contents/articles/2202.remote-server-administration-tools-rsat-for-windows-client-and-windows-server-dsforum2wiki.aspx) na klijentskom računalu virtualne Windows slijedeći upute na mreži TechNet.


## <a name="task-2---install-active-directory-administration-tools-on-the-virtual-machine"></a>Zadatak 2 – Alati za administraciju instaliranje servisa Active Directory na virtualnog računala
Izvršite sljedeće korake da biste instalirali alate za Active Directory administraciju na domenu pridruženo virtualnog računala. Dodatne [informacije o instalacija i pomoću alata za administraciju Remote Server](https://technet.microsoft.com/library/hh831501.aspx)potražite u članku Technet.

1. Dođite do **virtualnim strojevima** čvor Azure klasični portalu. Odaberite virtualnog računala koje ste stvorili u zadatak 1, a zatim kliknite **Poveži** na naredbenoj traci pri dnu prozora.

    ![Povezivanje sa sustavom Windows virtualnog računala](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Klasični portal traži da otvorite ili spremite datoteku s nastavkom ".rdp", koji se koristi za povezivanje s virtualnog računala. Kliknite da biste otvorili datoteku nakon dovršetka preuzimanja.

3. Kada se zatraži prijavu pomoću vjerodajnica korisnika koji pripadaju grupi "AAD Kontroler administratori". Na primjer, koristimo 'bob@domainservicespreview.onmicrosoft.com' naš velikim slovom.

4. Na početnom zaslonu otvorite **Upravitelj poslužitelja**. Kliknite **Dodavanje uloga i značajki** u oknu središnje prozor Upravitelj poslužitelja.

    ![Pokretanje Upravitelja poslužitelja virtualnog računala](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager.png)

5. Na stranici **Prije nego počnete** **značajke čarobnjak i dodavanje uloge**kliknite **Dalje**.

    ![Prije nego počnete stranice](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-begin.png)

6. Na stranici za **Vrstu instalacije** , ostavite mogućnost **instalacije na temelju uloga ili značajkama utemeljen** potvrđen, a zatim kliknite **Dalje**.

    ![Vrsta stranica za instalaciju](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-type.png)

7. Na stranici **Odabir poslužitelja** odaberite trenutnu virtualnog računala na resurse poslužitelja, a zatim kliknite **Dalje**.

    ![Stranica za odabir poslužitelja](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-server.png)

8. Na stranici **Uloge poslužitelja** , kliknite **Dalje**. Ne možemo preskočite ove stranice jer ne možemo ne instalirate sve uloge na poslužitelju.

9. Na stranici **značajke** klikom Proširite čvor **Remote Server Tools Administracija** , a zatim proširite čvor **Alati za administraciju uloge** . Odaberite značajka **AD DS i Alati za AD polja** na popisu alata za administraciju uloge.

    ![Značajke stranice](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-ad-tools.png)

10. Na stranici za **potvrdu** kliknite **Instaliraj** Oglas i značajka Alati AD polja na virtualnog računala. Kada se instalacija značajki uspješno završi, kliknite **Zatvori** da biste zatvorili čarobnjak za **Dodavanje uloge i značajke** .

    ![Stranici za potvrdu](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-confirmation.png)


## <a name="task-3---connect-to-and-explore-the-managed-domain"></a>Zadatak 3 – povezati i istraživanje upravljanih domene
Sad kad AD Administrativni alati instalirani na domenu pridruženo virtualnog računala, koristimo te alate za istraživanje i upravljati upravljanih domene.

> [AZURE.NOTE] Morate biti član grupe 'AAD Kontroler administratori' za administriranje upravljanog domene.

1. Na početnom zaslonu kliknite **Stavku Administrativni alati**. Trebali biste vidjeti Administrativni alati AD instalirano virtualnog računala.

    ![Administrativni alati instalirana na poslužitelju](./media/active-directory-domain-services-admin-guide/install-rsat-admin-tools-installed.png)

2. Kliknite **Centar za administratora servisa Active Directory**.

    ![Administracijski centar za Active Directory](./media/active-directory-domain-services-admin-guide/adac-overview.png)

3. Da biste istražili domene kliknite naziv domene u lijevom oknu (na primjer, "contoso100.com"). Obratite pozornost na dva spremnika naziva 'AADDC računala' i "AADDC korisnike".

    ![ADAC – prikaz domene](./media/active-directory-domain-services-admin-guide/adac-domain-view.png)

4. Kliknite spremnika koji se naziva **AADDC korisnicima** da biste vidjeli sve korisnike i grupe pripadaju upravljanog domene. Trebali biste vidjeti korisničke račune i grupe iz Azure AD smjernice za prikaz prema gore u ovom spremniku. Obavijest u ovom primjeru, korisnički račun za korisnika pod nazivom "bob i grupu pod nazivom 'AAD Kontroler administratori' dostupnih u ovom spremniku.

    ![ADAC – korisnici domene](./media/active-directory-domain-services-admin-guide/adac-aaddc-users.png)

5. Kliknite spremnika koji se naziva **AADDC računala** da biste vidjeli računalima pridruženima upravljanih domenu. Trebali biste vidjeti stavku za trenutni virtualnog računala, koji se priključuje domene. Računi računala na svim računalima koji su povezani s domenom upravljanih Azure servisa Active Directory Domain Services spremaju se u ovom spremniku 'AADDC računala'.

    ![ADAC - domene pridruženo računala](./media/active-directory-domain-services-admin-guide/adac-aaddc-computers.png)

<br>

## <a name="related-content"></a>Srodni sadržaji

- [Azure servisa Active Directory Domain Services – vodič za početak rada](./active-directory-ds-getting-started.md)

- [Uključivanje virtualnog računala za Windows Server domenu upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Implementacija Alati za administraciju udaljeni poslužitelj](https://technet.microsoft.com/library/hh831501.aspx)
