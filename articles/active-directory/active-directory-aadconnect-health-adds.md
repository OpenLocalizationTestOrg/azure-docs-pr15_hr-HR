
<properties
    pageTitle="Korištenje Azure AD povezivanje stanje s AD DS | Microsoft Azure"
    description="To je stranica stanja povezivanje Azure AD koji će govori o praćenje AD DS."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="arluca"/>

# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Korištenje Azure AD povezivanje stanje s AD DS
Sljedeća dokumentacija je za Active Directory Domain Services s stanjem povezati Azure AD za nadzor. Su podržane verzije AD DS: Windows Server 2008 R2, Windows Server 2012 i Windows Server 2012 R2.

Dodatne informacije o AD FS s povežite zdravlje Azure AD za nadzor, potražite u članku [Korištenje Azure AD povezivanje stanja AD fs](active-directory-aadconnect-health-adfs.md). Uz to, informacije o nadzor Azure AD Connect (Sinkroniziranje) s Azure AD povežite zdravlje potražite [Pomoću Azure AD povezivanje stanja sinkronizacije](active-directory-aadconnect-health-sync.md).

![Azure AD Connect stanja za AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Upozorenja za Azure AD povežite zdravlje za AD DS
U odjeljku upozorenja unutar Azure AD povežite zdravlje za AD DS daje popis aktivnih i riješi upozorenja vezana uz vaše kontrolera domena. Odabir upozorenje aktivna ni riješi otvara novi plohu s dodatnim informacijama uz korake razlučivost web-mjesta i veze na dokumentaciju za podršku. Svaka vrsta upozorenja može imati jednu ili više instanci koje odgovaraju svakom kontrolera domene utječe taj određeni upozorenja. Pri dnu upozorenja plohu, dvokliknite je kontroler problematične domene da biste otvorili dodatne plohu s više pojedinosti o tu instancu upozorenja.

U ovom plohu možete omogućiti obavijesti e-poštom za upozorenja i u prikazu Promjena vremenskog raspona. Proširivanje vremenski raspon omogućuje vam da biste vidjeli prethodnog riješi upozorenja.

![Pogreška pri sinkronizaciji Azure AD Connect](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Nadzorna ploča kontrolera domene
Ove nadzorne ploče nudi topološko prikaz okruženju, zajedno s ključa radu metriku i status stanja za svaki vaše kontrolera nadziranim domene. Izloženi metriku pomoć da biste brzo prepoznavanje kontrolera sve domene koje zahtijevaju dodatnu istrage. Prema zadanim postavkama, prikazuje se samo podskupa stupaca. Međutim, možete pronaći na čitav skup Dostupni stupci tako da dvokliknete naredba stupce. Odabir stupaca koji vam najviše zanimaju, uključuje ove nadzorne ploče u jedan i jednostavno postavite da biste pogledali stanja okruženje za AD DS.

![Kontrolera domena](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Kontrolera domena se mogu grupirati prema njihov odgovarajući domene ili web-mjesta, što je korisno za razumijevanje topologije okruženju. Na kraju, ako dvokliknite zaglavlje plohu, na nadzornoj ploči Maksimizira mogu koristiti na zaslonu dostupna agencija. U ovom veći prikaz koristan je kada se prikazuje više stupaca.

## <a name="replication-status-dashboard"></a>Nadzorna ploča stanja replikacije
Prikaz statusa replikacije i replikacije topologije vaše kontrolera nadziranim domene, nudi se ove nadzorne ploče. Status najnovije pokušaj replikacije nalazi uz korisno dokumentaciju za sve pogreške koje se nalaze. Možete dvokliknuti kontroler domene s pogreškom, da biste otvorili novi plohu uz podatke kao što su: pojedinosti o pogrešci, preporučuje se razlučivost koraka i veze na dokumentaciju za otklanjanje poteškoća.

![Status replikacije](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Nadzor
Ova značajka omogućuje grafički trendova mjerača različite performansi koji se neprestano prikupiti iz svake kontrolera nadziranim domene. Performanse kontrolera domena se može usporediti jednostavno preko svih drugih nadziranim kontrolera vaše skupa stabala. Osim toga, možete vidjeti različite mjerača performansi usporedno, što je korisno pri otklanjanju poteškoća u svom okruženju.

![Nadzor](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Prema zadanim postavkama, ne možemo ste sam odabrao četiri mjerača performansi; Međutim, možete unijeti i drugima klikom na naredbu filtar i odabir ili poništenje odabira mjerača sve željene performansi. Osim toga, možete dvokliknuti performanse brojač grafikon da biste otvorili novi plohu koji sadrži točke podataka za svaku kontrolera nadziranim domene.

## <a name="related-links"></a>Srodne veze

* [Azure AD Connect stanja](active-directory-aadconnect-health.md)
* [Stanje Agent za instalaciju za Azure AD Connect](active-directory-aadconnect-health-agent-install.md)
* [Stanje postupci za Azure AD Connect](active-directory-aadconnect-health-operations.md)
* [Korištenje Azure AD povezati stanja AD fs](active-directory-aadconnect-health-adfs.md)
* [Korištenje stanja povezati Azure AD za sinkronizaciju](active-directory-aadconnect-health-sync.md)
* [Najčešća pitanja o stanju za Azure AD Connect](active-directory-aadconnect-health-faq.md)
* [Povijest verzija stanja za Azure AD Connect](active-directory-aadconnect-health-version-history.md)
