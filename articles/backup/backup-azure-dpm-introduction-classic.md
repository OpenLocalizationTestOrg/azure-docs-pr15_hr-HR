<properties
    pageTitle="Uvod u sigurnosne kopije Azure DPM | Microsoft Azure"
    description="Uvod u sigurnosno kopiranje DPM poslužitelje pomoću servisa Azure sigurnosnog kopiranja"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="Sustava centar za zaštitu Upravitelj podataka, Upravitelj zaštite podataka, dpm sigurnosnog kopiranja"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Priprema za sigurnosno kopiranje radnih opterećenja za Azure s DPM

> [AZURE.SELECTOR]
- [Poslužitelj za Azure sigurnosne kopije](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Poslužitelj za Azure sigurnosne kopije (Classic)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (Classic)](backup-azure-dpm-introduction-classic.md)


Ovaj članak sadrži Uvod u korištenje Microsoft Azure Backup da biste zaštitili poslužitelje sustava centra podataka zaštitu Manager (DPM) i radnih opterećenja. Tako da pročitate, ćete razumjeli:

- Kako funkcionira sigurnosne kopije server Azure DPM
- Preduvjeti za postizanje izglađenim sučelje za sigurnosne kopije
- Uobičajene pogreške i kako ih baviti
- Podržani scenariji

Centar za sustav DPM sigurnosno podatke o datotekama i aplikacije. Podataka sigurnosno DPM može biti pohranjene na vrpci na disku ili sigurnosnu kopiju na Azure pomoću sigurnosnog kopiranja Microsoft Azure. DPM stupi u interakciju s Azure sigurnosne kopije na sljedeći način:

- **DPM uveden kao fizičke poslužitelju ili na lokalnim virtualnog računala** – ako DPM je uveden kao fizičke poslužitelju ili na lokalnim Hyper-V virtualnog računala možete sigurnosno kopiranje podataka za Azure sigurnosno kopiranje zbirke ključeva osim na disku i vrpcu sigurnosne kopije.
- **DPM u uveden kao Azure virtualnog računala** – iz sustava centra 2012 R2 ažuriranje 3, DPM može uvesti kao Azure virtualnog računala. Ako je DPM implementiran kao Azure virtualnog računala možete sigurnosno kopiranje podataka za Azure diskova priložiti DPM Azure virtualnog računala ili prostora za pohranu podataka možete offload sigurnosnim do programa Azure sigurnosno kopiranje zbirke ključeva.

## <a name="why-backup-your-dpm-servers"></a>Zašto se sigurnosno kopiranje poslužitelja DPM?

Tvrtke prednosti korištenja Azure sigurnosne kopije za sigurnosno kopiranje DPM poslužitelji obuhvaćaju sljedeće:

- Za lokalnu implementaciju DPM, možete koristiti Azure sigurnosnu kopiju kao zamjena za Dugoročne implementacije na vrpcu.
- U slučaju DPM implementacije u Azure sigurnosne kopije Azure omogućuje offload prostora za pohranu s diska Azure omogućujući vam skaliranja spremanjem starijim podacima u Azure i sigurnosne kopije nove podatke na disku.

## <a name="how-does-dpm-server-backup-work"></a>Kako funkcionira sigurnosne kopije DPM server?
Da biste sigurnosno virtualnog računala, najprije točke u vrijeme snimku podataka potreban je. Servis za Azure sigurnosne kopije pokreće sigurnosno kopiranje zakazano vrijeme, a pokreće sigurnosne kopije nastavak da biste preuzeli snimku. Sigurnosno kopiranje proširenje koordinate sa servisom VSS u goste da biste postigli dosljednost, a poziva API snimke blobova platforme Azure prostora za pohranu servisa kad postane dosljednost. To možete učiniti da biste snimku dosljedan diskova virtualnog računala bez potrebe da biste ga isključili.

Nakon snimanja snimku podaci se prenose Azure sigurnosne kopije servis za sigurnosno kopiranje zbirke ključeva. Servis vodi brigu o prepoznavanje i prijenos blokove koje su se promijenile od zadnje sigurnosne kopije upućivanje učinkovito spremanje sigurnosne kopije i mrežne. Po dovršetku prijenos podataka snimka se uklanja, a zatim stvori točka za oporavak. Oporavak sada mogu vidjeti na portalu za Azure klasični.

