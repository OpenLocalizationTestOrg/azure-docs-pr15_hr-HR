<properties
    pageTitle="Azure Active Directory Domain Services: Upravljati DNS-a na upravljanih domena | Microsoft Azure"
    description="Upravljati DNS-a na Azure Active Directory Domain Services upravljanih domena"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="administer-dns-on-an-azure-ad-domain-services-managed-domain"></a>Upravljati DNS-a na domeni upravljanih programa Azure servisa Active Directory Domain Services
Azure Active Directory Domain Services sadrži (razlučivanje naziva domene) DNS poslužitelju koji omogućuje Razrješavanje DNS za domenu upravljanog. Ponekad ćete morati konfiguriranje DNS-a na upravljanih domeni. Možda ćete morati stvaranje DNS zapisa za strojeva koji su povezani s domenom, konfiguriranje virtualne IP adrese za učitavanje balancers ili vanjski DNS prosljeđivanja za postavljanje. Zbog toga, korisnici koji se nalaze u grupama 'AAD Kontroler administratori' odobravaju DNS administracijske ovlasti upravljanih domeni.


## <a name="before-you-begin"></a>Prije početka
Da biste obavljali zadatke navedene u ovom članku, morate:

1. Valjani **Azure pretplatu**.

2. U **imeniku Azure AD** - ili sinkroniziraju s lokalnog imenika ili samo oblak direktorija.

3. **Azure servisa Active Directory Domain Services** mora biti omogućen za Azure AD direktorija. Ako niste učinili, slijedite sve zadatke navedene u [Vodič za početak rada](./active-directory-ds-getting-started.md).

4. **Domene pridruženo virtualnog računala** s kojeg administriranje servisa Azure AD domene upravlja domene. Ako nemate kao što je virtualnog računala, slijedite sve zadatke navedene u članku pod naslovom [Uključivanje virtualnog računala za Windows upravljanih domenu](./active-directory-ds-admin-guide-join-windows-vm.md).

5. Morate vjerodajnice za **korisnički račun pripadaju grupi ' AAD Kontroler administratori '** u imeniku za administriranje DNS za svoju domenu upravljanog.

<br>

## <a name="task-1---provision-a-domain-joined-virtual-machine-to-remotely-administer-dns-for-the-managed-domain"></a>Zadatak 1 - dodjele domene pridruženo virtualnog računala daljinsko administriranje DNS za domenu upravljanog
Azure upravljanih domene servisa Active Directory Domain Services može upravljati daljinski pomoću poznatih servisa Active Directory administrativne alate kao što je Active Directory Administrativni centar (ADAC) ili AD PowerShell. Isto tako, DNS-om za domenu upravljanog može se administrirati daljinski pomoću alata za administraciju DNS poslužitelj.

Administratori u direktoriju Azure AD nemaju ovlasti za povezivanje s kontrolera domena upravljanih domeni putem udaljene radne površine. Članovi grupe 'AAD Kontroler administratori' možete upravljati DNS-a za upravljane domene daljinski pomoću alata za DNS poslužitelj s Windows Server/klijentskog računala koji je pridružen upravljanih domenu. Alati za DNS poslužitelj možete instalira u sklopu značajke neobavezno Remote Server Administracija Alati (RSAT) na Windows Server i klijentskim računalima pridruženo upravljanih domeni.

Prvi zadatak je Dodjela virtualnog računala Windows Server koji je pridružen upravljanih domenu. Upute potražite u članku [Uključivanje virtualnog računala za Windows Server da biste je Azure AD domene upravlja domenskih servisa](active-directory-ds-admin-guide-join-windows-vm.md).


