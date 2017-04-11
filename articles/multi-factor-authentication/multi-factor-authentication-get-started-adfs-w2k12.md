<properties
    pageTitle="MFA poslužitelju Windows Server 2012 R2 AD fs | Microsoft Azure"
    description="U ovom se članku opisuje kako započeti s Azure višestruke provjere autentičnosti i ADFS u sustavu Windows Server 2012 R2."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/14/2016"
    ms.author="kgremban"/>


# <a name="secure-your-cloud-and-on-premises-resources-using-azure-multi-factor-authentication-server-with-ad-fs-in-windows-server-2012-r2"></a>Zaštita pomoću Azure višestruke provjere autentičnosti poslužitelja za AD FS u sustavu Windows Server 2012 R2 oblaka i lokalnog resursa

Ako koristite Active Directory Federation Services (AD FS), a želite sigurne oblaka ili lokalnog resursa, možete konfigurirati Azure višestruke provjere autentičnosti poslužitelja za AD FS. Tu konfiguraciju pokreće potvrdu dva koraka za krajnje točke visoke vrijednosti.

U ovom se članku navode ćemo pomoću Azure višestruke provjere autentičnosti poslužitelja za AD FS u sustavu Windows Server 2012 R2. Da biste saznali više, pročitajte više o tome [sigurne oblaka i lokalnog](multi-factor-authentication-get-started-adfs-adfs2.md)resursima pomoću Azure višestruke provjere autentičnosti poslužitelja AD fs 2.0.

## <a name="secure-windows-server-2012-r2-ad-fs-with-azure-multi-factor-authentication-server"></a>Sigurne Windows Server 2012 R2 AD FS s poslužiteljem za Azure višestruka provjera autentičnosti

Kada instalirate Azure višestruke provjere autentičnosti poslužitelja, imate sljedeće mogućnosti:

- Instalacija Azure višestruke provjere autentičnosti poslužitelja lokalno na istom poslužitelju kao i AD FS
- Instalirajte prilagodnik Azure višestruka provjera autentičnosti lokalno na poslužitelju za AD FS, a zatim višestruke provjere autentičnosti poslužitelja na drugo računalo

Prije početka, imajte na umu sljedeće informacije:

- Niste potrebne za instalaciju Azure višestruke provjere autentičnosti poslužitelja na poslužitelj za AD FS. Međutim, morate instalirati prilagodnik višestruke provjere autentičnosti za AD FS na programa Windows Server 2012 R2 sa sustavom AD FS. Poslužitelj možete instalirati na drugo računalo ako je podržana verzija i instalirate prilagodnik za AD FS zasebno na poslužitelj za vanjski pristup za AD FS. Pogledajte sljedeće postupke da biste saznali kako instalirati prilagodnik zasebno.

- Kada je dizajniran prilagodnik MFA poslužitelja AD FS, on je očekivanu AD FS može proći naziv relying strana na prilagodnik. Nakon toga naziv relying strana može koristiti kao naziv aplikacije. No to izgleda ne treba slučaj. Ako vaša tvrtka ili ustanova koristi tekstnu poruku ili metoda provjere mobilne aplikacije, nizovi definiranim u postavkama tvrtke sadrži rezervirano mjesto, <$*application_name*$>. To rezervirano mjesto ne zamjenjuje se automatski kada koristite AD FS prilagodnika. Preporučujemo da uklonite rezerviranog mjesta s odgovarajućim nizovi kada zaštitite AD FS.

- Račun koji koristite za prijavu mora imati korisnička prava za stvaranje sigurnosne grupe u servisu Active Directory.

- Čarobnjak za instalaciju višestruka provjera autentičnosti AD FS prilagodnik stvara sigurnosnu grupu naziva PhoneFactor administratori u vašoj instanci servisa Active Directory. Zatim dodaje račun servisa AD FS servisa za vanjski pristup za ovu grupu. Preporučujemo da provjerite na upravljaču domene grupi administratori PhoneFactor uistinu stvorili, a da AD FS račun za servis je član grupe. Ako je potrebno, ručno dodati račun servisa AD FS u grupi administratori PhoneFactor na upravljaču domene.