>[AZURE.NOTE] Za Linux virtualnim strojevima moguće je samo datoteke-dosljedan sigurnosnu kopiju.

## <a name="prerequisites"></a>Preduvjeti
Priprema Azure sigurnosnu kopiju za sigurnosno kopiranje podataka DPM na sljedeći način:

1. **Stvaranje sigurnosne kopije sigurnog** – stvaranje na sigurnog na konzoli za Azure sigurnosnu kopiju.
2. **Preuzimanje sigurnog vjerodajnice** – u Azure sigurnosnu kopiju, prijenos upravljanje certifikat koji ste stvorili u zbirke ključeva.
3. **Instalacija Azure Agent za sigurnosne kopije i registraciju poslužitelja** – iz Azure sigurnosne kopije, instalirajte agenta na svakom poslužitelju DPM i registraciju DPM poslužitelja u sigurnosno kopiranje zbirke ključeva.

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>Preduvjeti (i ograničenja)

- DPM možete imati kao fizičke poslužitelja ili virtualni stroj Hyper-V instaliran na web-mjesto sustava centra 2012 SP1 ili u okvir za sustav centar 2012 R2. Mogu se izvoditi i kao Azure virtualnog računala sustavom 2012 R2 centar sustava s najmanje 3 Kumulativno ažuriranje za DPM 2012 R2 ili Windows virtualnog računala u VMWare koji se izvode na 2012 R2 centar sustava s najmanje Update Rollup 5.
- Ako koristite DPM sustava centra 2012 SP1, instalirajte Update Rollup 2 za sustav centar Upravitelj zaštite podataka SP1. To je potrebno da biste mogli instalirati Azure Backup Agent.
- Poslužitelj DPM mora imati komponente Windows PowerShell i .net Framework 4,5 instaliran.
- DPM može sigurnosno kopirati Većina radnih opterećenja Azure sigurnosno kopirati. Što je podržano pogledajte cijeli popis Azure sigurnosne kopije za podršku ispod stavke.
- S mogućnošću "Kopiraj na vrpcu" nije moguće oporaviti podatke pohranjene u Azure sigurnosnu kopiju.
- Morat ćete račun za Azure s omogućena značajka Azure sigurnosnu kopiju. Ako nemate račun, možete stvoriti besplatnu probnu računa u samo nekoliko minuta. Saznajte više o [sigurnosne kopije Azure cijene](https://azure.microsoft.com/pricing/details/backup/).
- Korištenje sigurnosnih kopija Azure zahtijeva Azure Backup Agent za instalaciju na poslužiteljima na kojima želite sigurnosno kopirati. Svaki poslužitelj, morate imati najmanje 10% veličina podataka koje se stvara sigurnosnu kopiju, dostupni kao lokalne besplatnog prostora za pohranu. Na primjer, sigurnosno kopiranje 100 GB podataka potreban je najmanje 10 GB slobodnog prostora na prazno mjesto. Dok je minimalne 10%, preporučuje se 15% besplatne lokalne prostora za pohranu za predmemoriju.
- Podaci će se pohranjuju u Azure sigurnog spremišta. Nema ograničenja za količinu podataka koje možete spremiti sigurnosnu kopiju sigurnosne kopije programa Azure vault, ali veličinu izvora podataka (primjerice virtualnog računala ili baze podataka) bi trebalo biti dulji od 54,400 GB.

Ove vrste datoteka podržane su za spremiti sigurnosnu kopiju Azure:

- Šifrirana (Potpuna izrade sigurnosnih kopija samo)
- Sažeti (rastuća sigurnosne kopije podržane)
- Kratke (rastuće sigurnosne kopije podržane)
- Komprimirana i kratke (tretira kao Sparse)

I to nisu podržani:

- Poslužitelje na velika i mala slova datotečnih nisu podržane.
- Veza (preskočene)
- Reparse točaka (preskočene)
- Šifrirana i sažeti (preskočene)
- Šifrirana i kratke (preskočiti)
- Komprimirana strujanje
- Kratke strujanje

>[AZURE.NOTE] Iz u sustavu centar 2012 DPM sa servisnim paketom SP1 Nadalje, sigurnosno kopirajte gore radnih opterećenja zaštićen DPM za Azure pomoću Microsoft Azure sigurnosne kopije.
