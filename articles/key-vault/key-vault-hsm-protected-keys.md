<properties
    pageTitle="Stvaranje i prijenos zaštićenima HSM tipke za Azure ključ sigurnog | Microsoft Azure"
    description="Pomoću ovog članka planiranje, generiranje, a zatim prenijeti vlastite zaštićenima HSM tipki sa strelicama pomoću sigurnog ključ Azure."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="cabailey"/>
#<a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Kako stvoriti i prijenos HSM zaštićene ključevima za Azure ključ sigurnog

##<a name="introduction"></a>Uvod

Za dodatnu jamstvo kada koristite sigurnog ključ Azure, moguće je uvesti i generiranje ključeva u hardver sigurnost Module (HSMs) koji nikad ostavite HSM granicu. Ovaj scenarij je često se nazivaju *Premjesti vlastiti ključ*ili BYOK. Na HSMs su FIPS 140-2 razine 2 provjeriti. Azure sigurnog ključ koristi obitelj nShield Thales HSMs da biste zaštitili ključeva.

Pomoću informacija u ovoj temi plan za generiranje te prenijeti vlastite zaštićenima HSM tipki sa strelicama pomoću sigurnog ključ Azure.

Ova funkcija nije dostupna za Azure Kini. 

>[AZURE.NOTE] Dodatne informacije o Azure ključ sigurnog potražite u članku [što je sigurnog ključ Azure?](key-vault-whatis.md)  
>
>Dohvaćanje rada vodič, što obuhvaća i stvaranje ključa sigurnog zaštićenima HSM tipke, potražite u članku [Početak rada s sigurnog ključ Azure](key-vault-get-started.md).

Dodatne informacije o stvaranju i prijenos ključa HSM zaštićene putem interneta:

- Stvaranje ključa iz programa izvanmrežne radne stanice, koji smanjuje plošni napada.

- Tipku šifriran pomoću na ključ sustava Exchange ključ (KEK), koji ostaje šifrirane dok se ne prenose se u HSMs sigurnog ključ za Azure. Samo šifrirane verzija ključ ostavlja izvorne radne stanice.

- Na alata postavlja svojstva na klijentu ključ koji povezuje ključ svijeta sigurnost Azure ključ sigurnog. Pa nakon HSMs sigurnog ključ za Azure primati i dešifriranje ključ, te HSMs možete ga koristiti. Nije moguće izvesti ključa. U ovom povezivanje je postavio Thales HSMs.

- U ključ sustava Exchange ključ (KEK) koja se koristi za šifriranje ključa se generira unutar HSMs sigurnog ključ za Azure i nije moguće izvesti. Na HSMs nametnuti da može biti verzija KEK izvan na HSMs nije Očisti. Na alata obuhvaća i attestation iz Thales u KEK nije moguće izvesti, a generirana unutar originalnog HSM koji je proizveo Thales.

- Na alata obuhvaća attestation iz Thales da svijeta Azure ključ sigurnog sigurnost i generirana originalnog HSM proizveo Thales. U ovom attestation dokazuje vam Microsoft koristi originalnog hardvera.

- Microsoft koristi zasebnom KEKs i razdvajanje svijetu sigurnosti na svakom zemljopisnom području. U ovom odvojenosti osigurava da ključ mogu se koristiti samo u centre za podatke u području u kojem šifrirane ga. Ako, na primjer, ključ iz Europske klijenta ne može koristiti u centre za podatke u Sjevernoj Američka ili Azija.

##<a name="more-information-about-thales-hsms-and-microsoft-services"></a>Dodatne informacije o Thales HSMs i Microsoftove servise

Thales e-sigurnost je početne globalni davatelj usluga šifriranje podataka i cyber sigurnost rješenja za financijske usluge, Visoko tehnologije, proizvodnje, državne i tehnologije sektori. Dok je 40 godina evidentiranje zapis o zaštiti tvrtke i državne informacije, Thales rješenja koriste četiri od pet najveću energiju i aerospace tvrtke. Njihova rješenja koristi 22 NATO države i sigurne više od 80 u cent transakcija diljem svijeta plaćanja.

Microsoft surađuje s Thales da biste poboljšali stanje crteža za HSMs. Te poboljšanja omogućuju vam da biste dobili uobičajeni prednosti glavnom računalu usluge bez relinquishing kontrolu nad ključeva. Konkretno, te poboljšanja omogućuju Microsoft upravljanje u HSMs tako da ne biste morali. Kao servis u oblaku, Azure ključ sigurnog mijenja veličinu prema gore na kratku obavijest da bi odgovarao krivina korištenje vaše tvrtke ili ustanove. U isto vrijeme ključ zaštićen unutar tvrtke Microsoft HSMs: nadzor nad time ključa životni ciklus jer stvaranje ključa i prijenos HSMs tvrtke Microsoft.

