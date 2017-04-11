<properties
   pageTitle="Azure AD Connect: Preduvjeti i hardver | Microsoft Azure"
   description="U ovoj se temi opisuju stara requisites i hardverski preduvjeti za Azure AD Connect"
   services="active-directory"
   documentationCenter=""
   authors="andkjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/12/2016"
   ms.author="billmath"/>

# <a name="prerequisites-for-azure-ad-connect"></a>Preduvjeti za Azure AD povezivanje
U ovoj se temi opisuju stara requisites i hardverski preduvjeti za Azure AD Connect.

## <a name="before-you-install-azure-ad-connect"></a>Prije instalacije Azure AD Connect
Prije instalacije Azure AD Connect, postoji nekoliko stvari koje su vam potrebne.

### <a name="azure-ad"></a>Azure AD
- Azure pretplatu ili [Azure probnu pretplatu](https://azure.microsoft.com/pricing/free-trial/). To je samo potrebne za pristup portalu Azure, a ne za korištenje Azure AD Connect. Ako koristite PowerShell ili Office 365 ne morate Azure pretplate da biste koristili Azure AD Connect. Ako imate licencu za Office 365 možete koristiti i portal sustava Office 365. Uz licencu za Office 365 plaćenu možete i dohvatiti na portalu Azure s portala Office 365.
    - Možete koristiti i funkciju pretpregleda Azure AD [Azure portal](https://portal.azure.com). Ovaj portal zahtijevaju Azure licencu.
- [Dodavanje i provjeru domene](active-directory-add-domain.md) namjeravate koristiti u Azure AD. Ako, na primjer, ako planirate koristiti contoso.com za korisnike, a zatim provjerite je li potvrdio tu domenu, a ne samo koristite zadanu domenu contoso.onmicrosoft.com.
- Klijent za Azure AD omogućuje zadani 50k objekte. Nakon potvrde domene, ograničenje povećava se na 300 k objekte. Ako trebate još više objekata u Azure AD morate otvoriti slučaja podršku da bi ograničenje povećati dodatno paran. Ako je potrebno više od 500 k objekata morat ćete licencu, kao što je Office 365, Azure AD osnovni, Azure AD Premium ili paket mobilnost za tvrtke.

### <a name="prepare-your-on-premises-data"></a>Priprema lokalnih podataka
- Pregledajte [značajke neobavezno sinkronizacije u Azure AD možete omogućiti](active-directory-aadconnectsyncservice-features.md) i procjenu značajkama koje trebate omogućiti.

### <a name="on-premises-servers-and-environment"></a>Lokalne poslužitelje i okruženja
- AD sheme verzija i skup stabala funkcionalna razina mora biti Windows Server 2003 ili noviji. Kontrolera domena možete pokrenuti bilo koja verzija pod uvjetom da se zadovolje uvjeti razine sheme i skup stabala.
- Ako namjeravate koristiti značajku **upisima lozinku** kontrolera domena mora biti Windows Server 2008 (s najnovijim SP) ili noviji. Ako vaš prevođenja 2008 (pre R2), a zatim morate zatvoriti i [hitni popravak KB2386717](http://support.microsoft.com/kb/2386717).
- Kontrolerom domene koriste Azure AD mora biti snimanje. Nije podržana za korištenje RODC (kontroler domene samo za čitanje) i Azure AD Connect slijedite neki preusmjeravanja za pisanje.
- Azure AD Connect nije moguće instalirati na Small Business Server ili Windows Server Essentials. Poslužitelj morate koristiti Windows Server standard ili bolje.
- Poslužitelj za Azure AD Connect mora imati cijeli GUI instalirali. Nije podržana za instalaciju na poslužitelju core.
- Azure AD Connect mora biti instaliran Windows Server 2008 ili novijem. Ovaj poslužitelj možda kontrolera domene ili poslužitelju članu ako koristite eksplicitnih postavke. Ako koristite prilagođene postavke, poslužitelj može biti samostalni i ne morate biti član domene.
- Ako instalirate Azure AD Connect Windows Server 2008, provjerite je li da biste primijenili najnovije hitnih sa servisa Windows Update. Instalacija je možete pokrenuti s poslužiteljem za unpatched.
- Ako namjeravate koristiti značajku **sinkronizaciju lozinke**poslužitelj za Azure AD Connect mora biti na Windows Server 2008 R2 SP1 ili noviji.
- Poslužitelj za Azure AD Connect mora imati [.NET Framework 4.5.1](#component-prerequisites) ili noviji i [Microsoft PowerShell 3.0](#component-prerequisites) ili noviju verziju.
- Ako se uvodi Active Directory Federation Services, poslužiteljima na kojima su instalirani AD FS ili proxy poslužitelj aplikacije Web mora biti Windows Server 2012 R2 ili novijim. [Daljinsko upravljanje Windows](#windows-remote-management) mora biti omogućena na sljedećim poslužiteljima za daljinsku instalaciju.
- Ako se uvodi Active Directory Federation Services, morat ćete [SSL certifikata](#ssl-certificate-requirements).
- Ako se uvodi Active Directory Federation Services, morate konfigurirati [razlučivanje naziva](#name-resolution-for-federation-servers).
- Azure AD Connect zahtijeva baze podataka SQL Server da biste pohranili podatke identitet. Po zadanom je instalirana na SQL Server 2012 Express LocalDB (osnovnu verziju sustava SQL Server Express) i stvaranja računa servisa za servis na lokalnom računalu. SQL Server Express je ograničenje veličine 10GB koji omogućuje upravljanje približno 100 000 objektima. Ako vam je potrebna upravljanje veću količinu direktorija objekata, morate je čarobnjak za instalaciju na druge instalacije sustava SQL Server.
- Ako koristite odvojene SQL Server, tim preduvjetima Primijeni:
    - Azure AD Connect podržava sve flavors sustava Microsoft SQL Server iz sustava SQL Server 2008 (s SP4) da biste 2014 SQL Server. Baza podataka Microsoft Azure SQL je **nisu podržane** u obliku baze podataka.
    - Morate koristiti velika i mala slova SQL razvrstavanje. One koji su povezani s na \_CI_ u njegovo ime. Je **nije podržana** za korištenje velika i mala slova razvrstavanje otkrije \_CS_ u njegovo ime.
    - Možete imati samo jedan modula za sinkroniziranje po instanca baze podataka. Preporučuje se **nije podržana** za zajedničko korištenje instanca baze podataka s FIM-MIM sinkronizacije, DirSync ili Azure AD sinkronizaciju.

### <a name="accounts"></a>Poslovni subjekti
- Azure AD globalni Administrator račun za Azure AD imenik želite integrirati s. Mora biti **računa obrazovne ustanove ili tvrtke ili ustanove** i ne može biti **Microsoftova računa**.
- Ako koristite eksplicitnih postavke ili nadogradili DirSync, morate imati račun Administrator u tvrtki za lokalni Active Directory.
- [Računi u servisu Active Directory](./connect/active-directory-aadconnect-accounts-permissions.md) ako koristite put prilagođene postavke instalacije.

### <a name="azure-ad-connect-server-configuration"></a>Konfiguraciju poslužitelja za Azure AD Connect
- Ako vaš globalni administratori imaju MFA omogućena, URL **https://secure.aadcdn.microsoftonline-p.com** mora biti na popisu pouzdanih web-mjesta. Morat ćete Dodaj na popis pouzdanih web-mjesta ako je dodan prije zatraži test za MFA. Koristite Internet Explorer da biste ga dodali pouzdanih web-mjesta.

### <a name="connectivity"></a>Povezivanje
- Poslužitelj za Azure AD Connect treba Razrješavanje DNS za intranet i internet. DNS poslužitelj moraju imati mogućnost za razrješavanje imena i lokalni Active Directory, kao i krajnje točke Azure AD.
- Ako imate vatrozid na intranetu, a morate otvoriti priključke između poslužitelja za Azure AD Connect i vaše kontrolera domena, pogledajte [Azure AD Connect priključke](active-directory-aadconnect-ports.md) za dodatne informacije.
- Ako proxy ili vatrozid ograničenje koje je moguće pristupiti URL-ovi, zatim URL-ove navedenih u [Office 365 URL-ovi i IP adresom rasponi](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) moraju biti otvoreni.
    - Ako koristite Microsoft Cloud u Njemačka ili Microsoft Azure državne oblaka, pogledajte [Azure AD Connect sinkronizaciju servisa instance pitanja vezana uz](active-directory-aadconnect-instances.md) za URL-ove.
- Azure AD Connect je prema zadanim postavkama možete komunicirati s Azure AD pomoću TLS 1.0. To možete promijeniti tako da biste TLS 1.2 tako da slijedite korake iz [omogućite 1.2 TLS za Azure AD Connect](#enable-tls-12-for-azure-ad-connect).
- Ako koristite izlaznog proxyja za povezivanje s Internetom, sljedeća postavka u datoteci **C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config** mora biti dodan za u čarobnjaku za instalaciju i Azure AD Connect sinkronizaciju da biste se mogli povezati s Internetom i Azure AD. Ovaj tekst mora se unijeti pri dnu datoteku. U kodu, &lt;PROXYADRESS&gt; predstavlja stvarni proxy IP adresa ili glavno računalo naziv.

```
    <system.net>
        <defaultProxy>
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

- Ako proxy poslužitelj zahtijeva provjeru autentičnosti, [račun servisa](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) moraju nalaziti u domene i put prilagođene postavke instalacije morate koristiti da biste odredili na [račun servisa prilagođene](./connect/active-directory-aadconnect-get-started-custom.md#install-required-components). Trebat ćete i druge promjene na machine.config. Ovime promjene u machine.config Čarobnjak za instalaciju i modula za sinkroniziranje odgovarati na njih zahtjeva za provjeru autentičnosti iz proxy poslužitelj. U čarobnjaku za instalaciju svih koriste se stranica, osim stranici **Konfiguracija** na traje u korisnikove vjerodajnice. Na stranici **Konfiguracija** na kraju Čarobnjak za instalaciju kontekstu promijenio će se na [račun servisa](./connect/active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts) koji je stvoren vi. U odjeljku machine.config trebao bi izgledati ovako.

```
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy
            usesystemdefault="true"
            proxyaddress="http://<PROXYADDRESS>:<PROXYPORT>"
            bypassonlocal="true"
            />
        </defaultProxy>
    </system.net>
```

Dodatne informacije o [zadani proxy Element](https://msdn.microsoft.com/library/kd3cf2ex.aspx)potražite u članku MSDN.

Ako imate problema s povezivanjem, pročitajte članak [Otklanjanje poteškoća s povezivanjem](active-directory-aadconnect-troubleshoot-connectivity.md).

### <a name="other"></a>Drugi
- Neobavezno: Testiranje korisnički račun da biste provjerili sinkronizacije.

## <a name="component-prerequisites"></a>Preduvjeti za komponentu

### <a name="powershell-and-net-framework"></a>PowerShell i .net Framework
Azure AD Connect ovisi o Microsoft PowerShell i .NET Framework 4.5.1. Potreban je ova verzija ili novija verzija instalirana na poslužitelju. Ovisno o verziji sustava Windows Server, učinite sljedeće:

- Windows Server 2012R2
  - Prema zadanim postavkama nije instaliran Microsoft PowerShell, potreban je ne poduzmete.
  - .NET framework 4.5.1 i noviji izdanja se nude putem značajke Windows Update. Provjerite jeste li instalirali najnovija ažuriranja za Windows Server na upravljačkoj ploči.
- Windows Server 2008R2 i Windows Server 2012.
  - Najnoviju verziju sustava Microsoft PowerShell dostupna je u **Windows Management Framework 4.0**dostupne na [Microsoftov centar za preuzimanje](http://www.microsoft.com/downloads).
  - .NET framework 4.5.1 i noviji izdanja dostupne su na [Microsoftovu centru za preuzimanje](http://www.microsoft.com/downloads).
- Windows Server 2008
  - Najnoviji podržanu verziju programa PowerShell dostupna je u **sustavu Windows Management Framework 3.0**, dostupne na [Microsoftov centar za preuzimanje](http://www.microsoft.com/downloads).
 - .NET framework 4.5.1 i noviji izdanja dostupne su na [Microsoftovu centru za preuzimanje](http://www.microsoft.com/downloads).

### <a name="enable-tls-12-for-azure-ad-connect"></a>Omogućivanje TLS 1.2 za Azure AD Connect
Azure AD Connect koristi TLS 1.0 prema zadanim postavkama za šifriranje komunikaciji između poslužitelja modula za sinkroniziranje i Azure AD. To možete promijeniti tako da konfigurirate .net aplikacije da biste koristili TLS 1.2 po zadanom na poslužitelju. Dodatne informacije o TLS 1.2 pronaći ćete u [Microsoft sigurnost savjeta 2960358](https://technet.microsoft.com/security/advisory/2960358).

1. TLS 1.2 nije moguće omogućiti na poslužitelju Windows Server 2008. Potrebno je Windows Server 2008R2 ili noviji. Provjerite je li imati .net 4.5.1 popravkom instalacije za vaš operacijski sustav, pročitajte članak [Microsoft sigurnost savjeta 2960358](https://technet.microsoft.com/security/advisory/2960358). Možda će to ili novije verzije već instaliran na vašem poslužitelju.
2. Ako koristite Windows Server 2008R2, zatim provjerite TLS 1.2 omogućena. Na poslužitelju Windows Server 2012 i novijim verzijama, TLS 1.2 već mora biti omogućen.
```
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2]
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Client] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server] "DisabledByDefault"=dword:00000000 "Enabled"=dword:00000001
```
3. Za sve operacijske sustave, postavite ovog ključa registra i ponovno pokrenuti poslužitelj.
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319
"SchUseStrongCrypto"=dword:00000001
```
4. Ako želite omogućiti TLS 1.2 između poslužitelja modul sinkronizaciju i udaljeni poslužitelj SQL, zatim provjerite imate potrebne verzija instalirana za [TLS 1.2 podrške za Microsoft SQL Server](https://support.microsoft.com/kb/3135244).

## <a name="prerequisites-for-federation-installation-and-configuration"></a>Preduvjeti za vanjski pristup instalacije i konfiguracije

### <a name="windows-remote-management"></a>Daljinsko upravljanje sustavom Windows
Kada pomoću Azure AD Connect Implementacija servisa Active Directory Federation Services ili proxy poslužitelj aplikacije za Web, pročitajte preduvjete ispod da bi se osigurala veze i konfiguracija neće uspjeti:

- Ako je poslužitelju ciljni domene pridružili, provjerite je li Windows Remote upravlja omogućen
    - U dodatnim PSH naredba prozoru pomoću naredbe`Enable-PSRemoting –force`
- Ako je poslužitelju ciljni strojno WAP spojene preko domene, postoji nekoliko preduvjeta
    - Na ciljnom računalu (WAP računalo):
         - Provjerite je li u winrm (daljinsko upravljanje Windows / WS upravljanja) putem dodatak Servisi koji se pokreće servis
         - U dodatnim PSH naredba prozoru pomoću naredbe`Enable-PSRemoting –force`
    - Na računalu na kojemu je čarobnjak pokrenut (Ako je ciljnom računalu koje nisu domene spojene ili nepouzdanih domene):
        - U dodatnim PSH naredba prozoru pomoću naredbe`Set-Item WSMan:\localhost\Client\TrustedHosts –Value <DMZServerFQDN> -Force –Concatenate`
        - U upravitelju poslužitelja:
             - Dodavanje DMZ WAP glavnog računala resurse (Upravitelj poslužitelja -> Upravljanje... -> Dodavanje poslužitelje pomoću kartice DNS-a)
             - Kartica Upravitelj poslužitelja sve poslužitelje: desnom tipkom miša kliknite WAP poslužitelj i odaberite upravljanje kao..., unesite creds lokalne (ne domena) za strojno WAP
             - Da biste provjerili valjanost remote connectivity PSH, na kartici poslužitelji sve Upravitelj poslužitelja: desnom tipkom miša kliknite WAP poslužitelja, a zatim odaberite komponente Windows PowerShell.  Da biste bili sigurni udaljene sesije PowerShell možete uspostaviti trebali biste otvoriti udaljenu sesiju PSH.

### <a name="ssl-certificate-requirements"></a>SSL certifikat preduvjeti
**Važno:** naglašeno preporučuje se korištenje istog SSL certifikata na svim sve čvorove farmi AD FS, kao i sve web-aplikacije proxy poslužitelji.

- Certifikat mora biti u X509 certifikata.
- Možete koristiti samopotpisani certifikat na poslužitelje u okruženju Laboratorija test. Međutim, radnom okruženju, preporučujemo dobiti certifikat iz javne ustanove za Izdavanje.
    - Ako koristite javno pouzdanih potvrdu, provjerite je li certifikat koji je instaliran na svakom poslužitelju Proxy aplikacije Web pouzdanih na oba lokalni poslužitelj, a na svim poslužiteljima vanjski pristup
- Identitet certifikat mora odgovarati naziv usluge za vanjski pristup (na primjer, sts.contoso.com).
    - Identitet je ili predmet drugo ime (SAN) nastavkom od dNSName vrsta ili, ako nema stavki SAN, predmetni naziv naveden kao uobičajeni naziv.  
    - Više stavki SAN može biti prisutne u certifikata, pod uvjetom da jedan od njih odgovara nazivu servis vanjski pristup.
    - Ako namjeravate koristiti uključivanje radnom mjestu, dodatne SAN potreban je s vrijednošću **enterpriseregistration.** Slijedi sufiks korisnika Glavno ime (UPN) tvrtke ili ustanove, na primjer, **enterpriseregistration.contoso.com**.
- Certifikate koji se temelji na CryptoAPI sljedeće tipke za generiranje (CNG) i ključa za pohranu davatelji nisu podržani. To znači da morate koristiti certifikat na temelju CSP (davatelj usluga šifriranja), a ne na KSP (ključa za pohranu davatelja).
- Podržane su zamjenske certifikata.

### <a name="name-resolution-for-federation-servers"></a>Razrješavanje imena u pošti poslužitelje
- Postavite DNS zapise za AD FS vanjski pristup naziv usluge (primjerice sts.contoso.com) za intranet (internom DNS poslužitelju) i ekstranet (javno DNS-om putem registrar domena). Za intranetsku DNS zapis biti sigurni da koristite odgovora i ne CNAME zapisa. Ovo je potreban za provjeru autentičnosti sustava windows da bi ispravno funkcionirala s računala spojene domene.
- Ako su implementacija više od jednog poslužitelj(i) za AD FS ili Web aplikacije Proxy poslužitelj, provjerite da ste konfigurirali raspoređivača opterećenja i DNS zapise za naziv servisa AD FS vanjski pristup (na primjer sts.contoso.com) na raspoređivača opterećenja.
- Za integriranu provjeru autentičnosti sustava windows tako da radi za preglednik aplikacije pomoću preglednika Internet Explorer na intranetu, provjerite je li dodati naziv servisa AD FS vanjski pristup (na primjer sts.contoso.com) Intranetska zona u preglednika Internet Explorer. To mogu kontrolirati putem pravilnika grupe i implementiran na svim svojim računalima domene pridružili.

## <a name="azure-ad-connect-supporting-components"></a>Azure AD Connect komponente za podršku
Slijedi popis komponenata koja se instalira Azure AD Connect na poslužitelju na kojem je instaliran Azure AD Connect. Taj je popis za osnovnu instalaciju Express.  Ako odlučite koristiti različite SQL Server na stranici Instalacija sinkronizacije servisa, zatim SQL Express LocalDB nije instaliran lokalno.

- Azure AD Connect stanja
- Usluge prijavu pomoćnika za Microsoft Online za IT profesionalce (ali nema ovisnosti na njemu instalirati)
- Uslužni programi sustava Microsoft SQL Server 2012 naredbenog retka
- Microsoft SQL Server 2012 eksplicitnih LocalDB
- Nativni klijent za Microsoft SQL Server 2012.
- Paket programa Microsoft Visual C++ 2013 ponovne distribucije

## <a name="hardware-requirements-for-azure-ad-connect"></a>Hardverski preduvjeti za Azure AD Connect
Sljedeća tablica prikazuje minimalni preduvjeti za sinkronizaciju računalo Azure AD Connect.

| Broj objekata u servisu Active Directory | PROCESOR | Memorije | Veličina tvrdi disk |
| ------------------------------------- | --- | ------ | --------------- |
| Manje od 10 000 | 1.6 GHz | 4 GB | 70 GB |
| 10 000 – 50.000 | 1.6 GHz | 4 GB | 70 GB |
| 50000 – 100,000 | 1.6 GHz | 16 GB | 100 GB |
| Za 100 000 ili više objekata je potrebna punu verziju sustava SQL Server|  |  |  |
| 100,000 – 300.000 | 1.6 GHz | 32 GB | 300 GB |
| 300.000 – 600,000 | 1.6 GHz | 32 GB | 450 GB |
| Više od 600,000 | 1.6 GHz | 32 GB | 500 GB |

Minimalni preduvjeti za računala sa sustavom AD FS ili web-aplikacije poslužiteljima je sljedeće:

- Procesor: Lišće Jezgra 1,6 GHz ili noviji
- MEMORIJE: 2GB ili noviji
- Azure VM: Konfiguriranje A2 ili noviji

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
