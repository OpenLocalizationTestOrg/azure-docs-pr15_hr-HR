<properties
   pageTitle="Oporavak servisa sigurnog najčešća pitanja vezana uz | Microsoft Azure"
   description="Ova verzija u najčešćim Pitanjima podržava javno pretpregled izdanja servisa Azure sigurnosnu kopiju. Odgovori na najčešća pitanja o agent za sigurnosne kopije, sigurnosno kopiranje i zadržavanja, oporavak, sigurnost i druge najčešća pitanja o Azure sigurnosne kopije rješenja."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="sigurnosno kopiranje rješenje; Servis za sigurnosne kopije"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Oporavak servisa sigurnog – najčešća Pitanja


U ovom se članku navode informacije specifične za oporavak servisa sigurnog, a zatim ga nadopunjuju [Najčešća pitanja vezana uz Azure sigurnosnu kopiju](backup-azure-backup-faq.md). Najčešća pitanja vezana uz sigurnosne kopije Azure nudi potpunog skupa pitanja i odgovori o servisu Azure sigurnosnu kopiju.  

Možete postavljati pitanja o Azure sigurnosnu kopiju u odjeljku Disqus u ovom se članku ili povezane članka. Pitanja o servisu Azure sigurnosnog kopiranja možete objaviti i u na [forumu za raspravu](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Oporavak Services sefovi su Voditelj resursa koji se temelje. Sigurnosno kopiranje sefovi (klasičan način rada) i dalje podržava? <br/>
Da, i dalje podržane sefovi sigurnosnu kopiju. Stvaranje sigurnosne kopije sefovi [Klasični portal](https://manage.windowsazure.com). Stvaranje sefovi oporavak servisa [Azure portal](https://portal.azure.com). No preporučujemo stvaranje oporavak servisa sigurnog kao sve buduće poboljšanja bit će dostupan samo u oporavak servisa sigurnog.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Može li se pri migraciji sigurnosne kopije sigurnog za oporavak servisa sigurnog? <br/>
Nažalost ne, trenutno ne možete preseliti sadržaj sigurnosne kopije sigurnog za oporavak servisa sigurnog. Radimo na dodavanje tu funkciju, ali nije dostupan kao dio javno pretpregled.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Oporavak Services sefovi podržavaju klasični VMs ili, ovisno o resursima VMs? <br/>
Oporavak Services sefovi podržava oba modela.  Možete stvoriti sigurnosnu VM stvorene na portalu klasični (koji su VMs klasičan način rada) ili u VM stvorili na portalu za Azure (koji se temelje Voditelj resursa) za oporavak servisa sigurnog.

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Li sigurnosnu kopiju moje klasični VMs u sigurnosno kopiranje zbirke ključeva. Sada želim migrirati Moje VMs iz klasičan način rada u načinu Voditelj resursa.  Kako se sigurnosno kopirajte ih u oporavak servisa sigurnog?
Sigurnosne kopije klasični VMs u sigurnosno kopiranje zbirke ključeva automatski nije moguće migrirati za oporavak servisa sigurnog prilikom migracije na VMs iz klasični način Voditelj resursa. Slijedite ove korake za migraciju VM sigurnosne kopije:

1. U sigurnosne kopije sigurnog idite na karticu **Zaštićeni stavke** i odaberite na VM. Kliknite [Prekini zaštitu](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Ostavite *Brisanje pridruženih sigurnosne kopije podataka* mogućnost **poništen**.
2. Migriranje virtualnog računala iz klasičan način rada u načinu Voditelj resursa. Provjerite je li da web-mjesto za pohranu i u okvir za mrežne odgovara virtualnog računala i migriraju u načinu Voditelj resursa.
3. Stvaranje sigurnog za usluge oporavak i konfiguriranje sigurnosne kopije na migriranim virtualnog računala pomoću **sigurnosne kopije** akcije pri vrhu nadzorne ploče zbirke ključeva. Dodatne informacije o [omogućivanju sigurnosnu kopiju oporavak servisa sigurnog](backup-azure-vms-first-look-arm.md)
