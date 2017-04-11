<properties
    pageTitle="Azure AD Connect: Podržani topologija | Microsoft Azure"
    description="Ova tema detaljno podržane i nepodržane topologija za Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="AndKjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="billmath"/>

# <a name="topologies-for-azure-ad-connect"></a>Povezivanje topologija za Azure AD
Cilj u ovoj se temi je opisuje različitih lokalnih i topologija Azure AD za Azure AD Connect sinkronizaciju kao ključne Integracija rješenje. Opisuje podržane i nepodržane konfiguracije.

Legende za slike u dokumentu:

Opis | Ikona
-----|-----
Skup stabala lokalnog servisa Active Directory| ![AD](./media/active-directory-aadconnect-topologies/LegendAD1.png)
Active Directory s filtriranim uvoza| ![AD](./media/active-directory-aadconnect-topologies/LegendAD2.png)
Azure AD Connect sinkronizacijom poslužitelja| ![Sinkronizacija](./media/active-directory-aadconnect-topologies/LegendSync1.png)
Azure AD Connect sinkronizacije "pripremna načinu poslužitelja"| ![Sinkronizacija](./media/active-directory-aadconnect-topologies/LegendSync2.png)
GALSync FIM2010 ili MIM2016| ![Sinkronizacija](./media/active-directory-aadconnect-topologies/LegendSync3.png)
Azure AD Connect sinkronizaciju poslužitelj detaljne| ![Sinkronizacija](./media/active-directory-aadconnect-topologies/LegendSync4.png)
Azure AD direktorija |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png)
Nepodržane scenarija | ![Nije podržana](./media/active-directory-aadconnect-topologies/LegendUnsupported.png)

