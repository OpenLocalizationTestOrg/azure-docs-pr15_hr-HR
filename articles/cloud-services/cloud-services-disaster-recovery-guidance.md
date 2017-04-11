<properties
    pageTitle="Što učiniti u slučaju programa Azure servisa prekidu koji utječe na servise u Oblaku Azure | Microsoft Azure"
    description="Saznajte što učiniti u slučaju programa Azure service prekidu koji utječe na servise u Oblaku Azure."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Što učiniti u slučaju programa Azure servisa prekidu koji utječe na servise u Oblaku Azure

Microsoft, radimo konačnog da biste bili sigurni da naših usluga uvijek su dostupne kada su vam potrebne. Prisilno izvan naš kontrola nam ponekad utjecati na način koji uzrokuju to izbjeglo neplanirano servisa.

Microsoft pruža usluge ugovor o razini (SLA) za njegove servise kao izvršenja za vrijeme aktivnosti i povezivanjem. SLA za pojedinačne Azure usluge pronaći ćete na [Ugovore o razini usluge za Azure](https://azure.microsoft.com/support/legal/sla/).

Azure već ima mnoge značajke ugrađene platformu koji podržavaju iznimno dostupnih aplikacija. Dodatne informacije o tih servisa, pročitajte [oporavak Izrada i visoke dostupnosti za Azure aplikacije](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

U ovom se članku pokriva true Izrada scenarij za oporavak kada cijelog područja dođe do prekida zbog glavne prirodnim Izrada ili prekid Primamo servisa. To su rijetko pojavljivanja, ali morate pripremiti za mogućnost ima li se prekida cijelo područje. Ako cijelo područje sučelja servisa prekidu, lokalno suvišnih kopije vaših podataka bi privremeno nedostupan. Ako ste omogućili zemlj replikacije, tri primjerka blob polja za pohranu Azure i tablice spremaju se u nekoj drugoj regiji. U slučaju dovršeno regionalnih prekida ili Izrada u kojem primarni regija nije oporaviti, Azure remaps sve DNS unose s područjem zemlj replicirati.

>[AZURE.NOTE]Imajte na umu da nemate sve kontrole nad taj postupak, a će javiti samo za to izbjeglo podatkovnog centra razini usluge. Zbog toga, morate je za druge strategije specifičnim aplikacijama sigurnosnu kopiju da biste postigli najvišu razinu dostupnost. Dodatne informacije potražite u odjeljku o [Strategije podatke za oporavak Izrada](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR). Ako želite da biste mogli utjecati na vlastitu prebacivanje, možda ćete morati razmisliti o korištenju [pristup za čitanje zemlj suvišnih prostora za pohranu (RA GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage), čime se kopija samo za čitanje podataka u drugoj regiji.

Da biste lakše rukovati tih rijetko pojava, slučaju servisa prekidu cijelog područja gdje je aplikacija Azure VM implementiran dajemo sljedeće smjernice za Azure virtualnim strojevima (VMs).

##<a name="option-1-wait-for-recovery"></a>Mogućnost 1: Čekanja za oporavak
U ovom slučaju ne poduzmete na svoj dio potreban je. Znate li Azure timovima rade održavate da biste vratili dostupnost servisa. Vidjet ćete trenutno stanje servisa na našem [Nadzorna ploča za stanje servisa Azure](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Ovo je najbolja mogućnost ako klijent nije vam postavio oporavak Azure web-mjesta ili ima sekundarnu implementacije u nekoj drugoj regiji.

Za korisnike koji žele odmah ostvarite pristup svojim distribuiranih servisima u oblaku, dostupne su sljedeće mogućnosti.

>[AZURE.NOTE]Imajte na umu da imaju ove mogućnosti nastanka gubitak podataka.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Mogućnost 2: Ponovno uvesti konfiguraciju servisa oblaka za novu regiju

Ako imate izvorni kod, samo jednostavno možete implementirati aplikaciju, pridruženi konfiguraciju i pridružene resurse na novi servis u oblaku u novu regiju.  

Više pojedinosti o stvaranju i implementacija servisne aplikacije za oblak potražite u članku [kako stvoriti i implementirati servis u oblaku](./cloud-services-how-to-create-deploy-portal.md).

Ovisno o aplikaciji izvora podataka, morate provjeriti oporavak postupke za izvor podataka aplikacije.
  * Prostor za pohranu Azure izvora podataka, potražite u članku [replikacije Azure prostora za pohranu](../storage/storage-redundancy.md#read-access-geo-redundant-storage) za provjeru na izborniku mogućnosti koje su dostupne na temelju modela replikacije ste odabrali za svoju aplikaciju.
  * Izvori podataka SQL baze podataka, pročitajte [Pregled: tvrtke continuity i baza podataka Izrada oporavak SQL baze podataka u Oblaku](../sql-database/sql-database-business-continuity.md) informacije o mogućnosti koje su dostupne na temelju modela odabranom replikacije aplikacije.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Mogućnost 3: Korištenje sigurnosne kopije implementaciju putem upravitelja za Azure promet
Ta mogućnost pretpostavlja već namijenjene rješenje aplikacije uz regionalne Izrada oporavak na umu. Koristite ovu mogućnost ako već imate sekundarne oblaka aplikacije implementacije servise koji se izvodi u nekoj drugoj regiji i povezanih putem kanala Upravitelj promet. U ovom slučaju Provjera stanja sekundarne implementacije. Ako je dobar, možete preusmjeriti promet na nju kroz Azure promet Manager. Pomoću ovog strategije možete iskoristiti metodu usmjeravanja promet i konfiguracija redoslijed za prebacivanje u Azure promet Manager. Dodatne informacije potražite u članku [kako konfigurirati postavke upravitelja promet](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Preko područja s Azure promet upravitelja za ujednačavanje Azure servise u Oblaku](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Daljnji koraci

Da biste saznali više o tome kako implementirati visoke dostupnosti strategije i oporavak Izrada, potražite u članku [oporavak Izrada i visoke dostupnosti za Azure aplikacije](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Razvoj detaljne tehničke razumijevanje mogućnosti platformu oblaka, potražite u članku [Azure otpornost Tehnički vodič](../resiliency/resiliency-technical-guidance.md).

Ako su upute ne isključite ili ako želite da na vaše ime učiniti obratite se [Podršci](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
