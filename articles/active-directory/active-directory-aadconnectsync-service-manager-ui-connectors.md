<properties
    pageTitle="Azure AD Connect sinkronizacije: Upravitelj sinkronizacije servisa korisničkog Sučelja | Microsoft Azure"
    description="Objašnjenje kartici poveznika u Upravitelj servisa sinkronizacije za Azure AD Connect."
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
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD Connect sinkronizacije: Upravitelj servisa za sinkronizaciju

[Operacije](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Poveznika](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Dizajner Metaverse](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Pretraživanje Metaverse](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Na kartici poveznika služi za upravljanje svim sustavima modula za sinkroniziranje je povezano s.

## <a name="connector-actions"></a>Poveznik akcije

Akcija | Komentar
--- | ---
Stvaranje | Nemojte koristiti. Povezivanje s dodatnim AD šuma, koristite čarobnjak za instalaciju.
Svojstva | Koristi se za domenu i OU filtriranje.
[Brisanje](#delete) | Koristi za ili izbrisati podatke u prostoru poveznika ili da biste izbrisali vezu na skup stabala.
[Konfiguriranje izvođenja profila](#configure-run-profiles) | Osim domena filtriranja, ništa da biste konfigurirali ovdje. Koristite ovu akciju da biste vidjeli već konfiguriran izvođenja profila.
Pokreni | Koristi se za pokretanje jednokratni Pokreni profila.
Zaustavi | Zaustavlja poveznik trenutno izvode profil.
Poveznik za izvoz | Nemojte koristiti.
Poveznik za uvoz | Nemojte koristiti.
Poveznik za ažuriranje | Nemojte koristiti.
Osvježi shemu | Osvježava predmemorirani shemu. Je Preferirani umjesto toga koristite mogućnost u čarobnjaku za instalaciju od koje ažuriranja sinkronizirati i pravila.
[Prostor poveznik za pretraživanje](#search-connector-space) | Koristi da biste pronašli objekte i [praćenje objekta i njegove podatke u sustavu](#follow-an-object-and-its-data-through-the-system).

### <a name="delete"></a>Brisanje
Akcija brisanja koristi se za dvije različite stvari.
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

Mogućnost **izbrišite poveznik razmak samo** uklanja sve podatke, ali imajte konfiguracije.

Mogućnost **Izbriši poveznika i poveznik prostora** uklanja podatke i konfiguracije. Ova mogućnost koristi se kada ne želite povezati s skupa stabala više.

Obje mogućnosti sinkronizaciju svih objekata i ažurirati metaverse objekte. Ova akcija je dugo izvodi operacija.

### <a name="configure-run-profiles"></a>Konfiguriranje izvođenja profila
Ta mogućnost omogućuje potražite u članku pokretanje profilima konfigurirana za poveznik.

![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Prostor poveznik za pretraživanje
Akciju pretraživanje poveznik prostora koristan je da biste pronašli objekte i otklanjanje poteškoća s podacima.

![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Započnite odabirom **opsega**. Možete pretraživati na temelju podataka (RDN, DN, sidro, podređenu stabla), ili stanja objekta (sve ostale mogućnosti).  
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Ako, primjerice učinite podređenu stabla pretraživanja, jedan parametra OU se sve objekte.
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png) s ovu rešetku možete odabrati programa objekta, odaberite **Svojstva**, a [iza njega](#follow-an-object-and-its-data-through-the-system) iz poveznik prostora izvor kroz na metaverse i poveznik prostor cilj.

## <a name="follow-an-object-and-its-data-through-the-system"></a>Praćenje objekta i njegove podatke u sustavu
Kada su otklanjanje problema s podacima, slijedite neki objekt iz izvorišnog poveznik prostora na metaverse i ciljnoj je prostor poveznik ključa postupak da biste shvatili Zašto podaci imati očekivanim vrijednostima.

### <a name="connector-space-object-properties"></a>Svojstva objekta prostora poveznika
**Uvoz**  
Kada otvorite objekt programa cs, postoji nekoliko kartica pri vrhu. Kartica **Uvoz** prikazuje podatke koje je kopirana bez postavljanja nakon uvoza.
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png) **Staru vrijednost** pokazuje što je pohranjena u sustavu i **Novu vrijednost** što zaprimljeni iz izvorišnog sustava, a još nije primijenjena. U ovom slučaju jer postoji pogreška sinkronizacije, promjenu nije moguće primijeniti.

**Pogreška**  
Stranica s pogreškom vidljiv samo ako je došlo je do problema s objektom. Da biste vidjeli detalje na stranici Postupci za dodatne informacije o tome kako [Otklanjanje poteškoća sa sinkronizacijom](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab).

**O podrijetlu**  
Na kartici o podrijetlu prikazuje kakvom objekt poveznik prostora na metaverse objekt. Možete vidjeti kada poveznik zadnje uvezeni promjena iz povezanih sustav i koje se pravila primjenjuju za popunjavanje podataka u na metaverse.
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png) u stupcu **Akcija** možete vidjeti postoji jedno pravilo **ulazni** sinkronizaciju s akcijom **dodjele resursa**. Koji upućuje na to da dok god postoji objekt prostora poveznik metaverse objekt ostane. Ako na popisu pravila za sinkronizaciju umjesto prikazuje pravilo sinkronizaciju s smjer **Izlazni** i **dodjele resursa**, znači da se taj objekt izbrisali nakon brisanja objekta metaverse.
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png) možete vidjeti u stupcu **PasswordSync** dolazni Konektor prostora mogu pridonositi promjene lozinke jer jedno pravilo za sinkronizaciju ima vrijednost **True**. Ovu lozinku pa se šalje Azure AD putem izlaznog pravilo.

Na kartici o podrijetlu možete pristupiti na metaverse tako da kliknete [Svojstva objekta Metaverse](#metaverse-object-properties).

Pri dnu svih kartica su dva gumba: **Pretpregled** i **zapisnika**.

**Pretpregled**  
Pretpregled stranice koristi se za sinkronizaciju jedan jedan objekt. Korisno je ako su neka pravila za sinkronizaciju klijenta za otklanjanje poteškoća i želite da biste vidjeli promjene na jedan objekt. Na raspolaganju su vam **Potpunu sinkronizaciju** i **Delta sinkronizacije**. Možete odabrati i između **Generiranje pretpregleda**, koji čuva samo promjena u memoriji, i **Izvršavanje pretpregled**, koje sve faze mijenja se u ciljnom poveznik razmake.
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png) možete provjeriti i koje se pravilo primjenjuje za određeni atribut tijek objekta.
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**Zapisnik**  
Stranica zapisnika koristi se da biste vidjeli status sinkronizacije lozinku i povijest, dodatne informacije potražite u članku [sinkronizaciju lozinke za otklanjanje poteškoća s](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization) .

### <a name="metaverse-object-properties"></a>Svojstva objekta Metaverse
**Atributi**  
Na kartici atribute vrijednosti možete vidjeti i koje poveznik pridonio ga.
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
**poveznika**  
Na kartici poveznika prikazuje sve razmake Konektor čiji prikaz objekta.
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png) Ta kartica omogućuje vam i da biste došli do [poveznik prostor objekta](#connector-space-object-properties).

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o konfiguraciji [Azure AD Connect sinkronizirati](active-directory-aadconnectsync-whatis.md) .

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