## <a name="task-2---install-dns-server-tools-on-the-virtual-machine"></a>Zadatak 2 – Alati za instalaciju DNS poslužitelj na virtualnog računala
Izvršite sljedeće korake da biste instalirali alate za administraciju DNS-a na domenu pridruženo virtualnog računala. Dodatne informacije o [instaliranju i pomoću alata za administraciju udaljenog poslužitelja](https://technet.microsoft.com/library/hh831501.aspx)potražite u članku Technet.

1. Dođite do **virtualnim strojevima** čvor Azure klasični portalu. Odaberite virtualnog računala koje ste stvorili u zadatak 1, a zatim kliknite **Poveži** na naredbenoj traci pri dnu prozora.

    ![Povezivanje sa sustavom Windows virtualnog računala](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)

2. Klasični portal traži da otvorite ili spremite datoteku s nastavkom ".rdp", koji se koristi za povezivanje s virtualnog računala. Kada se dovrši preuzimanje, kliknite datoteka.

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

9. Na stranici **značajke** klikom Proširite čvor **Remote Server Tools Administracija** , a zatim proširite čvor **Alati za administraciju uloge** . Odaberite **DNS poslužitelja** značajka na popisu alata za administraciju ulogu.

    ![Značajke stranice](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-tools.png)

10. Na stranici za **potvrdu** kliknite **Instalacija** da biste instalirali alate značajku DNS poslužitelj na virtualnog računala. Kada se instalacija značajki uspješno završi, kliknite **Zatvori** da biste zatvorili čarobnjak za **Dodavanje uloge i značajke** .

    ![Stranici za potvrdu](./media/active-directory-domain-services-admin-guide/install-rsat-server-manager-add-roles-dns-confirmation.png)


## <a name="task-3---launch-the-dns-management-console-to-administer-dns"></a>Zadatak 3 – pokretanje konzole za upravljanje DNS-a da biste saznali više o administraciji DNS-a
Sad kad značajka DNS poslužitelja Alati instalirana na domenu pridruženo virtualnog računala, koristimo Alati DNS za administriranje DNS upravljanih domeni.

> [AZURE.NOTE] Morate biti član grupe 'AAD Kontroler administratori' za administriranje DNS upravljanih domeni.

1. Na početnom zaslonu kliknite **Stavku Administrativni alati**. Trebali biste vidjeti **DNS** konzolu instalirano virtualnog računala.

    ![Administrativni alati – DNS konzolu](./media/active-directory-domain-services-admin-guide/install-rsat-dns-tools-installed.png)

2. Kliknite **DNS-a** da biste pokrenuli konzole za upravljanje DNS-om.

3. U dijaloškom okviru **za povezivanje s DNS poslužitelj** kliknite željenu mogućnost pod naslovom **sljedećem računalu**pa unesite naziv domene DNS upravljanih domene (primjerice, "contoso100.com").

    ![DNS konzolu - povezivanje s domenom](./media/active-directory-domain-services-admin-guide/dns-console-connect-to-domain.png)

4. DNS konzolu povezuje upravljanih domene.

    ![DNS konzolu - administriranje domene](./media/active-directory-domain-services-admin-guide/dns-console-managed-domain.png)

5. Sada možete koristiti DNS konzolu da biste dodali unose DNS-a za računala u virtualne mreže u kojima ste omogućili AAD domenske servise.

> [AZURE.WARNING] Budite oprezni prilikom administriranje DNS za domenu upravljanih pomoću alata za administraciju DNS. Provjerite te vam **Brisanje ili izmjenu ugrađene DNS zapisa koji koriste domenske servise u domeni**. Ugrađeni DNS zapise uključuju DNS zapisima, zapisa poslužitelja naziva i ostale zapise koji se koriste za Kontroler mjesto. Ako mijenjate te zapise, domenske servise su nadziranje prekinuto na virtualne mreže.


Potražite u [članku Alati DNS-a na mreži Technet](https://technet.microsoft.com/library/cc753579.aspx) dodatne informacije o upravljanju DNS-a.


## <a name="related-content"></a>Srodni sadržaji

- [Azure servisa Active Directory Domain Services – vodič za početak rada](./active-directory-ds-getting-started.md)

- [Uključivanje virtualnog računala za Windows Server domenu upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Administriranje domene upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-administer-domain.md)

- [Alati za administraciju DNS-a](https://technet.microsoft.com/library/cc753579.aspx)
