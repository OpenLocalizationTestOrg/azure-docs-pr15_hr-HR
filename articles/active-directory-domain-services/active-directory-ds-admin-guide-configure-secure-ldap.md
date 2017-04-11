<properties
    pageTitle="Konfiguriranje sigurne LDAP (LDAPS) u Azure AD domenske servise | Microsoft Azure"
    description="Konfiguriranje sigurne LDAP (LDAPS) za domenu upravljanih Azure servisa Active Directory Domain Services"
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
    ms.date="09/21/2016"
    ms.author="maheshu"/>

# <a name="configure-secure-ldap-ldaps-for-an-azure-ad-domain-services-managed-domain"></a>Konfiguriranje sigurne LDAP (LDAPS) za domenu upravljanih Azure AD domenske servise
U ovom se članku prikazuje kako možete omogućiti sigurne Lightweight Directory Access Protocol (LDAPS) za svoju domenu upravljanih Azure servisa Active Directory Domain Services. Sigurna LDAP se zove "Lightweight Directory Access Protocol (LDAP) putem Secure Sockets Layer (SSL) / prijenosa sloja sigurnosti (TLS)".

## <a name="before-you-begin"></a>Prije početka
Da biste obavljali zadatke navedene u ovom članku, morate:

1. Valjani **Azure pretplate**.

2. U **imeniku Azure AD** - ili sinkroniziraju s lokalnog imenika ili samo oblak direktorija.

3. **Azure servisa Active Directory Domain Services** mora biti omogućen za Azure AD direktorija. Ako niste učinili, slijedite sve zadatke navedene u [Vodič za početak rada](./active-directory-ds-getting-started.md).

