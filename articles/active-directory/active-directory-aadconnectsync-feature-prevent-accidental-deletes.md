<properties
   pageTitle="Azure AD Connect sinkronizacije: sprječava nenamjerno brisanje | Microsoft Azure"
   description="U ovoj se temi opisuju značajci slučajne briše (sprječava nenamjerno brisanje) Onemogući Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect sinkronizacije: sprječava nenamjerno brisanje
U ovoj se temi opisuju značajci nenamjerno briše (sprječava nenamjerno brisanje) Onemogući Azure AD Connect.

Prilikom instalacije Azure AD Connect sprječava nenamjerno brisanje omogućena po zadanom i konfiguriran tako da ne dopušta izvoz s više od 500 briše. Ta značajka namijenjen je zaštititi od promjene konfiguracije slučajne i promjene lokalnog imenika koje bi utječu mnogi korisnici i druge objekte.

Uobičajeni scenariji kada se prikaže mnogo briše obuhvaćaju:

- Promjena [Filtriranje](active-directory-aadconnectsync-configure-filtering.md) gdje nije odabrana cijelu [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) ili [domene](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) .
- Brišu se svi objekti na je Organizacijska Jedinica.
- Je Organizacijska Jedinica je preimenovati tako da se svi objekti u njoj se smatraju iz opseg za sinkronizaciju.

Zadanu vrijednost od 500 objekata koje se mogu mijenjati sa servisom PowerShell pomoću `Enable-ADSyncExportDeletionThreshold`. Trebali biste konfigurirati tu vrijednost radi prilagodbe veličini vaše tvrtke ili ustanove. Budući da raspored sinkronizacije pokreće svakih 30 minuta, je vrijednost broj briše vidjeti sljedećih 30 minuta.

Ako ima previše briše kopirana bez postavljanja da biste se izvesti Azure AD, izvoz zaustavlja i primite poruku e-pošte, ovako:

![Sprječava nenamjerno brisanje e-pošte](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Pozdrav (Tehnički kontakt). (Trenutku) servis sinkronizacije identiteta otkrio broj brisanja premašuje je konfiguriran brisanja praga za (naziv tvrtke ili ustanove). Ukupno (broja) objekata poslane za brisanje u ovom identiteta sinkronizacija. Ispunjen ili premašena vrijednost praga konfigurirano brisanje objekata (broj). Potrebna koje možete unijeti potvrdu da se ove brisanja treba obrađen prije nego što će se ne možemo nastaviti. Pročitajte članak preventing slučajne brisanja dodatne informacije o pogrešci navedena u poruci e-pošte.*

Možete vidjeti i status `stopped-deletion-threshold-exceeded` kada tražite u **Upravitelj sinkronizacije servisa** korisničkog Sučelja za izvoz profil.
![Sprječava nenamjerno brisanje korisničkog Sučelja Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Ako je neočekivana, istražili i korektivne poduzeti. Da biste vidjeli koji objekti su uskoro će se izbrisati, učinite sljedeće:

1. Pokrenite **servis za sinkronizaciju** s izbornika Start.
2. Idite na **poveznike**.
3. Odaberite poveznik s vrstom **Azure Active Directory**.
4. U odjeljku **Akcije** na desnoj strani odaberite **Prostora poveznik za pretraživanje**.
5. U skočnom izborniku u odjeljku **doseg**odaberite **Prekinuta jer** , a zatim odaberite vrijeme u prošlosti. Kliknite **pretraživanje**. Ova stranica omogućuje prikaz svih objekata uskoro će se izbrisati. Klikom na svaku stavku možete dobiti dodatne informacije o objekt. Možete kliknuti i **Postavke stupca** da biste dodali dodatne atribute da budu vidljive u rešetki.

![Prostor poveznik za pretraživanje](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Po želji sve briše pa učinite sljedeće:

1. Da biste privremeno onemogućiti tu zaštitu i omogućuju te briše bude obrađen, pokrenite cmdlet komponente PowerShell: `Disable-ADSyncExportDeletionThreshold`. Navedite Azure AD globalni Administrator račun i lozinku.
![Vjerodajnice](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. S Azure Active Directory poveznik odabran, odaberite akciju **pokrenuti** , a zatim **Izvoz**.
3. Da biste ponovno Omogućivanje zaštitu, pokrenite cmdlet komponente PowerShell: `Enable-ADSyncExportDeletionThreshold`.

## <a name="next-steps"></a>Daljnji koraci

**Pregled tema**

- [Azure AD Connect sinkronizacije: razumijevanje i prilagoditi sinkronizacije](active-directory-aadconnectsync-whatis.md)
- [Integriranje sustava lokalnih identiteta sa Azure Active Directory](active-directory-aadconnect.md)
