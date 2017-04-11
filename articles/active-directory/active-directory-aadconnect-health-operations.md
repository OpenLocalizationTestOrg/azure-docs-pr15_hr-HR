<properties
    pageTitle="Azure AD Connect stanja operacije."
    description="U ovom se članku opisuju dodatne operacije može izvoditi kada ste implementiran Azure AD povežite zdravlje."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="azure-ad-connect-health-operations"></a>Stanje postupci za Azure AD Connect

Sljedeće se temi opisuju razne operacije koje možete izvršiti pomoću Azure AD povezivanje stanja.

## <a name="enable-email-notifications"></a>Omogućivanje obavijesti e-poštom
Možete konfigurirati Azure AD povezivanje zdravlje usluge za slanje obavijesti e-poštom kada upozorenja generiraju koji označava infrastruktura za identitet nije dobar. To će se dogoditi kada se generira upozorenja, kao i kada je označena kao riješen. Slijedite upute u nastavku da biste konfigurirali obavijesti e-poštom.

![Stanje e-pošte za Azure AD Connect otkrijte obavijesti](./media/active-directory-aadconnect-health/email_noti_discover.png)

>[AZURE.NOTE] Po zadanom su onemogućeni obavijesti e-poštom.


### <a name="to-enable-azure-ad-connect-health-email-notifications"></a>Da biste omogućili Azure AD povežite zdravlje obavijesti e-poštom

1. Otvorite plohu upozorenja za servis za koju želite primati obavijesti e-pošte.
2. Kliknite gumb "Postavki obavijesti" s akcijske trake.
3. Uključivanje obavijesti e-poštu prebaci na Uključeno.
4. Uključite potvrdni okvir da biste konfigurirali globalni administratori za primanje obavijesti e-poštom.
5. Ako želite primati obavijesti putem e-pošte na bilo koje adrese e-pošte, možete ih odrediti u okviru Dodatne e-pošte primatelja. Da biste uklonili adresu e-pošte s popisa, desnom tipkom miša kliknite stavku, a zatim odaberite Izbriši.
6. Da biste dovršavanje promjene kliknite "Spremi". Sve promjene će se efekti tek kada odaberete "Spremi".

## <a name="delete-a-server-or-service-instance"></a>Brisanje poslužitelja ili servis instanci

### <a name="delete-a-server-from-azure-ad-connect-health-service"></a>Brisanje poslužitelju s Azure AD povezivanje servisa stanja
U nekim slučajevima želite uklanjanje poslužitelja s prate. Slijedite upute za uklanjanje poslužitelja s Azure AD povezivanje servisa stanja.

Prilikom brisanja poslužitelju, imajte na umu sljedeće:

- Time će se ZAUSTAVITI prikupljanje dodatne podatke s tog poslužitelja. Ovaj poslužitelj bit će uklonjen s servis nadzora. Nakon ove akcije neće moći da biste vidjeli nove upozorenja, nadzor ili korištenje analize podataka za ovaj poslužitelj.
- Ova akcija nije će deinstalirati ili ukloniti Health Agent s poslužitelja. Ako ne deinstalirate Health Agent prije izvođenje ovog koraka, možda ćete vidjeti događaje s pogreškom na poslužitelju koji se odnose na Health Agent.
- Ova akcija će izbrisati podatke već prikupljenih ovaj poslužitelj. Podaci će se izbrisati skladu sa pravila zadržavanja za podataka Microsoft Azure.
- Nakon izvršavanja ovu akciju, ako želite početi nadzirati na isti poslužitelj ponovno, morat ćete deinstalirati i ponovno instalirajte agent stanja na ovaj poslužitelj.


#### <a name="to-delete-a-server-from-azure-ad-connect-health-service"></a>Da biste izbrisali poslužitelju s Azure AD povezivanje servisa stanja

Azure AD Connect stanja za AD FS i Azure AD povezivanje (Sinkroniziranje):

1. Otvorite Elektronička ploča poslužitelja iz Elektronička ploča za popis poslužitelja tako da odaberete naziv poslužitelja koje će biti uklonjene.
2. Na Elektronička ploča poslužitelja kliknite gumb "Izbriši" s akcijske trake.
3. Potvrdite akciju da biste izbrisali poslužitelj tako da upišete naziv poslužitelja u okviru za potvrdu.
4. Kliknite gumb "Izbriši".

Azure AD Connect stanja za AD DS:

1. Otvorite nadzornu ploču kontrolera domena.
2. Odaberite kontrolerom domene koje će biti uklonjene.
3. Kliknite gumb "Izbriši odabrano" s akcijske trake.
4. Potvrdite akciju da biste izbrisali poslužitelj.
5. Kliknite gumb "Izbriši".

