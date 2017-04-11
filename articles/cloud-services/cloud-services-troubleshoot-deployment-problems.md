<properties
 pageTitle="Otklanjanje poteškoća za implementaciju servisa oblaka | Microsoft Azure"
 description="Postoji nekoliko uobičajenih problema možda naići kada servis u oblaku za implementaciju u Azure. U ovom se članku daje rješenja za neke od njih."
   services="cloud-services"
   documentationCenter=""
   authors="simonxjx"
   manager="felixwu"
   editor=""
   tags="top-support-issue"/>
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="tbd"
   ms.date="09/02/2016"
   ms.author="v-six" />

# <a name="troubleshoot-cloud-service-deployment-problems"></a>Otklanjanje poteškoća za implementaciju servisa oblaka

Kada implementacija Aplikacijski paket za oblak servisa Azure, možete dobiti informacije o implementacijskih iz okna sa **svojstvima** na portalu za Azure. Pojedinosti u ovom oknu možete koristiti za pomoć pri otklanjanju poteškoća sa servisom cloud, a možete unijeti informacije za podršku Azure prilikom otvaranja novi zahtjev za podršku.

U oknu **Svojstva** možete pronaći na sljedeći način:

* Na portalu Azure kliknite implementacija servis u oblaku, kliknite **sve postavke**, a zatim **Svojstva**.
* Azure klasični portalu kliknite implementacija servis u oblaku, kliknite **nadzorna PLOČA**, koja se nalazi u donjem desnom kutu stranice (ispod **brzi pogled**). Imajte na umu da postoji nema oznake "Svojstva" u oknu.

> [AZURE.NOTE] Kopiranje sadržaja okna sa **svojstvima** u međuspremnik tako da kliknete ikonu u gornjem desnom kutu okna.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Problem: ne mogu pristupiti web-mjesto, ali pokreće Moje implementacije i spremni sve instance uloga

URL web-mjesta vezu prikazane na portalu obuhvaćaju priključak. Zadani je priključak za web-mjesta je 80. Ako aplikacija je konfiguriran za pokretanje u drugi priključak, morate dodati na točan broj priključka URL prilikom pristupa web-mjesta.

1. Na portalu Azure kliknite implementacije servis u oblaku.
2. U oknu **Svojstva** Azure portal provjerite priključke za slučajeve uloga (u odjeljku **Krajnje točke unosa**).
3. Ako priključak nije 80, dodajte vrijednost odgovarajući priključak URL prilikom pristupa aplikacije. Da biste odredili koji nisu zadani priključak, unesite URL, a zatim dvotočku (:), nakon čega slijedi broj priključka, bez razmaka.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Problem: Moje uloga instance reciklira bez me ikakve

Servis automatskog popravka pojavljuje se automatski kada Azure otkrije problem čvorove i stoga premješta uloga instance novi čvorovi. Kada se to dogodi, možda će se prikazati vaša uloga instance recikliranje automatski. Da biste saznali je li servis automatskog popravka došlo je do:

1. Na portalu Azure kliknite implementacije servis u oblaku.
2. U oknu **Svojstva** Azure portal pregledajte podatke i odrediti hoće li servis automatskog popravka vrijeme opaženih uloge recikliranje.

Uloge će i koš za otprilike jednom mjesečno tijekom glavno računalo OS i ažuriranja za goste OS.  
Dodatne informacije potražite u članku na blogu [Uloga Instance pokreće zbog OS nadogradnje](http://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Problem: ne možete učiniti VIP Zamijeni i obavijest o pogrešci

VIP Zamijeni nije dopušteno ako implementacije ažuriranje je u tijeku. Uvođenje mogu se automatski ažurira, kada:

* Novi goste operacijski sustav je dostupan i da ste konfigurirali za Automatska ažuriranja.
* Pojavljuje se servisa automatskog popravka.

Da biste saznali jesu li zbog automatsko ažuriranje ne možete dovršiti VIP zamjena:

1. Na portalu Azure kliknite implementacije servis u oblaku.
2. U oknu **Svojstva** Azure portal pogledajte vrijednost **statusa**. Ako je **spremna**, provjerite **posljednje operacije** da biste vidjeli ako nedavno se dogodilo jedno koji bi mogli VIP Zamijeni.
3. Ponovite korake 1 i 2 za implementaciju proizvodnje.
4. Ako automatsko ažuriranje je u tijeku, pričekajte da biste dovršili prije no što pokušate učinite VIP Zamijeni.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Problem: Uloga instance ponavljanje između rada, Initializing, zauzet i zaustavljanja

Ovaj uvjet ne može upućivati na problem s datotekom kod, paketa i konfiguraciji aplikacije. U tom slučaju, trebali biste moći vidjeti status promjena svakih nekoliko minuta, a Azure portal možda nešto izgovorite kao što su **Recycling**, **zauzeto**ili **Initializing**. To znači da postoji nešto problem s programom koji je čuvanja instancu uloga pokretanje.

Dodatne informacije o otklanjanju poteškoća s za taj problem, potražite u članku na blogu [Azure PaaS izračunati Dijagnostika podataka](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) i [uobičajeni problemi koji uzrokuju uloge u koš za](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md).

## <a name="problem-my-application-stopped-working"></a>Problem: Moje aplikacije ne radi

1. Na portalu Azure kliknite instancu uloge.
2. U oknu **Svojstva** Azure portal Imajte na umu sljedeće uvjete da biste riješili problem:
   * Ako nedavno prestao instancu uloga (vrijednost **prekinuti count**možete provjeriti), uvođenje se može ažurirati. Pričekajte da biste vidjeli ako instancu uloga nastavlja radi samostalno.
   * Ako je instanca uloga **zauzeto**, provjerite kod aplikacije da biste vidjeli događaj [StatusCheck](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.statuscheck) obrađuje. Možda ćete morati dodati ili ispraviti neke kod koji rukuje ovaj događaj.
   * Idite do dijagnostičkih podataka i otklanjanje poteškoća scenariji u blogu [Azure PaaS izračunati Dijagnostika podataka](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

>[AZURE.WARNING] Ako je koš za servis u oblaku, ponovno svojstva za implementaciju, učinkovito brisanje informacija za izvorni problem.

## <a name="next-steps"></a>Daljnji koraci

Prikaz više [članke za otklanjanje poteškoća](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) za servise u oblaku.

Upute za otklanjanje poteškoća s oblaka servisa uloga pomoću Azure PaaS računalne Dijagnostika podatke, potražite u članku [se nizu blogova Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