- Informacije o instaliranju servisa SDK Web pomoću portala za korisnika, Saznajte više o [Implementacija korisnički portal za Azure višestruke provjere autentičnosti poslužitelja.](multi-factor-authentication-get-started-portal.md)


### <a name="install-azure-multi-factor-authentication-server-locally-on-the-ad-fs-server"></a>Lokalno instalirati Azure višestruke provjere autentičnosti poslužitelja na poslužitelj za ADFS

1. Preuzmite i instalirajte Azure višestruke provjere autentičnosti poslužitelja na poslužitelj za AD FS. Informacije o instalaciji, pročitajte o [početku rada s Azure višestruke provjere autentičnosti poslužitelja](multi-factor-authentication-get-started-server.md).
2. Na konzoli za upravljanje Azure višestruke provjere autentičnosti poslužitelja kliknite ikonu **servisa AD FS** , a zatim opcija **Dopusti korisniku registraciju** i **Dopusti korisnicima da biste odabrali način**.
3. Odaberite i druge mogućnosti koje želite navesti za tvrtku ili ustanovu.
4. Kliknite **Instaliraj prilagodnik za AD FS**.
<center>![Oblak](./media/multi-factor-authentication-get-started-adfs-w2k12/server.png)</center>
5. Ako se prikaže prozor servisa Active Directory, to znači dvije stvari. Računalo je pridruženo domeni, a konfiguracija servisa Active Directory za osiguravanje komunikacije između prilagodnik za AD FS i servis višestruka provjera autentičnosti nije potpun. Kliknite **Dalje** da biste automatski dovršiti tu konfiguraciju ili odaberite na **preskočiti Automatska konfiguracija servisa Active Directory i konfiguriranje postavki ručno** potvrdni okvir, a zatim kliknite **Dalje**.
6. Ako se prikaže windows lokalne grupe, to znači dvije stvari. Računalo je pridruženo domeni, a konfiguracija lokalne grupe za osiguravanje komunikacije između prilagodnik za AD FS i servis višestruka provjera autentičnosti nije potpun. Kliknite **Dalje** da biste automatski dovršiti tu konfiguraciju ili odaberite na **preskočiti Automatska konfiguracija lokalne grupe i konfiguriranje postavki ručno** potvrdni okvir, a zatim kliknite **Dalje**.
7. U čarobnjaku za instalaciju kliknite **Dalje**. Azure višestruke provjere autentičnosti poslužitelja stvara grupu administratori PhoneFactor i dodaje račun servisa AD FS grupi PhoneFactor administratori.
<center>![Oblak](./media/multi-factor-authentication-get-started-adfs-w2k12/adapter.png)</center>
8. Na stranici **Pokretanje instalacijskog programa** , kliknite **Dalje**.
9. U prilagodnik installer višestruka provjera autentičnosti AD FS, kliknite **Dalje**.
10. Kada se instalacija završi, kliknite **Zatvori** .
11. Nakon instalacije prilagodnik morate registrirati AD fs. Otvorite Windows PowerShell, a zatim pokrenite sljedeću naredbu:<br>
    `C:\Program Files\Multi-Factor Authentication Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1`
   <center>![Oblak](./media/multi-factor-authentication-get-started-adfs-w2k12/pshell.png)</center>
12. Da biste koristili upravo registrirani prilagodnik, uredite pravilo globalni provjere autentičnosti u AD FS. Konzola za upravljanje AD FS, idite na čvor **Pravila za provjeru autentičnosti** . U odjeljku **Višestruka provjera autentičnosti** , kliknite **Uredi** vezu pokraj odjeljka **Globalne postavke** . U prozoru **Uređivanje globalni pravila za provjeru autentičnosti** **Višestruka provjera autentičnosti** kao odaberite način provjere autentičnosti dodatne, a zatim **u redu**. Prilagodnik je registrirana kao WindowsAzureMultiFactorAuthentication. Ponovno pokrenite servis za AD FS za registraciju na snagu.

