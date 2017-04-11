<properties
    pageTitle="Azure Active Directory Domain Services: Uključivanje RHEL VM upravljanih domenu | Microsoft Azure"
    description="Pridruživanje crveno je vaša Enterprise Linux virtualnog računala Azure servisa Active Directory Domain Services"
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

# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Uključivanje crveno je vaša Enterprise Linux 7 virtualnog računala upravljanih domeni
U ovom se članku objašnjava uključivanje crveno je vaša Enterprise Linux (RHEL) 7 virtualnog računala s domenom upravljanog programa Azure servisa Active Directory Domain Services.

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Dodjela crveno je vaša Enterprise Linux virtualnog računala
Izvođenje sljedeće korake za dodjelu resursa na RHEL 7 virtualnog računala pomoću portala za Azure.

1. Prijavite se na [portal za Azure](https://portal.azure.com).

    ![Azure portala nadzorne ploče](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)

2. U lijevom oknu kliknite **Novo** , a unose **Je vaša crvene** trake za pretraživanje kao što je prikazano u sljedećim snimku zaslona. Stavke za crveno je vaša Enterprise Linux pojavljuju se u rezultatima pretraživanja. Kliknite **crveno je vaša Enterprise Linux 7,2**.

    ![Odaberite RHEL u rezultatima](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)

3. Rezultati pretraživanja u oknu **sve** treba popisa crveno je vaša Enterprise Linux 7,2 slike. Kliknite **crveno je vaša Enterprise Linux 7,2** da biste pogledali dodatne informacije o slika virtualnog računala.

    ![Odaberite RHEL u rezultatima](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)

4. U oknu **crveno je vaša Enterprise Linux 7,2** , trebali biste vidjeti dodatne informacije o slika virtualnog računala. Na padajućem izborniku **Odaberite model implementacije** odaberite **Klasični**. Kliknite gumb **Stvori** .

    ![Prikaz detalja o slika](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)

5. U oknu za **Stvaranje VM** unesite **Naziv glavnog računala** za nove virtualnog računala. U polje **korisničkog imena** i **lozinke**navesti lokalni administrator korisničko ime. Također možete koristiti ključa SSH za provjeru autentičnosti korisnika lokalni administrator. Odabrati i **Cijene sloju** za virtualnog računala.

    ![Stvaranje VM - osnovnih detalja](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)

6. Kliknite **neobavezno konfiguracije**. U oknu **neobavezno config** kliknite **mreže**.

    ![Stvaranje VM - konfiguriranje virtualne mreže](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-configure-vnet.png)

7. Tako ćete prikazati okno pod naslovom **mreže**. U oknu **mreže** kliknite **Virtualne mreže** da biste odabrali virtualne mreže na koju želite uvesti Linux VM. Otvorit će se okno **Virtualne mreže** . U oknu **Virtualne mreže** , odaberite mogućnost **Koristi postojeću virtualne mreže** . Zatim odaberite virtualne mreže u kojoj je dostupan Azure servisa Active Directory Domain Services. U ovom primjeru smo odaberite 'MyPreviewVNet' virtualne mreže.

    ![Stvaranje VM - odaberite virtualne mreže](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)

8. U oknu **neobavezna config** , kliknite gumb **u redu** .

    ![Stvaranje VM - virtualne mreže odabrana](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)

9. Spremni ste za stvaranje virtualnog računala. U oknu **Stvaranje VM** , kliknite gumb **Stvori** .

    ![Stvaranje VM - spreman](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm.png)

10. Uvođenje nove virtualnog računala koji se temelji na slici RHEL 7,2 trebala.

  ![Stvaranje VM - implementacije rada](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)

11. Nakon nekoliko minuta virtualnog računala mora biti implementirano uspješno i spreman za korištenje.

  ![Stvaranje VM - implementiran](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)



## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Daljinsko povezivanje s upravo dodijeljenu Linux virtualnog računala
U Azure je dodijeljena RHEL 7,2 virtualnog računala. Sljedeći zadatak je daljinsko povezivanje s virtualnog računala.

**Povezivanje s RHEL 7,2 virtualnog računala** Slijedite upute u članku [kako se prijaviti na virtualnog računala koja se izvodi Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

Ostale korake pretpostavlja da koristite PuTTY SSH klijent za povezivanje s RHEL virtualnog računala. Dodatne informacije potražite u članku [Preuzimanje PuTTY stranice](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

1. Otvorite program sustava PuTTY.

2. Unesite **Naziv glavnog računala** za novostvorenu RHEL virtualnog računala. U ovom primjeru naš virtualnog računala ima naziv glavnog računala "contoso rhel.cloudapp .net". Ako niste sigurni naziv glavnog računala na VM, odnose se na nadzornoj ploči VM portala za Azure.

    ![Povezivanje puTTY](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)

3. Prijavite se na virtualnog računala pomoću vjerodajnica za lokalni administrator koje ste naveli stvaranja virtualnog računala. U ovom se primjeru koristi lokalni administratorski račun "mahesh".

    ![PuTTY prijava](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Kliknite pločicu potrebna paketa Linux virtualnog računala
Nakon povezivanja s virtualnog računala, na sljedeći zadatak je za instalaciju paketa koji su potrebni za uključivanje domene na virtualnog računala. Poduzmite sljedeće korake:

1. **Instalacija realmd:** Paket realmd koristi se za uključivanje domene. U vašem PuTTY terminal unesite sljedeću naredbu:

    sudo yum Instaliraj realmd

    ![Instalacija realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    Nakon nekoliko minuta paket realmd trebali biste dobiti instalirati na virtualnog računala.

    ![realmd instaliran](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)

3. **Instalacija sssd:** Paket realmd ovisi o sssd za izvođenje operacija spajanja domene. U vašem PuTTY terminal unesite sljedeću naredbu:

    sudo yum Instaliraj sssd

    ![Instalacija sssd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    Nakon nekoliko minuta paket sssd trebali biste dobiti instalirati na virtualnog računala.

    ![realmd instaliran](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)

4. **Instalacija kerberos:** U vašem PuTTY terminal unesite sljedeću naredbu:

    sudo yum Instaliraj krb5-radne stanice krb5-libs

    ![Instalacija kerberos](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    Nakon nekoliko minuta paket realmd trebali biste dobiti instalirati na virtualnog računala.

    ![Kerberos instaliran](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Uključivanje virtualnog računala Linux upravljanih domeni
Sada potrebna paketa instaliranih na Linux virtualnog računala, na sljedeći zadatak je da biste se pridružili virtualnog računala upravljanih domenu.

1. Uvod u upravljane domene AAD domenske servise. U vašem PuTTY terminal unesite sljedeću naredbu:

    Uvod u lokalni sudo CONTOSO100.COM

    ![Uvod u lokalni](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    Ako je **otkriti lokalni** nije moguće pronaći upravljanih domene, provjerite je li domena dostupna iz virtualnog računala (pokušajte ping). Provjeriti da virtualnog računala uistinu implementiran isti virtualne mreže u kojoj je dostupan upravljanih domene.

2. Pokretanje kerberos. U vašem PuTTY terminal unesite sljedeću naredbu. Provjerite je li navedete korisnika koji pripada grupi "AAD Kontroler administratori". Samo ti korisnici mogu pridružiti računala upravljanih domenu.

    kinitbob@CONTOSO100.COM

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    Provjerite je li da navedete naziv domene u velika slova, još kinit neće uspjeti.

3. Priključivanje računala na domenu. U vašem PuTTY terminal unesite sljedeću naredbu. Navedite istog korisnika koje ste naveli u prethodnom koraku ('kinit').

    Uključivanje lokalni sudo – opširno CONTOSO100.COM -U'bob@CONTOSO100.COM'

    ![Lokalni spoj](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

Trebali biste dobiti poruku ("uspješno uvrštena stroj u područje") kada računalo uspješno pridruženo upravljanih domeni.


## <a name="verify-domain-join"></a>Potvrda domene spoj
Brzo možete provjeriti li na računalu je uspješno pridružen upravljanih domenu. Povezivanje s na nove domene pridruženo VM RHEL pomoću SSH i korisnički račun domene i zatim potvrdite da biste vidjeli je li korisnički račun riješen pravilno.

1. U vašem PuTTY terminal upišite sljedeću naredbu za povezivanje s na nove domene pridruženo RHEL virtualnog računala pomoću SSH. Pomoću računa za domene kojoj pripada upravljanog domene (primjerice, 'bob@CONTOSO100.COM' u tom slučaju.)

    ssh -l bob@CONTOSO100.COM contoso rhel.cloudapp.net

2. U vašem PuTTY terminal upišite sljedeću naredbu da biste vidjeli ako osnovne mape ispravno pokrenut.

    pwd

3. U vašem PuTTY terminal upišite sljedeću naredbu da biste vidjeli ako članstva u grupi su vezanog pravilno.

    ID-a

Slijedi primjer Izlaz iz te naredbe:

![Potvrda domene spoj](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)


## <a name="troubleshooting-domain-join"></a>Uključivanje domene za otklanjanje poteškoća
Potražite u članku [Uključivanje domene za otklanjanje poteškoća](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) .


## <a name="related-content"></a>Srodni sadržaji
- [Azure servisa Active Directory Domain Services – vodič za početak rada](./active-directory-ds-getting-started.md)

- [Uključivanje virtualnog računala za Windows Server domenu upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-join-windows-vm.md)

- [Kako se prijaviti na virtualnog računala koja se izvodi Linux](../virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md).

- [Instaliranje Kerberos](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)

- [Crvena je vaša Enterprise Linux 7 – vodič za integraciju sustava Windows](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