4. **Certifikat će se koristiti za omogućivanje sigurnog LDAP**.
    - **Preporučeno** - certifikat možete dobiti od tvrtke CA ili javne ustanove za izdavanje certifikata. Sigurnije je ta mogućnost konfiguracija.
    - Umjesto toga možete i odabrati da biste [stvorili samopotpisani certifikat](#task-1---obtain-a-certificate-for-secure-ldap) kao što je prikazano u nastavku ovog članka.

<br>

### <a name="requirements-for-the-secure-ldap-certificate"></a>Preduvjeti za sigurnu LDAP certifikata
Prije omogućivanja sigurne LDAP: D5 valjani certifikat po sljedećih smjernica. Ako pokušate Omogućivanje sigurnog LDAP za upravljane domene nije valjan netočan potvrdom ćete naići na pogreške.

1. **Pouzdanih izdavača** - za izdavanje certifikata vjerovati računala koja morate se povezati s domenom pomoću sigurne LDAP izdao certifikat. U ovom za izdavanje certifikata možda vaše tvrtke ili ustanove enterprise izdavanje ili javne ustanove za izdavanje certifikata vjerovati ta računala.

2. **Vijek trajanja** - potvrda moraju biti valjane za barem sljedećih 3 6 mjeseci. Nadziranje sigurnog pristupa LDAP upravljanih domene je prekinuto kada istekne certifikata.

3. **Predmetni naziv** - predmetni naziv na certifikat mora biti zamjenski znak za upravljane domene. Na primjer, ako domene pod nazivom "contoso100.com", potvrde predmetni naziv mora biti "*. contoso100.com". Postavite DNS naziv (predmet zamjensko ime) naziv zamjenskih znakova.

3. **Korištenje ključa** - certifikat mora biti konfigurirana za sljedeće koristi - digitalni potpisi i šifriranje ključa.

4. **Namjena certifikata** - potvrda moraju biti valjane za provjeru autentičnosti poslužitelja SSL.

<br>

## <a name="task-1---obtain-a-certificate-for-secure-ldap"></a>Zadatak 1 – Dobivanje certifikata za sigurnu LDAP
Prvi zadatak uključuje stjecanje certifikat koji je korišten za sigurnog pristupa LDAP upravljanih domene. Imate dvije mogućnosti:

- Certifikat možete dobiti od ustanove za izdavanje certifikata. Na izdavanje možda vaše tvrtke ili ustanove enterprise CA ili javna ustanova za izdavanje certifikata.

- Stvaranje samopotpisane potvrde.


### <a name="option-a-recommended---obtain-a-secure-ldap-certificate-from-a-certification-authority"></a>Mogućnost (preporučeno) – nabavite sigurne LDAP certifikat od ustanove za izdavanje certifikata
Ako vaša tvrtka ili ustanova uvodi se enterprise Infrastruktura javnog ključa (PKI), morate dobiti potvrdu od enterprise izdavanje certifikata (CA) za tvrtku ili ustanovu. Ako vaša tvrtka ili ustanova dohvaća njegov potvrde iz javne ustanove za izdavanje certifikata, morate dobiti sigurne LDAP certifikat iz tog javna ustanova za izdavanje certifikata.

Kada se zatraži potvrda, provjerite je li prođete preduvjete navedene u [zahtjev za sigurnu LDAP certifikat](#requirements-for-the-secure-ldap-certificate).

> [AZURE.NOTE] Klijentskim računalima koje je potrebno da biste povezali upravljanih domenom pomoću sigurne LDAP moraju vjerovati izdavatelj certifikata LDAPS.


### <a name="option-b---create-a-self-signed-certificate-for-secure-ldap"></a>Mogućnost B – stvaranje samopotpisanog certifikata za sigurnu LDAP
Možete stvoriti samopotpisani certifikat za sigurnu LDAP ako:

- certifikati u vašoj tvrtki ili ustanovi nije izdala ustanove za izdavanje enterprise ili
- ne namjeravate koristiti certifikat javna ustanova za izdavanje certifikata.

**Stvaranje samopotpisanog certifikata pomoću komponente PowerShell**

Na računalu sa sustavom Windows otvorite novi prozor PowerShell kao **Administrator** i upišite sljedeće naredbe, da biste stvorili novi samopotpisani certifikat.

    $lifetime=Get-Date

    New-SelfSignedCertificate -Subject *.contoso100.com -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment -Type SSLServerAuthentication -DnsName *.contoso100.com

U prethodnom primjeru, zamijenite "contoso100.com" s nazivom domene DNS domene upravljanih Azure servisa Active Directory Domain Services.

![Odabir Azure AD direktorija](./media/active-directory-domain-services-admin-guide/secure-ldap-powershell-create-self-signed-cert.png)

Novostvoreni samopotpisani certifikat se smješta u spremištu certifikat na lokalnom računalu.


## <a name="task-2---export-the-secure-ldap-certificate-to-a-pfx-file"></a>Zadatak 2 - izvoz sigurne LDAP certifikata na web. PFX datoteka
Prije pokretanja ovog zadatka, provjerite je li ste nabavili sigurne LDAP certifikat iz vaše tvrtke izdavanje ili javna ustanova za izdavanje certifikata ili ste stvorili samopotpisani certifikat.

Izvršite sljedeće korake da biste izvezli LDAPS certifikata radi na. Datoteka PFX.

1. Pritisnite gumb **Start** , a zatim unesite **R**. U dijaloški okvir **Pokreni** upišite **blog** , a zatim kliknite **u redu**.

    ![Pokrenite konzolu za BLOG](./media/active-directory-domain-services-admin-guide/secure-ldap-start-run.png)

2. Na upit **Kontrola korisničkih računa** , kliknite **da** da biste pokrenuli BLOG (Microsoft Management Console) kao administrator.

3. Na izborniku **datoteka** kliknite **Dodaj/Ukloni dodatni-u programu …**.

    ![Dodavanje dodatka konzole za BLOG](./media/active-directory-domain-services-admin-guide/secure-ldap-add-snapin.png)

4. U dijaloškom okviru **Dodavanje ili uklanjanje dodaci** odaberite dodatak za **potvrde** pa kliknite u **Dodaj >** gumb.

    ![Dodavanje dodatka certifikati konzole za BLOG](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin.png)

5. U čarobnjaku za **potvrde dodatak** odaberite **račun na računalu** , a zatim kliknite **Dalje**.

    ![Dodavanje dodatka certifikata za račun na računalu](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-computer-account.png)

6. Na stranici **Odaberite računalu** odaberite **lokalno računalo: (računalo konzole radi)** i kliknite **Završi**.

    ![Dodavanje potvrde dodatak – Odabir računala](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-local-computer.png)

7. U dijaloškom okviru **Dodavanje ili uklanjanje dodataka** kliknite **u redu** da biste dodali dodatak za potvrde blog.

    ![Dodavanje dodatka certifikati blog – gotovo](./media/active-directory-domain-services-admin-guide/secure-ldap-add-certificates-snapin-done.png)

8. U prozoru za BLOG, kliknite da biste proširili **Korijen konzole**. Trebali biste vidjeti potvrde dodatak učitati. Kliknite **certifikati (lokalnom računalu)** da biste proširili. Kliknite da biste proširili **osobne** čvor, nakon čega slijedi čvor **certifikata** .

    ![Otvaranje osobnih certifikata spremišta](./media/active-directory-domain-services-admin-guide/secure-ldap-open-personal-store.png)

9. Trebali biste vidjeti samopotpisani certifikat koju smo stvorili. Možete provjeriti svojstva potvrde da biste bili sigurni u otisak prsta odgovara prijavili u sustavu PowerShell windows stvaranja certifikata.

10. Potvrdite okvir samopotpisani certifikat i **desnom tipkom miša kliknite**. Na izborniku klikom desne tipke miša odaberite **Svi zadaci** , a zatim **Izvoz...**.

    ![Izvoz certifikata](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert.png)

11. U **Čarobnjak za izvoz certifikata**, kliknite **Dalje**.

    ![Čarobnjak za izvoz certifikata](./media/active-directory-domain-services-admin-guide/secure-ldap-export-cert-wizard.png)

12. Na stranici **Izvoz privatni ključ** , odaberite **Da, izvezi privatni ključ**, a zatim kliknite **Dalje**.

    ![Izvoz certifikata privatni ključ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-private-key.png)

    > [AZURE.WARNING] MORATE izvesti privatni ključ uz certifikata. Ako unesete PFX koje sadrže privatni ključ za potvrdu, omogućivanje sigurnog LDAP za svoju domenu upravljanih prekida.

13. Na stranici **Oblik datoteke za izvoz** odaberite **Razmjena osobnih podataka - PKCS #12 (. PFX)** obliku datoteke izvezene certifikata.

    ![Oblik datoteke certifikat za izvoz](./media/active-directory-domain-services-admin-guide/secure-ldap-export-to-pfx.png)

    > [AZURE.NOTE] Samo na. Oblik datoteke PFX podržan. Izvoz certifikata radi na. Oblik datoteke CER.

14. Na stranici **Sigurnost** odaberite mogućnost **Lozinka** i upišite lozinku radi zaštite u. Datoteka PFX. Zapamti lozinku jer ona na sljedeći zadatak potrebna. Kliknite **Dalje** da biste nastavili.

    ![Lozinka za izvoz certifikata ](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-password.png)

    > [AZURE.NOTE] Zabilježite lozinku. Vam je potrebna tijekom omogućivanja sigurne LDAP za tu upravljanih domenu u [zadatka 3 – Omogućivanje sigurnog LDAP upravljanih domene](#task-3---enable-secure-ldap-for-the-managed-domain)

15. Na stranici **datoteka za izvoz** , navedite naziv i mjesto gdje biste željeli izvoz certifikata.

    ![Put za izvoz certifikata](./media/active-directory-domain-services-admin-guide/secure-ldap-export-select-path.png)

16. Na sljedećoj stranici kliknite **Završi** da biste izvezli certifikat PFX datoteku. Dijaloški okvir za potvrdu trebali biste vidjeti kada izvezeni certifikata.

    ![Izvoz certifikata gotovo](./media/active-directory-domain-services-admin-guide/secure-ldap-exported-as-pfx.png)


## <a name="task-3---enable-secure-ldap-for-the-managed-domain"></a>Zadatak 3 – Omogućivanje sigurnog LDAP upravljanih domene
Da biste omogućili sigurne LDAP poduzeti sljedeće korake konfiguracije:

1. Dođite do **[Azure klasični portal](https://manage.windowsazure.com)**.

2. Odaberite čvor **Servisa Active Directory** u lijevom oknu.

3. Odaberite Azure AD imenik (također se nazivaju "klijent"), na kojem su omogućene Azure servisa Active Directory Domain Services.

    ![Odabir Azure AD direktorija](./media/active-directory-domain-services-getting-started/select-aad-directory.png)

4. Kliknite karticu **Konfiguracija** .

    ![Konfiguriranje kartica direktorija](./media/active-directory-domain-services-getting-started/configure-tab.png)

5. Pomaknite se prema dolje do odjeljka pod naslovom **domenske servise**. Trebali biste vidjeti mogućnost pod naslovom **Sigurne LDAP (LDAPS)** , kao što je prikazano u sljedećim snimku zaslona:

    ![Sekciji konfiguracija servisa domene](./media/active-directory-domain-services-admin-guide/secure-ldap-start.png)

6. Kliknite gumb **certifikat Konfiguriraj...** da biste otvorili dijaloški okvir **Konfiguriranje certifikata za sigurnu LDAP** .

    ![Konfiguriranje certifikata za sigurnu LDAP](./media/active-directory-domain-services-admin-guide/secure-ldap-configure-cert-page.png)

7. Kliknite ikonu mape ispod **Certifikat PFX datoteka s** da biste odredili PFX datoteke koja sadrži certifikat koji želite koristiti za sigurnog pristupa LDAP upravljanih domene. Također, unesite lozinku koju ste naveli prilikom izvoza potvrde PFX datoteku. Zatim kliknite gotovo gumb pri dnu.

    ![Navedite sigurne LDAP PFX datoteke i lozinku](./media/active-directory-domain-services-admin-guide/secure-ldap-specify-pfx.png)

8. U odjeljku **domenske servise** karticu **Konfiguracija** trebali biste dobiti zasivljena, a nalaze se u na **na čekanju...** stanja za nekoliko minuta. Tijekom tog razdoblja certifikata LDAPS je potvrđena točnosti i sigurne LDAP je konfiguriran za upravljane domene.

    ![Sigurne LDAP – na čekanju stanja](./media/active-directory-domain-services-admin-guide/secure-ldap-pending-state.png)

    > [AZURE.NOTE] Da biste omogućili sigurne LDAP za svoju domenu upravljanih traje oko 10 do 15 minuta. Ako navedeni sigurne LDAP certifikat odgovaraju potrebni kriterij, sigurne LDAP nisu omogućeni u direktoriju i vidjeti pogreške. Ako, na primjer, naziv domene nije valjana, certifikat je istekla ili itd uskoro istječe.

9. Kada sigurne LDAP uspješno je omogućen za upravljane domene, na **čeka...** poruka trebala nestati. Trebali biste vidjeti otisak prsta certifikat koji se prikazuju.

    ![Sigurna LDAP omogućeno](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled.png)

<br>


## <a name="task-4---enable-secure-ldap-access-over-the-internet"></a>Zadatak 4 - Omogućivanje sigurnog pristupa LDAP putem Interneta
**Neobavezni zadatka** – Ako planirate pristupiti upravljanih domene LDAPS putem Interneta, preskočite ovaj zadatak konfiguracije.

Prije početka ovog zadatka, provjerite je li dovršite korake navedene u [3 zadatka](#task-3---enable-secure-ldap-for-the-managed-domain).

1. Vidjet ćete mogućnost **Omogući SIGURNE LDAP PRISTUP PUTEM INTERNET** u odjeljku **domenske servise** na stranici **Konfiguracija** . Ta mogućnost postavljeno na **ne** po zadanom jer Internetom upravljanih domenu putem sigurne LDAP onemogućen je prema zadanim postavkama.

    ![Sigurna LDAP omogućeno](./media/active-directory-domain-services-admin-guide/secure-ldap-enabled2.png)

2. Uključivanje i isključivanje **OMOGUĆIVANJE pristupa PUTEM SIGURNE LDAP na INTERNETU** na **da**. Kliknite gumb **SPREMI** na dnu ploče.
    ![Sigurna LDAP omogućeno](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access.png)

3. U odjeljku **domenske servise** karticu **Konfiguracija** trebali biste dobiti zasivljena, a nalaze se u na **na čekanju...** stanja za nekoliko minuta. Nakon nekog vremena omogućen pristup Internetu upravljanih domene putem sigurne LDAP.

    ![Sigurne LDAP – na čekanju stanja](./media/active-directory-domain-services-admin-guide/secure-ldap-enable-internet-access-pending-state.png)

    > [AZURE.NOTE] Da biste omogućili pristup Internetu putem sigurne LDAP za svoju domenu upravljanih traje 10 minuta.

4. Kada uspješno je omogućen sigurnog pristupa LDAP upravljanih domene putem Interneta, na **čeka...** poruka trebala nestati. Trebali biste vidjeti vanjski IP adresa koji se mogu koristiti za pristup direktoriju putem LDAPS u polju **VANJSKI IP adresa za LDAPS PRISTUP**.

    ![Sigurna LDAP omogućeno](./media/active-directory-domain-services-admin-guide/secure-ldap-internet-access-enabled.png)

<br>

## <a name="task-5---configure-dns-to-access-the-managed-domain-from-the-internet"></a>Zadatak 5 - konfiguriranje DNS-a da biste pristupili upravljanih domene s Interneta
**Neobavezni zadatak** – Ako planirate pristupiti upravljanih domene LDAPS putem Interneta, preskočite ovaj zadatak konfiguracije.

Prije početka ovog zadatka, provjerite je li dovršite korake navedene u [4 zadatak](#task-4---enable-secure-ldap-access-over-the-internet).

Nakon što ste omogućili sigurnog pristupa LDAP putem Interneta za upravljane domene, morate ažurirati DNS tako da klijentskim računalima mogu pronaći ovu upravljanih domenu. Na kraju zadatka 4 vanjske IP adrese se prikazuje na kartici **Konfiguriraj** u **VANJSKI IP adresa za LDAPS PRISTUP**.

Vanjskog davatelja usluga DNS konfigurirati tako da se naziv DNS upravljanih domene (primjerice, "contoso100.com") upućuje na vanjske IP adrese. U našem primjeru smo potrebnih za stvaranje DNS stavku sljedeće:

    contoso100.com  -> 52.165.38.113

To je – sada ste spremni za povezivanje s upravljanih domene sigurne LDAP putem Interneta.

> [AZURE.WARNING] Imajte na umu klijentskim računalima moraju vjerovati izdavatelj certifikata LDAPS da biste mogli uspješno povezati upravljanih domene pomoću LDAPS. Ako koristite ustanove za izdavanje enterprise ili javno pouzdana ustanova za izdavanje certifikata, ne morate ništa učiniti jer klijentskim računalima pouzdanost tih kreditnih certifikata. Ako koristite samopotpisani certifikat, morate instalirati javno dio samopotpisani certifikat u spremištu pouzdanih certifikata na klijentskom računalu.

<br>

## <a name="related-content"></a>Srodni sadržaji

- [Administriranje domene upravljanih programa Azure servisa Active Directory Domain Services](active-directory-ds-admin-guide-administer-domain.md)
