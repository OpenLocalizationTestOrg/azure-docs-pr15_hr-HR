<properties
   pageTitle="Generički LDAP poveznik | Microsoft Azure"
   description="U ovom se članku objašnjava kako konfigurirati Microsoftov generički LDAP poveznik."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="generic-ldap-connector-technical-reference"></a>Generički LDAP poveznik Tehničke reference
U ovom se članku opisuju generički LDAP poveznik. U članku odnosi se na sljedeće proizvode:

- Microsoftov Upravitelj identiteta 2016 (MIM2016)
- Upravitelj identiteta Forefront 2010 R2 (FIM2010R2)
    -   Morate koristiti hitni popravak 4.1.3671.0 ili noviji [KB3092178](https://support.microsoft.com/kb/3092178).

Poveznik za MIM2016 i FIM2010R2, nije dostupan kao preuzeti iz [Microsoftova centra za preuzimanje](http://go.microsoft.com/fwlink/?LinkId=717495).

Kada koje upućuju na IETF RFCs, ovaj dokument koristeći oblik (RFC [RFC broj] / [sekcije u dokumentu RFC]), na primjer (RFC 4512/4,3).
Dodatne informacije možete pronaći na http://tools.ietf.org/html/rfc4500 (morate zamijeniti 4500 točan broj RFC).

## <a name="overview-of-the-generic-ldap-connector"></a>Pregled generički LDAP poveznika
Generički LDAP Connector omogućuje integraciju servisa sinkronizacije s poslužiteljem v3 LDAP.

Za određene operacije i elemenata sheme, kao što su oni potrebne za izvođenje uvoz delta nije naveden u IETF RFCs. Za ove postupke podržani su samo LDAP direktorija izričito navedeno.

Iz perspektive više razine, sljedeće značajke su podržane u trenutnom izdanju sustava poveznik:

Značajka | Podrška
--- | --- |
Izvora povezanih podataka | Poveznik podržano je s LDAP v3 poslužiteljima (RFC 4510 usklađen). Ga testirano sa sljedećim: <li>Microsoft Active Directory Lightweight Directory Services (AD polja)</li><li>Microsoft Active Directory globalni katalog (AD GC)</li><li>Poslužitelj 389 direktorija</li><li>Apache imenički poslužitelj</li><li>IBM Tivoli DS</li><li>Isode direktorija</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Otvaranje DJ</li><li>Otvaranje pošte</li><li>Otvaranje LDAP (openldap.org)</li><li>Oracle (prethodno Ned) direktorija Server Enterprise Edition</li><li>RadiantOne virtualnog direktorija Server (VDS)</li><li>Ned jednog poslužitelja direktorija</li>**Najvažnije direktorija nisu podržani:** <li>Microsoft Active Directory Domain Services (AD DS) [umjesto toga koristite ugrađeni poveznik za Active Directory]</li><li>Oracle internetski imenik (OID)</li>
Scenariji   | <li>Upravljanje životni ciklus objekta</li><li>Upravljanje grupe</li><li>Upravljanje lozinkama</li>
Operacije |Na sve LDAP direktorija podržane su sljedeće postupke: <li>Potpuni uvoz</li><li>Izvoz</li>Sljedeće postupke podržani su samo na navedeni direktorija:<li>Uvoz Delta</li><li>Postavljanje lozinke, promjena lozinke</li>
Shema | <li>Shema otkrije iz sheme LDAP (RFC3673 i RFC4512/4.2)</li><li>Podržava strukturnih klase, klase aux i extensibleObject klase (RFC4512 na 4,3)</li>

### <a name="delta-import-and-password-management-support"></a>Delta uvoza i lozinku za podršku upravljanje
Podržani direktorija za uvoz Delta i upravljanje lozinkama:

- Microsoft Active Directory Lightweight Directory Services (AD polja)
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke
- Microsoft Active Directory globalni katalog (AD GC)
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke
- Poslužitelj 389 direktorija
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke i promjena
- Apache imenički poslužitelj
    - Ne podržava uvoz delta jer se taj imenik nema zapisnik stalni promjena
    - Podržava postavljanje lozinke
- IBM Tivoli DS
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke i promjena
- Isode direktorija
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke i promjena
- Novell eDirectory i NetIQ eDirectory
    - Podržava dodavanje, ažuriranje i Preimenuj postupci za uvoz delta
    - Ne podržava operacija brisanja za uvoz delta
    - Podržava postavljanje lozinke i promjena
- Otvaranje DJ
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke i promjena
- Otvaranje pošte
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke i promjena
- Otvaranje LDAP (openldap.org)
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke
    - Ne podržava Promjena lozinke
- Oracle (prethodno Ned) direktorija Server Enterprise Edition
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke i promjena
- RadiantOne virtualnog direktorija Server (VDS)
    - Morate koristiti verziju 7.1.1 ili noviji
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke i promjena
-  Ned jednog poslužitelja direktorija
    - Podržava sve operacije za uvoz delta
    - Podržava postavljanje lozinke i promjena

### <a name="prerequisites"></a>Preduvjeti
Prije korištenja poveznik pripazite da imate sljedeće na poslužitelju sinkronizacije:

- 4.5.2 za Microsoft .NET Framework ili novije verzije

### <a name="detecting-the-ldap-server"></a>Otkrivanje LDAP poslužitelj
Poveznik oslanja nakon različite tehnike za otkrivanje i prepoznavanje LDAP poslužitelj. Poveznik koristi korijenski DSE, naziv verziju dobavljača i ga pregledava sheme da biste pronašli jedinstveni objekti i atributi koje zna da postoje u određenim LDAP poslužiteljima. Ove podatke ako je pronađeno, koristi se za unaprijed ispunjavanje konfiguracija mogućnosti poveznik.

### <a name="connected-data-source-permissions"></a>Povezani dozvole za izvor podataka
Da biste obaviti uvoz i izvoz operacije objekata u direktoriju povezani, račun poveznika mora imati potrebne dozvole. Poveznik potrebne dozvole da biste mogli izvesti za pisanje, a dozvole da biste mogli uvesti za čitanje. Konfiguriranje dozvola izvesti u sklopu sučelja upravljanje direktorija cilj sam.

### <a name="ports-and-protocols"></a>Priključci i protokoli
Poveznik koristi broj priključka koji je naveden u konfiguraciji, koji se po zadanom je 389 za LDAP i 636 za LDAPS.

LDAPS, poslužite se SSL 3.0 i TLS. SSL 2.0 nije podržan, a nije moguće aktivirati.

### <a name="required-controls-and-features"></a>Kontrole i značajke
Sljedeće kontrole/značajke LDAP mora biti dostupni na poslužitelju LDAP poveznik ispravno funkcionirao:  
`1.3.6.1.4.1.4203.1.5.3`Filtri TRUE i False

Filtar True i False se često nije prijavljen kao podržava LDAP direktorija i možda prikazuju se na **Globalni stranice** u odjeljku **Obavezna značajke nije pronađen**. Koristi se za stvaranje **ili** filtara u LDAP upita, na primjer prilikom uvoza više vrsta objekta. Ako uvezete više vrsta objekta LDAP poslužitelj podržava tu značajku.

Ako koristite direktorij gdje je jedinstveni identifikator na sidrenja sljedeće mora biti dostupna (pogledajte odjeljak [Konfiguriranje sidra](#configure-anchors) u nastavku ovog članka dodatne informacije):  
`1.3.6.1.4.1.4203.1.5.1`Svi radu atributi

Ako imenik ima više objekata nego što stane u jedan poziv u direktoriju, zatim preporučuje se da biste koristili broja stranica. Za broja stranica za rad potreban vam je jedan od sljedećih mogućnosti:

**Mogućnost 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Mogućnost 2:**  
`2.16.840.1.113730.3.4.9`VLVControl  
`1.2.840.113556.1.4.473`SortControl

Ako su obje mogućnosti omogućeni u konfiguraciji poveznika, koristit će se pagedResultsControl.

`1.2.840.113556.1.4.417`ShowDeletedControl

ShowDeletedControl koristi se samo za metodu uvoza delta USNChanged da biste mogli vidjeti izbrisani objekti.

Poveznik pokušava otkriti mogućnosti izlaganje na poslužitelju. Ako nije moguće otkriti mogućnosti, upozorenje na stranici globalni u postoji poveznik svojstva. Ne LDAP poslužiteljima kontrole značajke izlaganje sve oni podržavaju i čak i ako postoji upozorenje poveznik možda raditi bez problema.

### <a name="delta-import"></a>Uvoz Delta
Uvoz Delta dostupna je samo kada otkrio direktorija za podršku. Trenutno koriste na sljedeće načine:

- LDAP Accesslog. U odjeljku [http://www.openldap.org/doc/admin24/overlays.html#Access bilježenje u zapisnik](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
- LDAP Changelog. U odjeljku [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
- Vremenska oznaka. Za eDirectory Novell/NetIQ poveznik koristi posljednjeg datuma/vremena da biste stvorili i ažurirati objekte. Novell/NetIQ eDirectory ne nudi u jednaku vrijednost znači da biste dohvatili izbrisani objekti. Tu mogućnost i može se koristiti ako nema drugih delta način uvoza aktivan LDAP poslužitelj. Ta mogućnost nije uspio uvoz izbrisanih objekata.
- USNChanged. Pročitajte sljedeće: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Nije podržano
Sljedeće značajke LDAP nisu podržani:

- LDAP referenci između poslužitelja (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Stvaranje nove poveznika
Da biste stvorili generički LDAP poveznika, u **Servis sinkronizacije** odaberite **Management Agent** i **Stvaranje**. Odaberite poveznik **generički LDAP (Microsoft)** .

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Povezivanje
Na stranici povezivanje morate navesti informacije o glavnog računala, priključak i povezivanje. Ovisno o tome koji povezivanje je odabrani, dodatne informacije mogu biti navedena u sljedećim odjeljcima.

![Povezivanje](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

- Postavka vremensko ograničenje veze koristi samo prvi veze s poslužiteljem kada otkrivanje shemi.
- Ako povezivanje je anonimno, zatim nijedno korisničko ime / koristiti web-mjesto lozinku i u okvir za potvrdu.
- Za ostale povezivanja unesite informacije ili u korisničko ime i lozinku ili odaberite certifikat.
- Ako koristite Kerberos za provjeru autentičnosti, zatim nuditi lokalni/domene korisnika.

Tekstni okvir **atribut pseudonima** koristi se za atribute definirane u shemi s RFC4522 sintakse. Ti atributi nije moguće otkriti tijekom otkrivanja sheme i poveznik mora pomoć da biste odredili te atribute. Na primjer sljedeće je potrebno koje se unose u okvir pseudonima atribut točno prepoznati atribut userCertificate kao binarni atribut:

`userCertificate;binary`

Slijedi primjer kako se tu konfiguraciju može izgledati kao što su:

![Povezivanje](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Potvrdite okvir **Uključi radu atributa u shemi** obuhvaćaju atribute stvorio poslužitelj. To obuhvaća atribute kao što su kada je stvorena i objekta zadnje ažuriranje vremena.

Ako se koriste extensible objekte (RFC4512 na 4,3) i omogućivanje ta mogućnost omogućuje svaki atribut koja će se koristiti za sve objekt, odaberite **Uključi extensible atributa u shemi** . Tom se mogućnošću čini u shemi vrlo velike pa osim ako povezanog direktorija je za korištenje ove značajke za preporuke da biste zadržali mogućnost nije odabrano.

### <a name="global-parameters"></a>Globalni parametara
Na stranici globalni parametre možete konfigurirati DN Evidencija promjena delta i dodatne značajke LDAP. Stranica s informacijama koje ste dobili od LDAP poslužitelj je prethodno popunjen.

![Povezivanje](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

U gornjem dijelu prikazuje informacije koje ste dobili od poslužitelja odvojeno, kao što su naziv poslužitelja. Poveznik i provjerava jesu li obavezna kontrola koje su prisutne u korijen DSE. Ako nisu navedeni te kontrole, prikazat će se upozorenje. Neke direktorija LDAP popis svih značajki u korijen DSE i moguće je da se poveznik funkcionira bez problema čak i ako postoji upozorenje.

Potvrdni okviri za **podržane kontrole** nadzire ponašanje za određene operacije:

- S odabranom mogućnosti Izbriši stabla, hijerarhije izbriše s jednog LDAP poziv. Pomoću stabla Izbriši nije odabrano, poveznik ne rekurzivne Izbriši po potrebi.
- Uz odabran je Straničeno rezultate, poveznik ne Straničeno uvoz s veličinom navedenog postupka za pokretanje.
- VLVControl i SortControl je alternative pagedResultsControl za čitanje podataka iz LDAP imenika.
- Ako su tri mogućnosti (pagedResultsControl, VLVControl i SortControl) poništeni poveznik uvozi sve objekta odjednom, koje se možda neće uspjeti ako je velike imenik.
- ShowDeletedControl koristi se samo ako je način za uvoz Delta USNChanged.

Evidencija promjena DN je kontekst imenovanja koristi delta evidenciju promjena, na primjer **cn = changelog**. Ta vrijednost mora biti navedena da biste mogli učiniti delta uvoz.

Slijedi popis evidencije promjena zadanog DNs:

Direktorija | Evidencija promjena Delta
--- | ---
Microsoft AD polja i AD GC | Automatski otkriven. USNChanged.
Apache imenički poslužitelj | Nije dostupno.
Direktorija 389 | Evidencija promjena. Zadana vrijednost da biste koristili: **cn = changelog**
IBM Tivoli DS | Evidencija promjena. Zadana vrijednost da biste koristili: **cn = changelog**
Isode direktorija | Evidencija promjena. Zadana vrijednost da biste koristili: **cn = changelog**
Novell/NetIQ eDirectory | Nije dostupno. Vremenska oznaka. Poveznik koristi zadnje ažuriranje datuma/vremena da biste dodali i ažurirati zapise.
Otvaranje DJ/DS | Evidencija promjena.  Zadana vrijednost da biste koristili: **cn = changelog**
Otvaranje LDAP | Pristup zapisnik. Zadana vrijednost da biste koristili: **cn = accesslog**
Oracle DSEE | Evidencija promjena. Zadana vrijednost da biste koristili: **cn = changelog**
RadiantOne VDS | Virtualni direktorij. Ovisi o direktorija povezan s VDS.
Ned jednog poslužitelja direktorija | Evidencija promjena. Zadana vrijednost da biste koristili: **cn = changelog**

Atribut lozinka je naziv atributa poveznik trebali biste koristiti da biste postavili lozinku u Promjena lozinke i lozinke postavite operacije.
Ta vrijednost je po zadanom postavljena na **userPassword** , ali mogu se promijeniti kada je potrebno za određeni LDAP sustav.

Na popisu dodatne particije moguće je da biste dodali dodatne prostora naziva automatski nije otkriven. Tu postavku, na primjer, može se koristiti ako nekoliko poslužitelja čine logičke klaster koje treba sve uvesti u isto vrijeme. Baš kao i servisa Active Directory u jedan skup stabala možete imati više domena, ali sve domene zajedničko korištenje jedne sheme, možete isti Simulirani tako da unesete dodatne prostori u tom okviru. Svaki prostora za naziv možete uvesti iz različitih poslužitelja i dodatne konfiguriran na stranici Konfiguracija particije i hijerarhije. Koristite kombinaciju tipki Ctrl + Enter da biste dobili novi redak.

### <a name="configure-provisioning-hierarchy"></a>Konfiguriranje dodjele resursa hijerarhije
Ova stranica koristi se za mapiranje DN komponente, na primjer OU vrsti objekta koji mora biti dodijeljena, primjerice organizationalUnit.

![Dodjeljivanje hijerarhije](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Konfiguriranjem dodjele resursa hijerarhiju možete konfigurirati poveznik za automatsko stvaranje strukture po potrebi. Na primjer, ako je polje naziva kontroler = contoso, kontroler = com i novi objekt cn = Ivane, ou = Seattle, c = US, kontroler = contoso, kontroler = com je dodijeljen, a zatim poveznik objekt možete stvoriti vrstu država za SAD-a i na organizationalUnit za Seattle ako one nisu već postoji u direktoriju.

### <a name="configure-partitions-and-hierarchies"></a>Konfiguriranje particije i hijerarhije
Na stranici particije i hijerarhije odaberite Svi se prostori naziva s objektima namjeravate uvoz i izvoz.

![Particije](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Za svaki prostor naziva i moguće je da biste konfigurirali postavke povezivanja koje želite odbaciti vrijednosti navedene na zaslonu za povezivanje. Ako te vrijednosti ostaju njihovoj zadane vrijednosti prazno, koristi se informacije na zaslonu za povezivanje.

I moguće je da biste odabrali koje spremnika i organizacijske jedinice poveznik treba uvoz i izvoz.

### <a name="configure-anchors"></a>Konfiguriranje sidra
Ova stranica uvijek imati zadanu vrijednost i ne može se promijeniti. Ako otkrio dobavljača poslužitelja u sidrenja možda unose s immutable atribut, primjerice GUID za objekt. Ako nije otkriven ili poznato je da biste onemogućili immutable atribut, poveznik dn (Razlikovni naziv) koristi kao na Sidrište.

![sidra](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)

Slijedi popis LDAP poslužiteljima i sidro koristi:

Direktorija | Sidrenje atributa
--- | ---
Microsoft AD polja i AD GC | objectGUID
Poslužitelj 389 direktorija | dn
Apache direktorija | dn
IBM Tivoli DS | dn
Isode direktorija | dn
Novell/NetIQ eDirectory | GUID
Otvaranje DJ/DS | dn
Otvaranje LDAP | dn
Oracle ODSEE | dn
RadiantOne VDS | dn
Ned jednog poslužitelja direktorija | dn

## <a name="other-notes"></a>Napomene
Ovo poglavlje sadrži informacije aspekti koje su specifične za ovaj poveznik ili drugih razloga su važne informacije.

### <a name="delta-import"></a>Uvoz Delta
Vodeni žig delta u Otvori LDAP je UTC datuma/vremena. Zbog toga, morate sinkronizirati satovi između servisa sinkronizacije FIM i otvaranje LDAP. Ako nije, možda će ispušteni neke stavke u delta evidencije promjena.

Za Novell eDirectory uvoz delta nije pronađen briše sve objekte. Zbog toga je potrebno za izvođenje potpunog uvoza povremeno da biste pronašli sve izbrisane objekte.

Za direktorija s Evidencija promjena delta koji se temelji na datuma/vremena, preporučljivo je da biste pokrenuli potpuni uvoz periodičku vrijeme. Ovaj postupak omogućuje modul sinkronizaciju da biste pronašli i dissimilarities između LDAP poslužitelj i što je trenutno u prostoru poveznik.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

-   Informacije o omogućivanju zapisivanja poveznik za otklanjanje poteškoća s potražite u članku [kako omogućiti praćenje ETW za poveznika](http://go.microsoft.com/fwlink/?LinkId=335731).
