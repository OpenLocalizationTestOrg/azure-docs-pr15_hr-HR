<properties
    pageTitle="Azure AD Connect sinkronizacije: Upravitelj sinkronizacije servisa korisničkog Sučelja | Microsoft Azure"
    description="Objašnjenje kartici operacije u Upravitelj servisa sinkronizacije za Azure AD Connect."
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

![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

Na kartici operacije prikazuje rezultate iz najnovije operacije. Ova kartica je ključ za razumijevanje i otklanjanje poteškoća.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Razumijevanje informacija koje su vidljive na kartici operacije
U gornjem dijelu prikazuje sve se pokreće chronic redoslijedom. Prema zadanim postavkama zapisnika operacija čuva podatke o zadnjih sedam dana, ali tu postavku možete promijeniti pomoću [raspored](active-directory-aadconnectsync-feature-scheduler.md). Želite da biste potražili sve cilja koji prikazuje status uspješno. Možete promijeniti sortiranje tako da kliknete zaglavlja.

Stupac **Stanje** je najvažnije informacije i prikazuje najčešće loši problem za niz. Ovdje je kratak sažetak od najčešćih statusa redom prioriteta da biste istražili (gdje * označava nekoliko mogućih pogrešaka).

Status | Komentar
--- | ---
zaustavljen-* | Pokreni se dovršiti. Na primjer, ako je udaljeni sustav nije dostupno, a ne možete uspostaviti.
zaustavljen pogreške – granice | Nema više od 5000 pogreške. Pokreni automatski je zaustavljeno zbog velikog broja pogrešaka.
dovršeni -\*-pogreške | Pokreni dovršeno, ali postoje pogreške (manje od 5000) koje treba imaju.
dovršeni -\*-upozorenja | Pokreni dovršen, ali neke podatke nije u očekivanom stanju. Ako imate pogrešaka, zatim ova poruka je obično samo simptoma. Dok ste riješili pogreške, ne treba istražiti upozorenja.
uspjeh | Nema problema.

Kada odaberete redak, u donjem se ažurira da biste prikazali detalje o koji se izvode. Na lijevoj strani donjeg, možda ćete imati popis pričaju **korak #**. Ovaj popis prikazuje se samo ako imate više domena u svoje skupa stabala gdje svaku domenu predstavlja korak. Naziv domene možete pronaći pod naslovom **particije**. U odjeljku **Statistiku sinkronizacije**, možete pronaći dodatne informacije o broju promjene koje su obrađeni. Možete kliknuti veze da biste dobili popis promijenjene objekata. Ako imate objekte s pogreškama, te pogreške se prikazivati u odjeljku **Pogreške u sinkronizaciji**.

## <a name="troubleshoot-errors-in-operations-tab"></a>Otklanjanje poteškoća u operacije kartica
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
Kada postoje pogreške, oba objekt u pogreške i pogreške samog su veze koje pruža dodatne informacije.

Najprije kliknite niz pogreške (**sinkronizaciju-pravilo-pogreške – funkcija – koji se prikazuje** na slici). Najprije predstavljanja s pregledom objekta. Da biste vidjeli stvarni pogreške, kliknite gumb **Praćenje stoga**. U ovom praćenje pruža ispravljanje pogrešaka razine podatke o pogrešku.

**Savjet:** Možete desnom tipkom miša kliknite okvir **poziva informacije o stogu** , **Odaberite sve**, a **kopija**. Zatim možete kopirati stog i pogledati pogrešku u uređivač omiljene, kao što je blok za pisanje.

- Ako je pogreška s **SyncRulesEngine**, zatim informacije o stogu poziv najprije sadrži popis svih atributa na objekt. Pomaknite se prema dolje dok ne ugledate naslov **InnerException = >**.  
![Upravitelj servisa za sinkronizaciju](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
Redak nakon prikazuje se pogreška. Na gornjoj slici, pogreška je iz na prilagođenu sinkronizaciju pravilo Fabrikam stvorili.

Ako pogreške sam dati dovoljno informacija, pa je vrijeme možete pogledati i same podatke. Možete kliknuti vezu s identifikator objekta i [praćenje objekta i njegove podatke u sustavu](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system).

## <a name="next-steps"></a>Daljnji koraci
Dodatne informacije o konfiguraciji [Azure AD Connect sinkronizirati](active-directory-aadconnectsync-whatis.md) .

Dodatne informacije o [integraciji vaših lokalnih identiteta sa servisu Azure Active Directory](active-directory-aadconnect.md).