##<a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Implementacijom dovode vlastiti ključ (BYOK) za Azure ključ sigurnog

Koristite sljedeće informacije i postupci ako će stvoriti vlastiti ključ HSM zaštićen, a zatim prijenos Azure ključ sigurnog – na Premjesti scenariju ključ (BYOK).


##<a name="prerequisites-for-byok"></a>Preduvjeti za BYOK

U sljedećoj su tablici popis preduvjeti za prijenos vlastiti ključ (BYOK) za Azure ključ sigurnog potražite u članku.

|Preduvjet|Dodatne informacije|
|---|---|
|Pretplatu na Azure|Da biste stvorili programa sigurnog ključ Azure, morate Azure pretplatu: [Prijava za besplatnu probnu verziju](https://azure.microsoft.com/pricing/free-trial/)|
|Podržava HSM zaštićene tipke sloju servisa Azure ključ sigurnog Premium|Dodatne informacije o razine servisa i mogućnosti za Azure ključ sigurnog potražite u članku na web-mjestu [Azure ključ sigurnog cijene](https://azure.microsoft.com/pricing/details/key-vault/) .|
|Thales HSM, smartcards i softver za podršku|Morate imati pristup Thales hardver sigurnosnom modulu i osnovni radu poznavanje Thales HSMs. Na popisu kompatibilne modela ili kupiti programa HSM ako ne morate potražite u članku [Thales hardver sigurnosnom modulu](https://www.thales-esecurity.com/msrms/buy) .|
|Sljedeći hardver i softver:<ol><li>Izvanmrežna x64 radne stanice s minimalne sistemske operacija Windows sustava Windows 7 i Thales nShield softver koji je barem verzija 11.50.<br/><br/>Ako se ove radne stanice pokreće Windows 7, morate [instalirati Microsoft .NET Framework 4,5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</li><li>Radne stanice koji je povezan s Internetom i ima minimalne sistemske operacija Windows sustava Windows 7.</li><li>USB pogonu ili neki drugi uređaj prijenosni uređaj za pohranu koji ima najmanje 16 MB slobodnog prostora.</li></ol>|Sigurnosnih vam razloga preporučujemo da prvi radne stanice nije povezano s mrežom. Međutim, ovaj preporuke ne programski nameće.<br/><br/>Imajte na umu da u upute koje slijediti ove radne stanice se nazivaju prekinuta veza radne stanice.</p></blockquote><br/>Uz to, li ključ klijenta za mrežu radnog, preporučujemo da koristite drugu, zasebnu radne stanice da biste preuzeli na alata i prijenos tipku klijenta. No za testiranje, možete koristiti isti radne stanice kao prve.<br/><br/>Imajte na umu da u uputama koji slijede ove drugi radne stanice se nazivaju radne stanice povezani s Internetom.</p></blockquote><br/>|

##<a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Stvaranje i prijenos ključ HSM sigurnog ključ za Azure

Da biste generirali i prenijeli ključ sustava sigurnog HSM ključ za Azure će koristiti sljedeće pet koraka:

- [Korak 1: Priprema vaše radne stanice povezani s Internetom](#step-1-prepare-your-internet-connected-workstation)
- [Korak 2: Priprema vaše radne stanice prekinuta veza](#step-2-prepare-your-disconnected-workstation)
- [Korak 3: Stvaranje ključa](#step-3-generate-your-key)
- [Korak 4: Priprema ključa za prijenos](#step-4-prepare-your-key-for-transfer)
- [Korak 5: Prijenos ključ sigurnog ključ Azure](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Korak 1: Priprema vaše radne stanice povezani s Internetom
Prvi korak učinite sljedeće postupke na vaše radne stanice koji je povezan s Internetom.


###<a name="step-11-install-azure-powershell"></a>Korak 1.1: Instalacija Azure komponente PowerShell

S internetskom vezom radne stanice, preuzmite i instalirajte modul Azure PowerShell koji obuhvaća cmdleti za upravljanje sigurnog ključ Azure. Potreban je minimalna verzija 0.8.13.

Upute za instalaciju potražite u članku [kako instalirati i konfigurirati Azure PowerShell](../powershell-install-configure.md).

###<a name="step-12-get-your-azure-subscription-id"></a>1.2 korak: Prijava identifikacijskog Broja za Azure pretplate

Pokretanje sesije razmjene Azure PowerShell i prijavite se na račun za Azure pomoću sljedeće naredbe:

        Add-AzureAccount
U prozoru preglednika skočni unesite račun za Azure korisničko ime i lozinku. Zatim, poslužite se naredbom [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) :

        Get-AzureSubscription
Iz izlaza, pronađite ID za pretplatu koju ćete koristiti za Azure ključ sigurnog. Kasnije ćete taj ID pretplate.

Zatvorite prozor Azure PowerShell.

###<a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>1.3 korak: Preuzimanje BYOK alata za Azure ključ sigurnog

Idite na Microsoft Download Center i [Preuzimanje alata za Azure ključ sigurnog BYOK](http://www.microsoft.com/download/details.aspx?id=45345) regiji ili instancu Azure. Da biste odredili naziv paketa za preuzimanje i njegove odgovarajuće raspršivanje SHA 256 paketa poslužite se sljedećim informacijama:

---

**Sjeverna Amerika:**

KeyVault-BYOK-Alati – United States.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

---

**Europa:**

KeyVault-BYOK-Alati – Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

---

**Azija:**

KeyVault-BYOK-Alati – AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

---

**Latinska Amerika:**

KeyVault-BYOK-Alati – LatinAmerica.zip
    
9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

---

**Japan:**

KeyVault-BYOK-Alati – Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

---

**Australija:**

KeyVault-BYOK-Alati – Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

---

[**Azure državne:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-Alati – USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

---

**Kanada:**

KeyVault-BYOK-Alati – Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

---

**Njemačka:**

KeyVault-BYOK-Alati – Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

---
**Indija:**

KeyVault-BYOK-Alati – India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

---

Da biste provjerili valjanost integritet preuzete alata BYOK, iz sesiju ljuske PowerShell Azure pomoću cmdleta [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) .

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Na alata obuhvaćaju sljedeće:

- Paket ključ ključ sustava Exchange (KEK) čiji naziv počinje s **BYOK-KEK - pkg-.**
- World sigurnosti paketa čiji naziv počinje s **BYOK-SecurityWorld - pkg-.**
- Skripta python pod nazivom v**erifykeypackage.py.**
- Imenovani **KeyTransferRemote.exe** i pridruženi DLL bibliotekama, datoteka se naredbenog retka izvršnu datoteku.
- Vizualni C++ slobodnu distribuciju paket, pod nazivom **vcredist_x64.exe.**

Kopirajte paket USB pogonu ili drugi prijenosni uređaj za pohranu.

##<a name="step-2-prepare-your-disconnected-workstation"></a>Korak 2: Priprema vaše radne stanice prekinuta veza

Drugi korak učinite sljedeće postupke na radne stanice koji nije povezan s mrežom (s Internetom ili internoj mreži).


###<a name="step-21-prepare-the-disconnected-workstation-with-thales-hsm"></a>2.1 korak: Priprema radne stanice prekinuta veza s Thales HSM

Instalacija softvera za podršku nCipher (Thales) na računalu sa sustavom Windows, a zatim priložite Thales HSM na tom računalu.

Alati za Thales provjerite jesu li u parametru path (**%nfast_home%\bin** i **%nfast_home%\python\bin**). Na primjer, upišite sljedeće:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Dodatne informacije potražite u korisničkom priručniku uključene Thales HSM.

###<a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Korak 2.2: Instaliranje alata BYOK na radne stanice prekinuta veza

Kopirajte paket BYOK alata iz USB pogon ili neki drugi prijenosni uređaj za pohranu, a zatim učinite sljedeće:

1. Izdvajanje datoteka iz preuzete paketa u bilo koju mapu.
2. Iz te mape, pokrenite vcredist_x64.exe.
3. Slijedite upute za instalaciju runtime komponente Visual C++ za Visual Studio 2013.

##<a name="step-3-generate-your-key"></a>Korak 3: Stvaranje ključa

Za taj treći korak, učinite sljedeće postupke na prekinuta veza radne stanice.

###<a name="step-31-create-a-security-world"></a>3.1 korak: Stvaranje sigurnosnih svijeta

Otvorite naredbeni redak i pokrenite program Thales novi svijeta.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Ovaj program stvara datoteku **Svijeta sigurnosti** na % NFAST_KMDATA%\local\world koji odgovara C:\ProgramData\nCipher\Key upravljanja Data\local mapu. Možete koristiti različite vrijednosti za na kvorum, ali u našem primjeru se od vas zatraži da unesete tri prazne kartice i PIN-ove za svaki od njih. Nakon toga sve dvije kartice dati puni pristup svijeta sigurnost. Na karticama postaju u **Administrator postavio kartice** za nove svijeta sigurnost.

Zatim učinite sljedeće:

- Sigurnosno kopirajte datoteku svijeta. Sigurne i zaštiti svijeta datoteka, kartica Administrator i njihove PIN-ove i provjerite ima li bez jednoj osobi pristup više kartica.

###<a name="step-32-validate-the-downloaded-package"></a>3,2 korak: Provjera valjanosti preuzete paketa

Ovaj korak nije obavezan ali preporučuje se tako da možete provjeriti sljedeće:

- Ključ sustava Exchange koji se isporučuje u na alata je generiran iz originalnog HSM Thales.
- Raspršivanje svijeta sigurnost uvrštene u na alata stvoreno je u originalni HSM Thales.
- Exchange ključ je nije moguće izvesti.

>[AZURE.NOTE]Da biste provjerili valjanost preuzeti paket, u HSM mora biti povezan, uključen, i morate imati svijeta sigurnosti na njemu (primjerice onu koju ste upravo stvorili).

Da biste provjerili valjanost preuzete paketa:

1.  Pokrenite skriptu verifykeypackage.py uz zauzimanja nešto od sljedećeg, ovisno o tome regiji ili instancu Azure:
    - U Sjevernoj Americi:

            python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
    - Europi:

            python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
    - Za Azija:

            python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
    - Za Latinska Amerika:

            python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
    - Za Japan:

            python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
    - Za Australiju:

            python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
    - Za [Državne ustanove Azure](https://azure.microsoft.com/features/gov/), koji koristi pojavljivanja vlade sad Azure:

            python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
    - Za Kanada:

            python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
    - Za Njemačka:

            python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
    - Za Indija:

            python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
    >[AZURE.TIP]Softver Thales obuhvaća python pri %NFAST_HOME%\python\bin

2.  Potvrda možete vidjeti sljedeće, što znači uspjela provjera valjanosti: **rezultat: USPJEH**

Ova skripta Provjeri valjanost lanac potpisnik najviše korijenski ključ Thales. Raspršivanje ovaj korijenski ključ uložen u skripti i njezina vrijednost mora biti **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**. Tu vrijednost možete potvrditi i zasebno web-mjestu sustava [Thales web-mjesta](http://www.thalesesec.com/).

Sada ste spremni za stvaranje novog ključa.

###<a name="step-33-create-a-new-key"></a>3,3 korak: Stvaranje novog ključa

Stvaranje ključa pomoću programa za **generatekey** Thales.

Pokrenite sljedeću naredbu da biste generirali ključ:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Kada pokrenete tu naredbu, koristite ove upute:

- Parametar *Zaštita* mora biti postavljeno na vrijednost **Modul**, kao što je prikazano. Time se stvara ključa modul zaštićen. Skup alata za BYOK ne podržava OCS zaštićene tipke.

- Zamijenite vrijednost *contosokey* za **ident** i **plainname** bilo koja vrijednost niza. Da biste minimizirali administratora folije i smanjenju rizika od pogreške, preporučujemo da koristite li iste vrijednosti za oba. Vrijednost **ident** mora sadržavati samo brojeve, crtice i slova u mala slova.

- Na pubexp ostavite prazno (zadano) u ovom primjeru, no možete odrediti određene vrijednosti. Dodatne informacije potražite u dokumentaciji Thales.

Ta se naredba stvara datoteku Tokenized ključ u mapi %NFAST_KMDATA%\local pod nazivom počevši od **key_simple_**, nakon čega slijedi **ident** koja je određena u naredbi. Na primjer: **key_simple_contosokey**. Datoteka sadrži šifrirane ključ.

Sigurnosno kopirajte Tokenized ključ datoteka na sigurnom mjestu.

>[AZURE.IMPORTANT] Kad kasnije prenijeti ključ sigurnog ključ Azure, Microsoft nije moguće izvesti tu tipku natrag tako da postane iznimno važno da stvorite sigurnosnu kopiju vašeg svijeta ključ i sigurnost sigurno. Thales zatražite upute i najbolje prakse za sigurnosno kopiranje ključa.

Sada ste spremni za prijenos ključ sigurnog ključ Azure.

##<a name="step-4-prepare-your-key-for-transfer"></a>Korak 4: Priprema ključa za prijenos

Za ovaj četvrti korak, učinite sljedeće postupke na prekinuta veza radne stanice.

###<a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>4.1 korak: Stvaranje kopije ključa s dozvolama za smanjena

Da biste smanjili dozvole na svoj ključ iz naredbenog retka, pokrenite nešto od sljedećeg, ovisno o tome regiji ili instancu Azure:

- U Sjevernoj Americi:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
- Europi:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
- Za Azija:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
- Za Latinska Amerika:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
- Za Japan:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
- Za Australiju:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
- Za [Državne ustanove Azure](https://azure.microsoft.com/features/gov/), koji koristi pojavljivanja vlade sad Azure:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
- Za Kanada:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
- Za Njemačka:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
- Za Indija:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1


Prilikom pokretanja naredbe zamijeniti *contosokey* s istom vrijednošću koju ste naveli u **3,3 korak: Stvaranje novog ključa** od koraka [Generiraj ključa](#step-3-generate-your-key) .

Što trebate priključite na karticama za sigurnost svijeta administrator.

Kada se naredba završi, prikazat **rezultat: USPJEH** , a kopija ključa s dozvolama za smanjene u datoteku pod nazivom key_xferacId_<contosokey>.

###<a name="step-42-inspect-the-new-copy-of-the-key"></a>4.2 korak: Provjera novu kopiju ključ

Po želji, pokrenite na Thales uslužni programi da biste potvrdili najmanje dozvole za novi ključ:

- aclprint.PY:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
- kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
Kada pokrenete te naredbe, zamijenite contosokey jednaku vrijednost koju ste naveli u **3,3 korak: Stvaranje novog ključa** od koraka [Generiraj ključa](#step-3-generate-your-key) .

###<a name="step-43-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Korak 4,3: Šifriranje ključa pomoću tvrtke Microsoft Exchange ključ

Pokrenite jedan od sljedećih naredbi, ovisno o tome regiji ili instancu Azure:

- U Sjevernoj Americi:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Europi:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Za Azija:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Za Latinska Amerika:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Za Japan:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Za Australiju:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Za [Državne ustanove Azure](https://azure.microsoft.com/features/gov/), koji koristi pojavljivanja vlade sad Azure:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Za Kanada:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Za Njemačka:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
- Za Indija:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Kada pokrenete tu naredbu, koristite ove upute:

- Zamjena *contosokey* identifikator koji ste koristili za stvaranje ključa u **3,3 korak: Stvaranje novog ključa** od koraka [Generiraj ključa](#step-3-generate-your-key) .

- Zamijenite *SubscriptionID* ID Azure pretplatu koja sadrži vaše ključa zbirke ključeva. Dohvaćeni tu vrijednost prethodno, u **1.2 korak: dohvaćanje identifikacijskog Broja za Azure pretplate** od koraka [Priprema vaše radne stanice povezani s Internetom](#step-1-prepare-your-internet-connected-workstation) .

- Zamijenite *ContosoFirstHSMKey* oznaka koja se koristi za naziv datoteke izlaz.

Kada se to završi uspješno, prikazuje **rezultat: USPJEH** i trenutnu mapu koja sadrži sljedeći naziv nove datoteke: TransferPackage -*ContosoFirstHSMkey*.byok

###<a name="step-44-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>4.4 korak: Kopiranje pakiranju ključa prijenos u radne stanice povezani s Internetom

Pomoću USB pogon ili neki drugi prijenosni uređaj za pohranu da biste kopirali Izlazna datoteka u prethodnom koraku (KeyTransferPackage ContosoFirstHSMkey.byok) vaše radne stanice povezani s Internetom.

##<a name="step-5-transfer-your-key-to-azure-key-vault"></a>Korak 5: Prijenos ključ sigurnog ključ Azure

Za završnom koraku na internetskom vezom radne stanice, koristite cmdlet za [Dodavanje AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx) da biste prenijeli ključa prijenosa paketa koju ste kopirali iz prekinuta veza radne stanice HSM sigurnog ključ za Azure:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Ako prijenos ne uspije, pogledajte prikazuje svojstva ključ koji ste upravo dodali.


##<a name="next-steps"></a>Daljnji koraci

Sada možete koristiti taj ključ HSM zaštićene u vašem ključa sigurnog. Dodatne informacije potražite u odjeljku **želite li koristiti modul sigurnost hardver (HSM)** u ovom praktičnom vodiču za [Početak rada s Azure ključ sigurnog](key-vault-get-started.md) .