### <a name="delete-a-service-instance-from-azure-ad-connect-health-service"></a>Brisanje instanca servisa Azure AD povezivanje servisu stanja

U nekim slučajevima možda želite da biste uklonili instanca servisa. Slijedite upute u nastavku da biste uklonili instanca servisa Azure AD povezivanje servisa stanja.

Prilikom brisanja instanca servisa, imajte na umu sljedeće:

- Tom akcijom uklonit će se trenutna instanca servisa iz servisa za nadzor.
- Ova akcija nije će deinstalirati ili ukloniti Health Agent s bilo kojeg poslužitelje koji su nadzirati kao dio ovu instancu usluge. Ako ne deinstalirate Health Agent prije izvođenje ovog koraka, možda ćete vidjeti događaje s pogreškom na poslužiteljima vezane uz Health Agent.
- Svi podaci iz ovu instancu usluge izbrisat će se po pravila zadržavanja za podataka Microsoft Azure.
- Nakon izvršavanja ovu akciju, ako želite pokrenuti servis za nadzor, provjerite deinstalirati i ponovno instalirati agent za stanje na svim poslužiteljima koji je moguće nadzirati. Nakon izvršavanja ovu akciju, ako želite početi nadzirati na isti poslužitelj ponovno, morat ćete deinstalirati i ponovno instalirajte agent stanja na ovaj poslužitelj.


#### <a name="to-delete-a-service-instance-from-azure-ad-connect-health-service"></a>Da biste izbrisali instanca servisa Azure AD povezivanje servisu stanja

1. Otvaranje servisa plohu iz plohu popis servisa tako da odaberete identifikator usluge (farme ime) koji želite ukloniti.
2. Na Elektronička ploča poslužitelja kliknite gumb "Izbriši" s akcijske trake.
3. Provjerite naziv usluge upisivanjem u okviru za potvrdu. (na primjer: sts.contoso.com)
4. Kliknite gumb "Izbriši".
<br><br>


