<properties
   pageTitle="Klaster tkanina servis za sigurnu | Microsoft Azure"
   description="U članku se opisuje sigurnost scenariji za servis tkanina klaster i različite tehnologije za implementaciju tih scenarija."
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/19/2016"
   ms.author="chackdan"/>

# <a name="service-fabric-cluster-security-scenarios"></a>Scenariji za sigurnost servisa tkanina klaster

Servis tkanina klaster je resurs da ste vlasnik. Klastere zaštićenim moraju se uvijek da biste spriječili povezivanje svoj klaster, osobito kada je radnih opterećenja radnog radi na tome neovlaštenih. Iako je moguće stvoriti nezaštićenu klaster, to tako da omogućuje sve anonimni korisnik s njim povezati ako je izlaže upravljanje krajnje točke na Internet za javno. 

Ovaj članak sadrži pregled sigurnost scenariji za klastere sustavom Azure ili samostalnu i različite tehnologije za implementaciju tih scenarija. Scenariji za sigurnost klaster su:

- Sigurnost čvor čvor
- Sigurnost čvor klijenta
- Kontrola pristupa na temelju uloga (RBAC)

## <a name="node-to-node-security"></a>Sigurnost čvor čvor
Secures komunikaciju između VMs ili strojeva u klasteru. Time se osigurava da samo računala koji su ovlašteni za uključivanje klaster može sudjelovati u hosting aplikacija i servisa u klasteru.

![Dijagram čvor čvor komunikacije][Node-to-Node]