<center>![Oblak](./media/multi-factor-authentication-get-started-adfs-w2k12/global.png)</center>

U ovom trenutku višestruke provjere autentičnosti poslužitelja je postavljena za biti davateljem dodatno unositi će se koristiti za AD FS.

## <a name="install-a-standalone-instance-of-the-ad-fs-adapter-by-using-the-web-service-sdk"></a>Instalirajte samostalne pojavljivanja prilagodnik za AD FS pomoću servisa SDK Web
1. Instalirajte servis SDK Web na poslužitelju na kojem se izvodi višestruke provjere autentičnosti poslužitelja.
2. Kopirajte sljedeće datoteke iz na \Program Files\Multi-faktor provjeru autentičnosti poslužitelja direktorij na poslužitelju na kojem planirate instalirati prilagodnik za AD FS:
  - MultiFactorAuthenticationAdfsAdapterSetup64.msi
  - REGISTER-MultiFactorAuthenticationAdfsAdapter.ps1
  - Unregister MultiFactorAuthenticationAdfsAdapter.ps1
  - MultiFactorAuthenticationAdfsAdapter.config
3. Pokrenite MultiFactorAuthenticationAdfsAdapterSetup64.msi instalacijsku datoteku.
4. U prilagodnik installer višestruka provjera autentičnosti AD FS, kliknite **Dalje** da biste pokrenuli instalaciju.
5. Kada se instalacija završi, kliknite **Zatvori** .

## <a name="edit-the-multifactorauthenticationadfsadapterconfig-file"></a>Uređivanje datoteke MultiFactorAuthenticationAdfsAdapter.config

Slijedite ove korake da biste uredili datoteku MultiFactorAuthenticationAdfsAdapter.config:

1. Čvor **UseWebServiceSdk** postavite na **true**.  
2. Postavite vrijednost za **WebServiceSdkUrl** URL višestruke provjere autentičnosti Web servisa SDK-a. Na primjer: *https://contoso.com/&lt;certificatename&gt;/MultiFactorAuthWebServicesSdk/PfWsSdk.asmx*, pri čemu je certificatename certifikata.  
3. Uređivanje skripte Register MultiFactorAuthenticationAdfsAdapter.ps1 dodavanjem *- ConfigurationFilePath &lt;put&gt; * na kraj na `Register-AdfsAuthenticationProvider` naredbe, gdje * &lt;put&gt; * je cijeli put do datoteke MultiFactorAuthenticationAdfsAdapter.config.

### <a name="configure-the-web-service-sdk-with-a-username-and-password"></a>Konfiguriranje usluge SDK Web pomoću korisničkog imena i lozinke

Postoje dvije mogućnosti za konfiguriranje servisa SDK Web. Prvi je pomoću korisničkog imena i lozinke, drugi certifikatom klijenta. Slijedite ove korake za prvu mogućnost ili prijeđite na sekundu.  

1. Postavite vrijednost za **WebServiceSdkUsername** na račun koji je član PhoneFactor administratori sigurnosne grupe. Korištenje na &lt;domene&gt;& #92; &lt;korisničko ime&gt; oblik.  
2. Postavite vrijednost za **WebServiceSdkPassword** na odgovarajuće lozinkom.

### <a name="configure-the-web-service-sdk-with-a-client-certificate"></a>Konfiguriranje usluge SDK Web s potvrdom klijenta

Ako ne želite koristiti korisničko ime i lozinku, slijedite ove korake da biste konfigurirali servis SDK Web potvrdu klijenta.