[//]: # (Start of RBAC section)
## <a name="manage-access-with-role-based-access-control"></a>Upravljanje pristupom s ulogom temelji pristup kontrola
### <a name="overview"></a>Pregled
[Kontrola pristupa temelje ulogama](role-based-access-control-configure.md) za Azure AD povežite zdravlje omogućuje pristup Azure AD povežite zdravlje usluge korisnika i/ili grupe izvan globalnog administratora. To se postiže Dodjela dozvola grupama and\or Predviđeni korisnici i njihovi mehanizma da biste ograničili globalni administratori u direktoriju.

#### <a name="roles"></a>Uloge
Povežite zdravlje Azure AD podržava sljedeće ugrađene uloge.

| Uloga | Dozvole |
| ----------- | ---------- |
| Vlasnik | Vlasnici mogu ***Upravljanje pristupom*** (npr. Dodijeli ulogu za korisnika/grupe), ***Prikaz svih podataka*** (primjerice prikaz upozorenja) portal i ***promijeniti postavke*** (npr. obavijesti e-pošte) unutar Azure AD povežite zdravlje. <br>Prema zadanim postavkama Azure AD globalni Administratori dodjeljuju ta uloga, i to nije moguće promijeniti.  |
|Suradnik|  Suradnici mogu ***prikazati sve podatke*** (npr. prikaz upozorenja) na portal i ***promijeniti postavke*** (npr. obavijesti e-pošte) unutar Azure AD povežite zdravlje.|
|Čitač| Čitatelji možete ***prikazati sve podatke*** (npr. prikaz upozorenja) na portalu unutar Azure AD povezivanje stanja.|

Sve ostale uloge (kao što su 'Korisnički pristup administratori' ili 'DevTest Labs korisnici') čak i ako je dostupna u sučelje za portala imati bez utjecaja da biste pristupili unutar Azure AD povezivanje stanja.

#### <a name="access-scope"></a>Opseg programa Access

Azure AD Connect podržava upravljanje pristupom na dvije razine:

- ***Sve instance servisa***: to je preporučena put za većinu korisnika i kontrola pristupa za sve instance servisa (npr. ADFS farma sustava) kroz sve vrste uloge koje se nadzire Azure AD povežite zdravlje.

- ***Instanca servisa***: U nekim slučajevima možda ćete morati segregate access koji se temelje na vrstama uloga ili instanca servisa. U tom slučaju možete upravljati pristupom na razini instanca servisa.  

Dozvole moguć je ako krajnji korisnik ima pristup bilo na razini imenika ili instanca servisa.


### <a name="how-to-allow-users-or-groups-access-to-azure-ad-connect-health"></a>Kako omogućiti pristup Azure AD povežite zdravlje korisnike ili grupe
#### <a name="steps-1-select-the-appropriate-access-scope"></a>Koraci 1: Odaberite odgovarajući pristup opsega
Da biste omogućili korisničkog pristupa na razini *sve instance servisa* unutar Azure AD povežite zdravlje, otvorite glavni plohu u Azure AD povežite zdravlje.<br>
#### <a name="step-2-add-users-groups-and-assign-roles"></a>Korak 2: Dodavanje korisnika, grupe i dodijelite uloge
1. Kliknite dio "Korisnici" iz odjeljka konfiguriranje.<br>
![Stanje RBAC glavni plohu za Azure AD Connect](./media/active-directory-aadconnect-health/RBAC_main_blade.png)
2. Odaberite "Dodaj"
3. Odaberite "Ulogu" kao što su "Vlasnik"<br>
![Azure AD Connect stanja RBAC Dodavanje korisnika](./media/active-directory-aadconnect-health/RBAC_add.png)
4. Unesite ime ili identifikator ciljano korisnika ili grupu. Istovremeno možete odabrati jedan ili više korisnika ili grupe. Kliknite "odaberite".
![Stanje RBAC odabir korisnika za Azure AD Connect](./media/active-directory-aadconnect-health/RBAC_select_users.png)
5. Odaberite "U redu".<br>

6. Nakon dovršetka Dodjela uloge korisnika i/ili grupe pojavit će se na popisu.<br>
![Popis korisnika RBAC stanja za Azure AD Connect](./media/active-directory-aadconnect-health/RBAC_user_list.png)

Ove korake omogućuje navedenih korisnici i grupe pristup skladu sa svojim dodijeljena uloga.
>[AZURE.NOTE]
- Globalni administratori uvijek imali puni pristup sve operacije, ali neće biti izlaganje na gornjem popisu računa globalnog administratora.
- "Pozivanje korisnika" značajka nije podržana unutar Azure AD povezivanje stanja.

#### <a name="step-3-share-the-blade-location-with-users-or-groups"></a>Korak 3: Zajedničko korištenje lokacije plohu s korisnicima ili grupama
1. Nakon Dodjela dozvola, korisnik može pristupiti Azure AD povežite zdravlje tako da [http://aka.ms/aadconnecthealth](http://aka.ms/aadconnecthealth).
2. Jednom na plohu, korisnik možete prikvačiti plohu ili različite dijelove na nadzornu ploču klikom na "Prikvači na nadzornoj ploči"<br>
![Plohu PIN-a za Azure AD RBAC stanja za povezivanje](./media/active-directory-aadconnect-health/RBAC_pin_blade.png)


>[AZURE.NOTE] Korisnik s ulogom "Čitač" dodijeljeni nećete moći izvođenje operacije "Stvaranje" da biste dobili Azure AD povežite zdravlje proširenje iz trgovine Azure Marketplace. Taj korisnik i dalje možete pristupiti na plohu tako da iznad vezu. Za kasnije korištenje korisnika možete prikvačiti plohu nadzorne ploče.

### <a name="remove-users-andor-groups"></a>Uklanjanje korisnika i/ili grupe
Možete ukloniti korisnika ili grupu dodali dio Azure AD povezivanje stanja uloga temelji kontrola pristupa tako da desnom tipkom miša kliknete, a zatim odaberete Ukloni.<br>
![Stanje RBAC Ukloni korisnika za Azure AD Connect](./media/active-directory-aadconnect-health/RBAC_remove.png)

[//]: # (End of RBAC section)

## <a name="related-links"></a>Srodne veze

* [Azure AD Connect stanja](active-directory-aadconnect-health.md)
* [Stanje Agent za instalaciju za Azure AD Connect](active-directory-aadconnect-health-agent-install.md)
* [Korištenje Azure AD povezivanje stanja AD fs](active-directory-aadconnect-health-adfs.md)
* [Korištenje stanja povezivanje Azure AD za sinkronizaciju](active-directory-aadconnect-health-sync.md)
* [Korištenje Azure AD povezivanje stanje s AD DS](active-directory-aadconnect-health-adds.md)
* [Najčešća pitanja o stanju za Azure AD Connect](active-directory-aadconnect-health-faq.md)
* [Povijest verzija stanja za Azure AD Connect](active-directory-aadconnect-health-version-history.md)