Klastere sustavom Azure ili samostalnu klastere sustavom Windows možete koristiti [Sigurnosni certifikat](https://msdn.microsoft.com/library/ff649801.aspx) ili [Sigurnost sustava Windows](https://msdn.microsoft.com/library/ff649396.aspx) za računala za Windows Server.
### <a name="node-to-node-certificate-security"></a>Sigurnosni certifikat čvor čvor
Servis tkanina koristi X.509 poslužitelja certifikata koji ste naveli kao dio konfiguracije vrsta čvora prilikom stvaranja klaster. Kratak pregled koji su ove potvrde i kako mogu nabaviti ili ih stvorite navedeni su na kraju ovog članka.

Sigurnosni certifikat konfiguriran prilikom stvaranja klaster portala za Azure, predlošci Voditelj resursa Azure ili predložak JSON samostalne. Možete navesti primarni certifikat i neobavezna sekundarne certifikat koji se koristi za potvrdu prebacivanja. Certifikati primarnih i sekundarnih navedete mora biti razlikuje se od klijenta za administratore i samo za čitanje klijent certifikati navedete za [klijent čvor sigurnost](#client-to-node-security).

Azure u članku [Postavljanje klaster tako da pomoću predloška Azure Voditelj resursa](service-fabric-cluster-creation-via-arm.md) da biste saznali kako konfigurirati sigurnosni certifikat klasteru.

Samostalne Windows Server potražite [sigurne samostalne klaster u sustavu Windows pomoću X.509 certifikate](service-fabric-windows-cluster-x509-security.md)

### <a name="node-to-node-windows-security"></a>Sigurnost čvor čvor sustava windows
Samostalne Windows Server potražite [sigurne samostalne klaster u sustavu Windows pomoću sigurnost sustava Windows](service-fabric-windows-cluster-windows-security.md)

## <a name="client-to-node-security"></a>Sigurnost čvor klijenta
Potvrđuje klijenata i secures komunikaciju između klijenta i pojedinačne čvorovi u klasteru. Ta vrsta sigurnosti potvrđuje i secures komunikacije klijent, koja omogućuje da samo ovlašteni korisnici mogu pristupati klaster i aplikacije u uveden u klaster. Klijenti jedinstveno prepoznaju se kroz svoja uvjerenja sigurnost sustava Windows ili svoja uvjerenja sigurnosni certifikat.

![Dijagram klijent čvor komunikacije][Client-to-Node]

[Sigurnosni certifikat](https://msdn.microsoft.com/library/ff649801.aspx) ili [Sigurnost sustava Windows](https://msdn.microsoft.com/library/ff649396.aspx), poslužite se klastere sustavom Azure ili samostalnu klastere sustavom Windows.

### <a name="client-to-node-certificate-security"></a>Zaštita certifikatom čvor klijenta
 Zaštita certifikatom čvor klijent konfiguriran prilikom stvaranja klaster ili putem Azure portalu resursima predložaka ili predložak JSON samostalne navođenjem potvrdu administrator klijenta i/ili potvrdu korisnik klijenta.  Administrator klijenta i korisnički klijent certifikate navedete mora biti razlikuje se od primarnih i sekundarnih certifikata koji navedete za [Sigurnost čvor čvor](#node-to-node-security).

Klijenti za povezivanje s klaster pomoću certifikata za administratore imali puni pristup mogućnostima upravljanja.  Klijenti za povezivanje s klaster pomoću certifikata samo za čitanje korisniku klijenta imati samo za čitanje za mogućnosti upravljanja. Drugim riječima ove potvrde se koriste za ulogu baza kontrolu pristupa (RBAC) što je opisano u nastavku ovog članka.

Azure u članku [Postavljanje klaster tako da pomoću predloška Azure Voditelj resursa](service-fabric-cluster-creation-via-arm.md) da biste saznali kako konfigurirati sigurnosni certifikat klasteru.

Samostalne Windows Server potražite [sigurne samostalne klaster u sustavu Windows pomoću X.509 certifikate](service-fabric-windows-cluster-x509-security.md)

### <a name="client-to-node-azure-active-directory-aad-security-on-azure"></a>Sigurnost Azure Active Directory (AAD) klijent čvor Azure
Klastere sustavom Azure možete i siguran pristup za krajnje točke upravljanja pomoću Azure Active Directory (AAD). Pročitajte članak [Postavljanje klaster tako da pomoću predloška Azure Voditelj resursa](service-fabric-cluster-creation-via-arm.md) za informacije o stvaranju potrebne AAD artefakte, kako ih popunjavanje tijekom stvaranja klaster te kako naknadno povezati s tim klastere.

## <a name="security-recommendations"></a>Preporuke o sigurnosti
Za klastere Azure, preporučuje se korištenje AAD sigurnosti za provjeru autentičnosti klijenti i certifikati za sigurnost čvor čvor.

Za samostalne Windows Server klastere preporučuje se korištenje sigurnost sustava Windows s grupom upravlja računima (GMA) ako imate Windows Server 2012 R2 i servisa Active Directory. U suprotnom i dalje koristiti sigurnost sustava Windows s računa za Windows.

## <a name="role-based-access-control-rbac"></a>Kontrola pristupa (RBAC) na temelju uloga
Kontrola pristupa omogućuje klaster administrator da biste ograničili pristup određene operacije klaster za različite grupe korisnika, a zatim dodatna klaster zaštita. Dvije vrste kontrola drugačiji pristup podržani za klijente povezati klaster: administratorsku ulogu i korisnička uloga.

Administratori imaju puni pristup mogućnostima upravljanja (uključujući mogućnosti za čitanje/pisanje). Korisnici, po zadanom imaju samo za čitanje za mogućnosti upravljanja (na primjer, mogućnosti upita) i mogućnost da biste riješili aplikacija i servisa.

Navedite administratorske i korisničke uloge klijenta prilikom stvaranja klaster unosom zasebna identiteta (potvrde AAD itd.) za svaki unos. Dodatne informacije o zadane postavke za kontrolu pristupa i kako promijeniti zadane postavke potražite u članku [Kontrola pristupa za servis tkanina klijente na temelju uloga](service-fabric-cluster-security-roles.md).


## <a name="x509-certificates-and-service-fabric"></a>X.509 certifikate i tkanina servisa
Digitalni certifikati X.509 koji se obično koriste se za provjeru autentičnosti klijenata i poslužitelja i za šifriranje i digitalno potpisivanje poruka. Dodatne informacije o ove potvrde, otvorite [Rad s certifikata](http://msdn.microsoft.com/library/ms731899.aspx).

Neki važnih stvari koje treba uzeti u obzir:

- Potvrde koje se koriste u klastere izvodi radnih opterećenja radnog trebali biste stvoren pomoću pravilno konfigurirani servis certifikata za Windows Server ili dobivenog odobrene [Za izdavanje certifikata (CA)](https://en.wikipedia.org/wiki/Certificate_authority).
- Nikad ne koristite neki privremeno ili testirajte potvrde u proizvodnje koje su stvorene pomoću alata kao što je MakeCert.exe.
- Poslužite se samopotpisani certifikat, ali biste trebali samo učiniti za klastere test, a ne u radnog.

### <a name="server-x509-certificates"></a>Poslužitelj X.509 certifikate

Certifikati poslužitelja imati primarni zadatka provjere autentičnosti na poslužitelju (čvora) klijentima ili provjere autentičnosti na poslužitelju (čvora) na poslužitelju (čvora). Jedna od početne provjere kada klijenta ili čvor potvrđuje čvor jest da biste provjerili vrijednost uobičajeni naziv u polje Subject. Ovaj uobičajeni naziv ili jednog naziva u certifikati predmet zamjenski mora postojati na popisu dopuštenih uobičajena imena.

Sljedeći članak opisuje način da biste generirali certifikati nazivima predmet zamjenski (SAN): [kako dodati alternativni naziv predmeta sigurne LDAP certifikat](http://support.microsoft.com/kb/931351).

Polje Subject može sadržavati više vrijednosti, svakom mjestu s za pokretanje da biste naznačili vrsta vrijednosti. Najčešće, Inicijalizacija je "CN" za uobičajene naziv; na primjer, "CN = www.contoso.com". Moguće je i za polje Subject tako da bude prazna. Ako se popunjava alternativni naziv predmeta polje nije obavezno, mora sadržavati uobičajeni naziv certifikata i jedna stavka po predmetu drugo ime. Te su unesene kao naziv DNS vrijednosti.

Vrijednosti polja predviđene namjene certifikata mora sadržavati odgovarajuće vrijednosti, kao što su "Provjera autentičnosti poslužitelja" ili "Provjera autentičnosti klijenta".

### <a name="client-x509-certificates"></a>Klijent X.509 certifikate

Certifikati klijent ne obično izdaje ustanova za izdavanje potvrda drugih proizvođača. Umjesto toga osobno spremište na trenutnom mjestu korisnika obično sadrži klijentskih potvrda postoji smješta korijenski izdavanje, s namjenu "Provjere autentičnosti klijent". Klijent možete koristiti takve potvrde kad je potreban je međusobna provjere autentičnosti.

>[AZURE.NOTE] Sve operacije upravljanja na servis tkanina klasteru potrebna certifikata na poslužitelj. Certifikati klijent nije moguće koristiti za upravljanje.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->


## <a name="next-steps"></a>Daljnji koraci

Ovaj članak sadrži konceptualnih informacija o sigurnosti klaster. [Stvaranje klaster u Azure pomoću predloška resursima](service-fabric-cluster-creation-via-arm.md) sljedeći, ili pomoću [portala za Azure](service-fabric-cluster-creation-via-portal.md).

<!--Image references-->
[Node-to-Node]: ./media/service-fabric-cluster-security/node-to-node.png
[Client-to-Node]: ./media/service-fabric-cluster-security/client-to-node.png