1. Klijentski certifikat možete dobiti od ustanove za izdavanje certifikata za poslužitelja na kojem je instaliran servis SDK Web. Saznajte kako [nabaviti klijentskih potvrda](https://technet.microsoft.com/library/cc770328.aspx).  
2. Uvoz certifikata klijenta spremište osobnog certifikata na lokalnom računalu na poslužitelju na kojem je pokrenut servis SDK Web. Provjerite je li ustanove za izdavanje certifikata javni certifikat u spremištu certifikata pouzdani korijenski certifikati.  
3. Izvezite javni i privatni ključevi klijentski certifikat .pfx datoteka.  
4. Izvezite javni ključ u obliku Base64 .cer datoteka.  
5. U upravitelju poslužitelja provjeriti je li omogućen značajka provjera autentičnosti za mapiranje klijentskih potvrda \Web Server\Security\IIS Web Server (IIS). Ako nije instaliran, odaberite **Dodaj uloga i značajki** da biste dodali ovu značajku.  
6. U upravitelju IIS dvokliknite **Uređivač konfiguracije** web-mjesto koje sadrži virtualnog direktorija Web servisa SDK. Važno je da to učinite na razini web-mjesta, a ne na razini virtualnog direktorija.  
7. Prijeđite na odjeljak **system.webServer/security/authentication/iisClientCertificateMappingAuthentication** .  
8. Postavljanje omogućena na **true**.  
9. Postavite oneToOneCertificateMappingsEnabled na **true**.  
10. Kliknite gumb **…** uz oneToOneMappings, a zatim kliknite **Dodaj** vezu.  
11. Otvorite datoteku .cer Base64 ranije izvezli. Uklanjanje *---ZAPOČETI certifikat---*, *---ZAVRŠILI certifikat---*i sve prijelome. Kopirajte niz za rezultata.  
12. Postavljanje potvrda niz kopirali u prethodnom koraku.  
13. Postavljanje omogućena na **true**.  
14. Postavite korisničko ime za račun koji je član PhoneFactor administratori sigurnosne grupe. Korištenje na &lt;domene&gt;& #92; &lt;korisničko ime&gt; oblik.  
15. Postavite lozinku na odgovarajući račun lozinku, a zatim zatvorite uređivač konfiguracije.  
16. Kliknite **Primijeni** vezu.  
17. U imeniku Web servisa SDK virtualne dvokliknite **provjeru autentičnosti**.  
18. Provjerite je li se postavljaju na **omogućeno**oponašanje ASP.NET i osnovnu provjeru autentičnosti i druge stavke postavljene na **onemogućene**.  
19. U imeniku Web servisa SDK virtualne dvokliknite **SSL postavke**.  
20. Postavite klijentskih potvrda **Prihvati**, a zatim kliknite **Primijeni**.  
21. Kopirajte .pfx datoteka ranije izvezli na poslužitelju na kojem se izvodi prilagodnik za AD FS.  
22. Uvoz .pfx datoteka u spremištu osobnih certifikata lokalnom računalu.  
23. Desnom tipkom miša i odaberite **Upravljanje privatni ključevi**i dodijeliti pristup za račun koji ste koristili za prijavu na servis za AD FS čitanje.  
24. Otvorite klijentski certifikat, a zatim kopirajte na otisak prsta na kartici **pojedinosti** .  
25. U datoteci MultiFactorAuthenticationAdfsAdapter.config postavite **WebServiceSdkCertificateThumbprint** niz kopirali u prethodnom koraku.  


Na kraju, da biste registrirali prilagodnik, pokrenite na \Program Files\Multi-Server\Register-MultiFactorAuthenticationAdfsAdapter.ps1 provjere autentičnosti faktor skripte u ljusci PowerShell. Prilagodnik je registrirana kao WindowsAzureMultiFactorAuthentication. Ponovno pokrenite servis za AD FS za registraciju na snagu.

## <a name="related-topics"></a>Povezane teme

Rješavanje problema potražite u odjeljku [Najčešća pitanja za Azure višestruke provjere autentičnosti](multi-factor-authentication-faq.md)
