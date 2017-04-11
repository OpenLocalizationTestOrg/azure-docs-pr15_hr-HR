<properties
    pageTitle="Azure Active Directory Domain Services: Sinkronizacija u upravljanih domenama | Microsoft Azure"
    description="Razumijevanje sinkronizacije u domeni upravljanih programa Azure Active Directory Domain Services"
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

# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>Sinkronizacija programa Azure servisa Active Directory Domain Services upravljanih domeni
Sljedeći dijagram prikazuje kako funkcionira sinkronizacija u Azure AD domenske servise upravljanih domene.

![Topologija sinkronizacije u Azure AD domenske servise](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>Sinkronizacija iz lokalnog imenika klijentu za Azure AD
Azure AD Connect sinkronizaciju koristi se za sinkronizaciju korisničkih računa, grupirali članstva i raspršivanja vjerodajnica za vaš klijent Azure AD. Atributi korisnika računa kao što su UPN-a i lokalnog sinkroniziraju SID (identifikator sigurnost). Ako koristite Azure servisa Active Directory Domain Services, naslijeđene vjerodajnica raspršivanja potreban za provjeru autentičnosti NTLM i Kerberos također se sinkroniziraju Azure AD klijentu.

Ako konfigurirate pisanje natrag, promjene koje su se pojavile u direktoriju Azure AD sinkroniziraju se natrag u lokalni Active Directory. Na primjer, ako promijenite lozinku korištenjem značajki za Azure AD samostalno promjena promijenjene lozinku ažurira u vaše lokalne domene servisa Active Directory.

> [AZURE.NOTE] Uvijek koristite najnoviju verziju programa Azure AD Connect da biste bili sigurni da imate rješenja za sve poznate programskih pogrešaka.


## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>Sinkronizacija s klijentom Azure AD upravljanog domene
Korisničke račune, članstva u grupi i raspršivanja vjerodajnica se sinkroniziraju s klijentom Azure AD domene upravljanih Azure servisa Active Directory Domain Services. Sinkronizacija se automatski. Ne morate konfiguriranje, praćenje i upravljanje tom se postupku sinkronizacije. Sinkronizacija se one-way/unidirectional u karaktera. Upravljani domene je uglavnom samo za čitanje osim prilagođenih organizacijske jedinice stvarate. Zbog toga ne možete mijenjati korisničke atribute, korisničke lozinke ili članstva u grupi unutar upravljanog domene. Zbog toga postoji bez obrnutim sinkronizacija promjene upravljanih domene vratite se u klijentu za Azure AD.


## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>Sinkronizacija s više skupa stabala lokalnog okruženja
Mnoge organizacije imaju infrastruktura za identitet prilično složene lokalnog koji se sastoji od više šuma računa. Azure AD Connect podržava sinkronizaciju korisnike, grupe i raspršivanja vjerodajnica iz više skupa stabala okruženja u kojima se klijent sustava Azure AD.

Nasuprot tome, vaš klijent Azure AD je mnogo jednostavnijim i nehijerarhijskom prostor naziva. Da biste korisnicima omogućili da kvalitetno pristupiti aplikacijama osigurava Azure AD, riješiti sukobe UPN preko korisničkih računa u različitim šuma. Vaše medvjedići upravljanih domene Azure servisa Active Directory Domain Services zatvorite resemblance klijent sustava Azure AD. Dakle, vidjeti paušalni OU strukture u upravljanih domene. Svi korisnici i grupe su pohranjeni u kontejneru 'AADDC korisnika', bez obzira na lokalnom domenom ili skup stabala iz koje oni su sinkronizirane u. Možda ste konfigurirali hijerarhijski OU Strukturiranje lokalnog. No upravljanih domene i dalje ima jednostavnu paušalni OU strukturu.


## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>Izostavljene – što nije sinkronizirano s upravljanih domene
Sljedeće objekte ili atribute ne sinkroniziraju se klijent sustava Azure AD ili upravljanih domene:

- **Izuzeti atribute:** Možete odabrati da biste izuzeli određene atribute sinkronizaciju klijentu za Azure AD iz lokalne domene pomoću Azure AD Connect. Ti isključenih atributi nisu dostupne u upravljanih domene.

- **Grupiranje pravila:** Pravila grupe konfiguriran u lokalne domene ne sinkroniziraju upravljanih domene.

- **SYSVOL zajedničko korištenje:** Isto tako, sadržaj SYSVOL na lokalne domene ne sinkroniziraju upravljanih domene.

- **Računalne objekte:** Računalne objekte za računala pridružena lokalne domene ne sinkroniziraju upravljanih domene. Ta računala imaju odnos pouzdanosti s upravljanih domene i pripadate samo vaše lokalne domene. U upravljanih domene, samo za računala koje ste izričito domene-pridružena upravljanih domene pronađite računalne objekte.

- **SidHistory atribute za korisnike i grupe:** Primarni korisnik i primarna grupa SID-ove iz lokalne domene se sinkroniziraju upravljanog domene. Međutim, postojećih atributa SidHistory za korisnike i grupe neće se sinkronizirati s vaše lokalne domene upravljanog domene.

- **Strukture organizacije jedinice (OU):** Organizacijska jedinica definirano u lokalne domene ne sinkroniziraju upravljanih domene. Postoje dva organizacijske jedinice ugrađene u upravljanih domene. Prema zadanim postavkama upravljanog domene ima paušalni OU strukturu. No možete [stvoriti prilagođeni OU u upravljanog domene](./active-directory-ds-admin-guide-create-ou.md).


## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>Kako određene atributi sinkroniziraju upravljanih domene
U sljedećoj su tablici navedeni neki uobičajeni atributi i opisuje način na koji se sinkroniziraju upravljanog domene.

| Atribut upravljanih domene | Izvor | Bilješke |
|:---|:---|:---|
|UPN-A|Atribut UPN-a korisnika u klijentu za Azure AD|Atribut UPN iz klijenta sustava Azure AD sinkronizira se kao što je upravljani domene. Zbog toga najčešće pouzdan način da biste se prijavili upravljanih domene koristi vaš UPN-a.|
|SAMAccountName|MailNickname korisnika u klijentu za Azure AD atribut ili generirani|Atribut SAMAccountName je izvorni termini iz atributa mailNickname u klijentu za Azure AD. Ako više korisničkih računa imaju isti atribut mailNickname, u SAMAccountName automatski se generira. Ako mailNickname ili UPN prefiksa dulja je od 20 znakova, u SAMAccountName automatski se generira zadovoljili 20 znakova od dopuštenog na SAMAccountName atribute.|
|Lozinke|Korisničke lozinke iz klijenta sustava Azure AD|Vjerodajnica raspršivanja potrebne za NTLM ili Kerberos provjeru autentičnosti (naziva se i zamjenske vjerodajnice) sinkroniziraju s klijentom Azure AD. Ako je vaš klijent Azure AD sinkronizirane klijentu, te vjerodajnice su izvorni termini iz lokalne domene.|
|Primarni korisnika/grupe SID|Automatski generirani|Primarni SID za račune korisnika/grupe se automatski generirani upravljanih domene. Taj atribut podudaraju u primarni korisnika/grupe SID objekta u vaše lokalne domene servisa Active Directory. Nepodudarnost je jer upravljani domena ima različite prostora za naziv SID od lokalne domene.|
|Povijest SID za korisnike i grupe|Lokalni primarnog korisnika i grupa SID|Atribut SidHistory za korisnike i grupe u upravljanih domene postavljen tako da odgovara odgovarajuće primarnog korisnika ili grupu SID u lokalne domene. Ta značajka pomaže olakšali Dizalica-i-shift lokalnog aplikacija upravljanih domenu, jer ćete morati ponovno ACL resursi.|

> [AZURE.NOTE] **Prijavite se u upravljanih domene pomoću oblika UPN:** Atribut SAMAccountName možda generirani za neke korisničke račune u upravljanih domene. Ako ima isti atribut mailNickname veći broj korisnika ili korisnici imaju pretjerano dugi prefiksi UPN-a, SAMAccountName za ti korisnici možda automatski generirani. Stoga ni SAMAccountName oblik (na primjer, "CONTOSO100\joeuser") nije uvijek pouzdan način da biste se prijavili domeni. Automatski generirani SAMAccountName korisnika mogu se razlikovati od njihove prefiks UPN-a. Pomoću oblika UPN-a (Ako, na primjer, 'joeuser@contoso100.com') pouzdano prijaviti upravljanih domene.


## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>Objekte koji se sinkroniziraju klijent sustava Azure AD iz upravljanih domene
Kao što je opisano u prethodnom dijelu članka, postoji bez sinkronizacije putem upravljanih domene vratite se u klijentu za Azure AD. Možete odabrati da biste [stvorili na prilagođenu tvrtke ili ustanove jedinice (OU)](./active-directory-ds-admin-guide-create-ou.md) u upravljanih domene. Nadalje, možete stvoriti druge organizacijske jedinice, korisnike, grupe ili računa servisa unutar te prilagođene organizacijske jedinice. Nema objekata koje je stvorila unutar prilagođene organizacijske jedinice sinkroniziraju s klijentom Azure AD. Tih objekata mogu se koristiti samo unutar upravljanih domene. Stoga tih objekata nisu vidljive pomoću cmdleta za Azure AD PowerShell, API Azure AD grafikonu ili značajkom upravljanja Azure AD korisničkog Sučelja.


## <a name="related-content"></a>Srodni sadržaji
- [Značajke - Azure AD Domain Services](active-directory-ds-features.md)

- [Scenariji za implementaciju - Azure servisa Active Directory Domain Services](active-directory-ds-scenarios.md)

- [Zahtjevi za Azure AD domenske servise za povezivanje s mrežom](active-directory-ds-networking.md)

- [Početak rada s Azure servisa Active Directory Domain Services](active-directory-ds-getting-started.md)