## <a name="single-forest-single-azure-ad-tenant"></a>Jedan skup stabala, jedan Azure AD klijentu
![Jedan skup stabala jedan klijentu](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

Najčešći topologije je jedan skup stabala smjernice o lokalnom s jednu ili više domena, a jedan Azure AD. Za provjeru autentičnosti Azure AD, koristi sinkronizaciju lozinke. Express instalaciju Azure AD Connect podržava samo ovaj topologije.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Smjernice za jedan skup stabala više sinkronizacijom poslužitelja za jednu Azure AD
![Jedan skup stabala filtrirane nije podržana](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Nije podržan imati više Azure AD Connect sinkronizacijom poslužitelja povezan s istom klijentu Azure AD osim na [pripremna poslužitelja](#staging-server). Nije podržan je čak i ako se to su konfiguriran za sinkroniziranje isključivih skup objekata. Koje možda imaju koje se smatraju to ako ne možete pristupiti svim domena u skup stabala s jednog poslužitelja ili učitavanje raspodijelite nekoliko poslužitelja.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Više šuma jedan Azure AD klijenta
![Višestruki skupa stabala jednog klijenta](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Mnoge tvrtke ili ustanove imaju okruženja u kojima se s više šuma lokalnog servisa Active Directory. Postoje razloge imate više od jednog skupa stabala lokalnog servisa Active Directory. Uobičajeni primjeri su dizajna s šuma račun resursa i kao rezultat nakon spajanja i nabavu.

Kad imate više šuma, sve šuma mora biti dostupna tako da jedan Azure AD Connect sinkronizacijom poslužitelja. Ne morate uključiti poslužitelj s domenom. Ako je potrebno dosegne skup svih stabala poslužitelja možete staviti u mreži DMZ.

Čarobnjak za instalaciju Azure AD Connect nudi nekoliko mogućnosti za konsolidaciju korisnike prikazane u više šuma. Cilj je li korisnik samo predstavlja jednom u Azure AD. Postoji nekoliko uobičajenih topologija koja možete konfigurirati na putu prilagođenu instalaciju u čarobnjaku za instalaciju. Odaberite odgovarajuću mogućnost koji predstavlja topologiji na stranici **jedinstveno prepoznavanje korisnika**. Konsolidacija samo je konfiguriran za korisnike. Dvostruko grupe se su Konsolidirani sa zadanom konfiguracijom.

Uobičajeni topologija se spominju u sljedećem odjeljku: [zasebnom topologija](#multiple-forests-separate-topologies), [cijeli mrežasto tkanje](#multiple-forests-full-mesh-with-optional-galsync)i [Resursa za račun](#multiple-forests-account-resource-forest).

Zadana konfiguracija sinkronizirano Azure AD Connect pretpostavlja:

1. Korisnici imati samo jedan račun omogućeno i skup stabala u kojoj se nalazi ovaj račun koristi se za provjeru autentičnosti korisnika. U ovom pretpostavlja se oba sinkronizaciju lozinke i za vanjski pristup. UserPrincipalName i sourceAnchor/immutableID potjecati iz ovog skupa stabala.
2. Korisnici imati samo jedan poštanski sandučić.
3. Skup stabala koji hostira poštanski sandučić za korisnika ima najbolju kvalitetu podataka za atribute koje su vidljive u sustava Exchange globalni popis adresa (GAL). Ako postoji nijedan poštanski sandučić na korisnike, sve skupa stabala može koristiti da biste slali vrijednosti tih atributa.
4. Ako imate povezane poštanski sandučić, zatim postoji i drugi račun u različitim skupa stabala koristi za prijavu.

Ako vaše okruženje podudaraju se s ove pretpostavke, događa se sljedeće:

- Ako imate više od jedne aktivni račun ili više od jedne poštanskog sandučića, modula za sinkroniziranje izdvaja jedan i drugi Zanemari.
- Povezane poštanski sandučić s bez aktivni račun ne izvozi za Azure AD. Korisnički račun nije predstavljen kao član u bilo kojoj grupi. Povezane poštanskog sandučića u DirSync uvijek predstavlja kao normalne poštanski sandučić. Ta promjena je namjerno različite ponašanje radi bolje podržava više skupa stabala scenarije.

Dodatne informacije pronaći ćete u [objašnjenje configuation zadani](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Više šuma, više sinkronizacijom poslužitelja za jednog klijenta za Azure AD
![Višestruki skupa stabala višestruki sinkronizacije nije podržana](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Nije podržano imati više od jedne poslužitelj za Azure AD povezivanje sinkronizaciju povezan s jednom Azure AD klijenta. Iznimka je korištenje na [pripremna poslužitelja](#staging-server).

### <a name="multiple-forests--separate-topologies"></a>Više šuma – zasebnom topologija
**Korisnici prikazane su samo jednom preko svih direktorija**

![Jednom skupa stabala između više korisnika](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Višestruki skupa stabala Seperate topologija](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

U okruženju, sve šuma lokalnog tretiraju se kao zasebna entiteti i nijedan korisnik bio koje su prisutne u drugi skup stabala.
Svaki skup stabala ima vlastitu Exchange tvrtke ili ustanove i između na šuma postoji bez GALSync. U ovom topologije može biti situacija nakon spajanja tvrtki/nabavu ili u tvrtki ili ustanovi gdje se svaki poslovne jedinice radi odvajanja međusobno. Ove šuma iz iste tvrtke ili ustanove u Azure AD i prikazuju s objedinjenih GAL.
Na ovoj slici svaki objekt u svaki skup stabala je prikazan jednom u metaverse i pridružuje u ciljnom Azure AD klijentu.

### <a name="multiple-forests--match-users"></a>Više šuma – podudaranje korisnika
**Identiteti korisnika postoji preko više direktorija**

Uobičajeni te scenarije je da su raspodjele i sigurnosne grupe mogu sadržavati kombinacije korisnika, kontakata i FSPs (vanjski sigurnosni upravitelji)

FSPs se koriste u ZBRAJA predstavlja članova iz drugih šuma u sigurnosnoj grupi. Sve FSPs se riješiti na stvarni objekt Azure AD.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Više šuma – puni mreža s neobavezno GALSync
**Identiteti korisnika postoji preko više direktorija. Usklađivanje pomoću: atribut pošte**

![Pošta skupa stabala između više korisnika](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Višestruki skupa stabala puno mrežasto tkanje](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Potpuna mrežasto tkanje topologije omogućuje korisnika i resursa koji će se nalaziti u bilo kojem skupa stabala i obično činite postojati dvosmjerni trusts između na šuma.

Ako je Exchange koje su prisutne u više od jednog skupa stabala može po želji biti rješenje za lokalni GALSync. Svaki korisnik predstavlja kao kontakt u drugim šuma. GALSync obično je implementirati pomoću Upravitelj identiteta Forefront 2010 ili Microsoft Upravitelj identiteta 2016. Azure AD Connect nije moguće koristiti za lokalni GALSync.

U ovom scenariju identiteta objekata spojeni pomoću atribut pošta. Korisnik s poštanskim sandučićem u jedan skup stabala pridruženo s kontaktima iz drugih šuma.

### <a name="multiple-forests--account-resource-forest"></a>Više šuma – skupa stabala račun resursa
**Identiteti korisnika postoji preko više direktorija. Usklađivanje pomoću: ObjectSID i msExchMasterAccountSID atributa**

![ObjectSID skup stabala između više korisnika](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Višestruki skupa stabala AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

U skup stabala topologija račun resurse imate šuma račun s aktivnim korisničkim računima. Imate šuma resursa s onemogućeni računi.

U ovom scenariju (neke) **skupa stabala resursa** smatra pouzdanima sve **šuma računa**. Skupa stabala resursa obično je shemom proširenog AD sa sustava Exchange i Lync. Sve servise sustava Exchange i Lync, kao i druge usluge za zajedničko korištenje nalaze se u ovaj skup stabala. Korisnici imaju onemogućene korisnički račun u ovu skupa stabala i poštanski sandučić je povezana s skupa stabala računa.

## <a name="office-365-and-topology-considerations"></a>Office 365 i topologije razmatranja
Nekoliko radnih opterećenja sustava Office 365, morate određene ograničenja podržanih topologija. Ako namjeravate koristiti bilo koji od njih, pročitajte temu podržanih topologija za povećavaju.

Radno opterećenje |  
--------- | ---------
Exchange Online | Ako postoji više od jedne Exchange tvrtke ili ustanove lokalnog (to jest, Exchange implementiran više od jednog skupa stabala), a zatim koristite Exchange 2013 SP1 ili noviji. Detalji o nalazi se ovdje: [Hibridne implementacije s više šuma servisa Active Directory](https://technet.microsoft.com/library/jj873754.aspx)
Skype za tvrtke | Ako koristite više šuma lokalnog podržana je samo topologije skupa stabala resursa za račun. Detalje o za podržanih topologija nalazi se ovdje: [okolini preduvjeti za Skype za tvrtke poslužiteljska verzija 2015](https://technet.microsoft.com/library/dn933910.aspx)

## <a name="staging-server"></a>Pripremna poslužitelja
![Pripremna poslužitelja](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect podržava instaliranju drugi poslužitelj u **načinu rada pripremna**. Na poslužitelju u tom načinu čita podatke iz svih povezanih direktorija, ali pišite bilo da biste povezanog direktorija. Koristi ciklusa normalni sinkronizacije i stoga je ažuriranu kopiju podataka identitet. U Izrada gdje primarni poslužitelj ne uspije koje možete uspjeti putem pripremni poslužitelj. To se u čarobnjaku za Azure AD Connect. Drugi poslužitelj kapaciteta može se nalaziti na različitim podatkovnog centra jer nema infrastrukture se zajednički koriste s primarnom poslužitelju. Ručno kopirajte promjene konfiguracije na primarni poslužitelj tako da drugi poslužitelj.

Pripremna server može se koristiti i da biste testirali novu prilagođenu konfiguraciju i efekta sadrži nad podacima. Možete pregledati promjene i prilagodite konfiguraciju. Kada ste zadovoljni Nova konfiguracija, možete učiniti pripremna poslužitelja aktivnog poslužitelja i postaviti stare aktivnog poslužitelja u pripremna način.

Ovu metodu može se koristiti i da biste zamijenili poslužitelja aktivnog sinkronizaciju. Priprema novi poslužitelj i postavite ga u pripremna načinu rada. Provjerite je li je u stanju dobro, onemogućivanje pripremna način (čime aktivni), a isključi trenutno aktivnog poslužitelja.

Nije moguće imati više od jednog pripremna poslužitelja kada želite imati više kopija centre za različite podatke.

## <a name="multiple-azure-ad-tenants"></a>Više klijenata za Azure AD
Microsoft preporučuje pojavljuju jednog klijenta u Azure AD za tvrtke ili ustanove.
Prije nego što namjeravate koristiti više klijenata za Azure AD ove teme pokrivaju uobičajeni scenariji koji vam omogućuje korištenje jednog klijenta.

Tema |  
--------- | ---------
Delegiranje pomoću administratora jedinice | [Upravljanje administratora jedinice u Azure AD](active-directory-administrative-units-management.md)

![Višestruki skupa stabala višestruki klijenta](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Postoji 1:1 odnosa između tablica i poslužitelj za Azure AD Connect sinkronizaciju klijent za Azure AD. Za svaki klijent Azure AD potrebno vam je jedan instalaciju Azure AD Connect sinkronizacijom poslužitelja. Klijent instance Azure AD su dizajnom Izolirani i korisnici u jednom ne vide korisnika na klijentu. Ako je namijenjen ovaj odvojenosti, a zatim je podržana konfiguracije, ali u suprotnom trebali biste koristiti u jednoj Azure AD klijentu modela.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Svaki objekt samo jednom u klijentu za Azure AD
![Skup stabala jednog filtriranja](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

U ovom topologiji jedan poslužitelj za Azure AD Connect sinkronizaciju povezan je s svaki klijent Azure AD. Sinkronizacija poslužitelje Azure AD Connect mora biti konfigurirana za filtriranje da bi se svaka ima isključivih skupa objekata da biste upravljali. Možete, primjerice doseg svaki poslužitelj za određene domene ili organizacijsku jedinicu. DNS domena biti registriran samo u jednom Azure AD klijentu. Na UPNs korisnika u lokalnom sustavu AD morate koristiti odvojene prostore za naziv kao i. Na primjer, u sliku iznad tri zasebna UPN nastavke registrirane u lokalnom sustavu AD: contoso.com, fabrikam.com i wingtiptoys.com. Korisnici u svakom lokalne domene servisa Active Directory pomoću različitih prostora za naziv.

Postoji bez GALsync između instanci klijentu Azure AD. U adresar u sustavu Exchange Online i Skype za tvrtke samo prikazuje korisnike u istom klijentu.

Ovaj topologije sadrži sljedeće ograničenja u suprotnom podržane scenarije:

- Samo jedan od klijenata za Azure AD možete omogućiti hibridna implementacija sustava Exchange s lokalnim servisom Active Directory.
- Uređaji za Windows 10 možete se povezati samo s jednog klijenta Azure AD.

Obavezu isključivih skupa objekata odnosi i na upisima. Neke značajke upisima nisu podržani uz ovaj topologije od tih značajki pretpostavlja jedan konfiguriranje lokalnog:

-   Grupa upisima sa zadanom konfiguracijom
-   Uređaj upisima

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Svaki objekt više puta u klijentu za Azure AD
![Jedan skup stabala višestruki klijent nije podržana](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Jedan skup stabala višestruki poveznika nije podržana](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

- Podržana je da biste sinkronizirali isti korisniku više klijenata za Azure AD.
- Nije podržana. da biste promijenili da bi korisnici u jedan Azure AD prikazuju se kao kontakata u drugi klijent Azure AD.
- Podržana je da biste izmijenili Azure AD Connect sinkronizaciju povezati s više klijenata za Azure AD.

### <a name="galsync-by-using-writeback"></a>GALsync pomoću upisima
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD klijenata su Izolirani dizajna.

- Podržana je da biste promijenili konfiguraciju Azure AD Connect sinkronizacije za čitanje podataka na drugi klijent Azure AD.
- Ne da biste izvezli korisnika kao kontakata u drugu lokalnog AD pomoću sinkronizacije Azure AD Connect.

### <a name="galsync-with-on-premises-sync-server"></a>GALsync s lokalnog poslužitelja za sinkronizaciju
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

Podržano je korištenje FIM2010/MIM2016 lokalnim korisnicima GALsync između dva Exchange tvrtke ili ustanove. Korisnici u tvrtki ili ustanovi jedan prikazuje kao vanjski korisnici/kontakte iz druge tvrtke ili ustanove. Ove reklame različitih lokalnih mogu se sinkronizirati s vlastite Azure AD korisnicima.

## <a name="next-steps"></a>Daljnji koraci
Da biste saznali kako instalirati Azure AD Connect scenarija, potražite u članku [Instalacija Prilagođeno Azure AD Connect](./connect/active-directory-aadconnect-get-started-custom.md).

Dodatne informacije o konfiguraciji [Azure AD Connect sinkronizirati](active-directory-aadconnectsync-whatis.md) .

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
