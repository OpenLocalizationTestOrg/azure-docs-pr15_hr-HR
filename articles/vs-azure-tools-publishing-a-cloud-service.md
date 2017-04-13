<properties
   pageTitle="Objavljivanje na servis u Oblaku pomoću alata za Azure | Microsoft Azure"
   description="Saznajte kako objaviti projekata servisa Azure oblak pomoću Visual Studio."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="publishing-a-cloud-service-using-the-azure-tools"></a>Objavljivanje na servis u Oblaku pomoću alata za Azure

Pomoću alata za Azure za Microsoft Visual Studio, možete objaviti Azure aplikacije izravno iz Visual Studio. Visual Studio podržava integrirani objavljivanje na pripremna ili okruženje proizvodnje za servise u oblaku.

Prije objavljivanja Azure aplikacije, morate imati pretplatu na Azure. Morate postaviti oblaka račun i servis za pohranu se može koristiti u aplikaciji. Možete postaviti te pri [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

>[AZURE.IMPORTANT] Kad objavite, možete odabrati okruženje za implementaciju za servis u oblaku. Morate odabrati i račun za pohranu koji se koristi za pohranu Aplikacijski paket za implementaciju. Nakon implementacije, Aplikacijski paket uklanja se s računa za pohranu.

Kada razvoja i testiranja Azure aplikacije implementacija Web možete koristiti da biste postupno objavite promjene za svoje uloge web. Nakon objavljivanja aplikaciju okruženje za implementaciju, implementacija Web možete izravno implementirati promjene virtualnog računala sa sustavom ulogu web. Ne morate pakiranje i objaviti cijelu Azure program svaki put kada želite da biste ažurirali vaša uloga web da biste testirajte promjene. Na taj se način imate dostupne web uloga promjene u oblaku za testiranje bez čekanja da bi se objavljuju u okruženje za implementaciju aplikacije.

Da biste objavili Azure aplikacije i da biste ažurirali web uloga pomoću implementacija Web, koristite sljedeće postupke:

- Objavljivanje ili pakiranje Azure aplikacije Visual Studio

- Ažuriranje web uloga kao dio razvoja i testiranja ciklus

## <a name="publish-or-package-an-azure-application-from-visual-studio"></a>Objavljivanje ili pakiranje Azure aplikacije Visual Studio

Kad objavite svoju aplikaciju za Azure, možete učiniti nešto od sljedećeg:

- Stvaranje paketa za servis: možete koristiti taj paket i konfiguracijska datoteka servisa da biste objavili vaše aplikacije u okruženju implementacije [Azure klasični Portal](http://go.microsoft.com/fwlink/?LinkID=213885).

- Objavite projekt Azure s Visual Studio: da biste objavili izravno Azure aplikaciju, upotrijebite čarobnjak za objavljivanje. Informacije potražite u članku [Objavljivanje čarobnjak aplikacije za Azure](vs-azure-tools-publish-azure-application-wizard.md).

### <a name="to-create-a-service-package-from-visual-studio"></a>Stvaranje servisnog paketa iz Visual Studio

1. Kada budete spremni objaviti svoju aplikaciju, otvorite Eksplorer za rješenja, otvorite izbornik prečaca za Azure projekt koji sadrži vašim ulogama i odaberite Objavi.

1. Da biste stvorili samo paket servisa, slijedite ove korake:  

  1. Na izborniku prečaca za Azure projekt odaberite **paketa**.

  1. U dijaloškom okviru **Paket Azure aplikacije** odaberite konfiguracija servisa za koji želite pakirati, a zatim konfiguracije Sastavi.

  1. (neobavezno) Da biste uključili udaljene radne površine za servis u oblaku nakon objavljivanja, potvrdite okvir **Omogući udaljene radne površine za sve uloge** , a zatim odaberite **Postavke** da biste konfigurirali udaljene radne površine. Upute za ispravljanje pogrešaka servis u oblaku nakon objavljivanja, uključite ispravljanje pogrešaka udaljene tako da odaberete **Omogućivanje udaljene ispravljanje pogrešaka za sve uloge**.

      Dodatne informacije potražite u članku [Korištenje udaljenu radnu površinu s Azure uloge](vs-azure-tools-remote-desktop-roles.md).

  1. Da biste stvorili paket, odaberite vezu **paketa** .

      Eksplorer za datoteke prikazuje mjesto datoteke novostvorenu paketa. To mjesto možete kopirati tako da ga možete koristiti s [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

  1. Da biste objavili taj paket u okruženje za implementaciju, morate koristiti to mjesto kao mjesto paketa pri stvaranju servis u oblaku i implementacija paket okruženju s [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Neobavezno) Da biste prekinuli postupak implementacije, na izborniku prečaca za stavku retka u zapisnik aktivnosti, odaberite **Odustani i ukloniti**. Time se zaustavlja se postupak implementacije i briše okruženje za implementaciju s Azure.

    >[AZURE.NOTE] Da biste uklonili okruženje za implementaciju kada je uveden, morate koristiti [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Neobavezno) Nakon što ste pokrenuli vaša uloga instance, Visual Studio automatski prikazuje okruženje za implementaciju u čvor **Servise u Oblaku** u programu Explorer poslužitelja. Na tom mjestu možete vidjeti status instanci pojedinačne uloge. Potražite [resurse za upravljanje Azure pomoću programa Explorer oblaka](vs-azure-tools-resources-managing-with-cloud-explorer.md). Sljedeća ilustracija prikazuje instance uloga dok su i dalje u stanju Initializing:

    ![VST_DeployComputeNode](./media/vs-azure-tools-publishing-a-cloud-service/IC744134.png)

## <a name="update-a-web-role-as-part-of-the-development-and-testing-cycle"></a>Ažuriranje Web uloga kao dio razvoja i testiranja ciklusa

Ako aplikaciju programa pozadinskog Infrastruktura je stabilan, ali uloge web potrebno više čestih ažuriranja, implementacija Web možete koristiti da biste ažurirali samo web uloga u projektu. To je korisno kada ne želite da biste ponovno stvaranje i ponovno implementirate uloge suradnika pozadinskog ili ako imate više uloge web i želite ažurirati samo jedno od uloge web.

### <a name="requirements"></a>Preduvjeti

Ovo su sistemski preduvjeti za korištenje implementacija Web da biste ažurirali vaša uloga web:

- **Za razvoj i testiranje potrebe samo:** Promjena izravno u virtualnog računala kojem se pokreće ulogu web. Ako je ovo virtualnog računala mora biti brisanja, promjene gube jer izvorni paket koji ste objavili služi za ponovno stvaranje virtualnog računala uloge. Morate ponovno objavite aplikaciju da biste dobili najnovije promjene za ulogu web.

- **Se može ažurirati samo web uloge:** Nije moguće ažurirati uloge suradnika. Osim toga, ne možete ažurirati RoleEntryPoint u web role.cs.

- **Može podržavati instancu web uloge:** Ne možete imati više instanci sve uloge web okruženje za implementaciju. Međutim, više web uloge svaki s samo jedna instanca podržane.

- **Morate omogućiti udaljene radne površine veze:** Ovo je obavezan da bi implementacija Web mogli koristiti na korisnika i lozinku za povezivanje s virtualnog računala za implementaciju promjene na poslužitelj sa sustavom Internet Information Services (IIS). Uz to, možda ćete morati povezati virtualnog računala da biste dodali pouzdana ustanova IIS na ovom računalu virtualne. (Na taj način udaljene IIS koji koriste implementacija Web sigurna.)

Sljedeći postupak pretpostavlja da koristite čarobnjak za **Objavljivanje Azure aplikacije** .

### <a name="to-enable-web-deploy-when-you-publish-your-application"></a>Da biste omogućili Web implementacija Kad objavite svoju aplikaciju

1. Da biste omogućili na **Omogućivanje implementacija Web** za sve web uloge, već morate konfigurirati udaljene radne površine veze. Odaberite **Omogući udaljene radne površine** za sve uloge, a zatim unesite vjerodajnice koje će se koristiti za povezivanje daljinski u okvir za **Konfiguraciju udaljene radne površine** koja će se prikazati. Dodatne informacije potražite u članku [Korištenje udaljenu radnu površinu s Azure uloge](vs-azure-tools-remote-desktop-roles.md) .

1. Da biste omogućili implementacija Web za sve uloge za web u aplikaciji, odaberite **Omogući Web implementacije svih uloga web**.

    Pojavit će se žutim trokutom upozorenja. Uvođenje web koristi se nepouzdanih, samopotpisani certifikat prema zadanim postavkama ne preporučuje se za prijenos povjerljive podatke. Ako vam je potrebna sigurne ovaj postupak za povjerljive podatke, možete dodati SSL certifikat koja će se koristiti za implementaciju Web veze. Taj certifikat mora biti pouzdanih certifikata. Informacije o tome kako to učiniti potražite u odjeljku **Da biste provjerite Web implementacije Secure** u nastavku ovog članka.

1. Odaberite **Dalje** da biste prikazali **Sažetak** zaslona, a zatim odaberite **Objavi** za implementaciju servisa u oblaku.

    Objavljeno je servis u oblaku. Virtualnog računala koja je stvorena je daljinske veze omogućen za IIS tako da se implementacija Web može se koristiti za ažuriranje vašim ulogama web bez ponovno objavljivanje ih.

    >[AZURE.NOTE] Ako imate više od jedne instance konfigurirana za web uloge, pojavljuje se poruka upozorenja koja govori ulogama web bit će ograničeni na jednu instancu samo u paket koji je stvoren za objavljivanje aplikacija. Odaberite **u redu** da biste nastavili. Prema uputama u odjeljku preduvjeti, možete imati više uloga web ali samo jedna pojava svaku ulogu.

### <a name="to-update-your-web-role-by-using-web-deploy"></a>Da biste ažurirali vaša uloga Web pomoću implementacija Web

1. Da biste koristili implementacija Web, promijenite kod projekta za bilo koju od vašim web ulogama u Visual Studio koje želite objaviti, pa desnom tipkom miša kliknite čvor projekta u rješenje i pokažite na **Objavi**. Pojavit će se dijaloški okvir **Objavi Web** .

1. (Neobavezno) Ako ste dodali pouzdano SSL certifikat koji želite koristiti za udaljene veze za IIS, možete isključiti potvrdni okvir **Dopusti nepouzdanih certifikata** . Informacije o dodavanju potvrdu da bi Web implementacije sigurno potražite u odjeljku **Da biste provjerite Web implementacije sigurno** u nastavku ovog članka.

1. Da biste koristili implementacija Web, mehanizam za objavljivanje mora korisničko ime i lozinka koje ste postavili za udaljene radne površine prilikom objavljivanja paketa.

  1. U odjeljak **korisničko ime**unesite korisničko ime.

  1. U okvir **Lozinka**unesite lozinku.

  1. (Neobavezno) Ako želite Spremi ovu lozinku u ovaj profil, odaberite **Spremi lozinku**.

1. Da biste objavili promjene na vaša uloga web, odaberite **Objavi**.

    Statusnoj traci prikazuje **rada Objavi**. Kada objavljivanje dovrši, **Objavi uspjela** prikazat će se. Promjene sada uveden ulogu web na virtualnog računala. Sada možete pokrenuti Azure aplikacije u okruženju Azure da biste testirali promjene.

### <a name="to-make-web-deploy-secure"></a>Da biste se zaštitili Web implementacije

1. Uvođenje web koristi se nepouzdanih, samopotpisani certifikat prema zadanim postavkama ne preporučuje se za prijenos povjerljive podatke. Ako vam je potrebna sigurne ovaj postupak za povjerljive podatke, možete dodati SSL certifikat koja će se koristiti za implementaciju Web veze. Taj certifikat mora biti pouzdanih certifikata koji ste dobili od ustanove za izdavanje certifikata (CA).

    Da biste implementacija Web sigurne za svaki virtualnog računala za svaku vaša uloga web, morate prenijeti pouzdanih certifikat koji želite koristiti za web-mjesto implementacija [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885). Time ćete dodati certifikat virtualnog računala koja je stvorena za ulogu web Kad objavite svoju aplikaciju.

1. Da biste dodali pouzdano SSL certifikat IIS da biste koristili za udaljene veze, slijedite ove korake:

  1. Da biste povezali virtualnog računala sa sustavom ulogu web, odaberite instancu sustava web uloge u **Oblak Explorer** ili **Explorer poslužitelja**, a zatim naredbe **Poveži putem udaljene radne površine** . Detaljne upute o povezivanju virtualnog računala potražite u članku [Korištenje udaljenu radnu površinu s Azure uloge](vs-azure-tools-remote-desktop-roles.md).

      Vaš preglednik će vas da biste preuzeli programa. RDP datoteka.

  1. Da biste dodali SSL certifikata, otvorite servisa za upravljanje u upravitelju IIS. U upravitelju IIS Omogući SSL tako da otvorite vezu **povezivanja** u oknu **Akcije** . Pojavit će se dijaloški okvir **Dodavanje povezivanje na web-mjesta** . Odaberite **Dodaj**, a zatim na padajućem popisu **Vrsta** odaberite HTTPS. Na popisu **SSL certifikat** odaberite SSL certifikat koji ste potpisao izdavanje certifikata i prenijeli [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885). Dodatne informacije potražite u članku [Konfiguriranje postavki veze servisa za upravljanje](http://go.microsoft.com/fwlink/?LinkId=215824).

      >[AZURE.NOTE] Ako dodate pouzdanih SSL certifikat žutim trokutom upozorenja više se ne pojavljuje u **Čarobnjak za objavljivanje**.

## <a name="include-files-in-the-service-package"></a>Umetanje datoteke u paket servisa

Možda ćete morati uključiti određene datoteke u vašeg paketa da bi bili dostupni na virtualnog računala koji je stvoren za uloge. Ako, na primjer, možda ćete morati dodati programa .exe ili .msi datoteku koja se koristi pri pokretanju skripta za vašeg paketa. Ili, možda ćete morati dodati programa skupa koji zahtijeva web ulogu ili tempiranja uloga projekta. Da biste dodali datoteke mora biti dodan rješenje Azure aplikacije.

### <a name="to-include-files-in-the-service-package"></a>Da biste dodali datoteke u paket servisa

1. Da biste dodali u sklopu servisa pakirati, poduzmite sljedeće korake:

  1. U **Pregledniku rješenja** otvorite čvor projekta za projekt koji nedostaju referentne skupa.

  1. Da biste dodali u sklopu projekta, otvorite izbornički prečac za mapu **reference** , a zatim odaberite **Dodaj referencu**. Pojavit će se dijaloški okvir Dodavanje referencu.

  1. Odaberite referencu koju želite dodati, a zatim odaberite gumb **u redu** .

      Reference se dodaje na popis u mapi **referenci** .

  1. Otvaranje izbornika prečaca za sastavljanje koju ste dodali, a zatim odaberite **Svojstva**. Pojavit će se prozor **Svojstva** .

      Da biste uključili ovaj skup u paketu servisa **lokalnu kopiju popisa** odaberite **True**.

1. U **Pregledniku rješenja** otvorite čvor projekta za projekt koji nedostaju referentne skupa.

1. Da biste dodali u sklopu projekta, otvorite izbornički prečac za mapu **reference** , a zatim odaberite **Dodaj referencu**. Pojavit će se dijaloški okvir **Dodavanje referencu** .

1. Odaberite referencu koju želite dodati, a zatim odaberite gumb **u redu** .

    Reference se dodaje na popis u mapi **referenci** .

1. Otvaranje izbornika prečaca za sastavljanje koju ste dodali, a zatim odaberite **Svojstva**. Pojavit će se prozor svojstva.

1. Da biste dodali ovaj skup u paket servisa, na popisu **Lokalnu kopiju** odaberite **True**.

1. Da biste dodali datoteke u paket servisa koji su dodani u projekt uloga web, otvorite izbornički prečac za datoteku pa odaberite **Svojstva**. U prozoru **Svojstva** odaberite **sadržaja** iz okvira popisa **Sastavljanje akcija** .

1. Da biste dodali datoteke u paket servisa koji su dodani u projekt ulogu suradnika, otvorite izbornički prečac za datoteku pa odaberite **Svojstva**. U prozoru **Svojstva** odaberite **kopirati ako novija** iz okvira popisa **Kopiraj izlaznog direktorija** .

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o objavljivanju za Azure s Visual Studio, potražite u članku [Objavljivanje čarobnjak aplikacije za Azure](vs-azure-tools-publish-azure-application-wizard.md).
