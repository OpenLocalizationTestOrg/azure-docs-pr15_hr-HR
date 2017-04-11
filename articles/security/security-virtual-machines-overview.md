<properties
   pageTitle="Pregled sigurnosti Azure virtualnim strojevima | Microsoft Azure"
   description=" Azure virtualnim strojevima što je fleksibilnosti virtualizacije bez potrebe za kupnju i održavanje fizički hardverski koja se pokreće virtualnog računala.  Ovaj članak sadrži pregled temeljni Azure sigurnosnim značajkama koje je moguće koristiti s Azure virtualnih računala. "
   services="security"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/16/2016"
   ms.author="terrylan"/>

# <a name="azure-virtual-machines-security-overview"></a>Pregled sigurnosti Azure virtualnim strojevima

Azure virtualnim strojevima možete uvesti te širokog raspona računalstvo rješenja agilno način. S podrškom za Microsoft Windows, Linux, Microsoft SQL Server, Oracle, IBM, SAP i servisa Azure BizTalk možete implementirati sve radno opterećenje i bilo koji jezik na gotovo bilo kojeg operacijski sustav.

Azure virtualnog računala pruža fleksibilnost virtualizacije bez potrebe za kupnju i održavanje fizički hardverski koja se pokreće virtualnog računala.  Možete izraditi i implementacija aplikacije uz jamstvo da se podaci nalaze zaštićenog i sigurne u našem iznimno sigurne podatkovnim centrima.

U Azure, možete izraditi poboljšane sigurnosti, usklađen rješenja koja:

- Virtualnim strojevima Zaštita od virusa i zlonamjernog softvera
- Šifriranje osjetljivih podataka
- Sigurna mrežni promet
- Prepoznavanje i otkriti prijetnji
- Zadovoljava preduvjete za usklađenost

Je nude pregled temeljni Azure sigurnosnim značajkama koje je moguće koristiti s virtualnim strojevima cilj ovog članka. Nudimo vam i veze na članke koji daju pojedinosti svake značajke da biste saznali više.  

Temeljni Azure virtualnog računala sigurnosne mogućnosti da biste se obuhvaćeno u ovom se članku:

- Antimalware
- Modul za sigurnost hardvera
- Šifriranje virtualnog računala
- Sigurnosno kopiranje virtualnog računala
- Oporavak Azure web-mjesta
- Virtualne mreže
- Upravljanje pravilima za sigurnost i izvješćivanje o pogreškama
- Usklađenost

## <a name="antimalware"></a>Antimalware

S Azure, možete koristiti softver antimalware dobavljača sigurnost kao što je Microsoft, Symantec, Trend Micro, tvrtke McAfee i Kaspersky da biste zaštitili virtualnih računala od štetnih datoteke, reklamni i drugih prijetnji. U odjeljku Podrobnije se informirajte u nastavku da biste pronašli članke sustava na partnera rješenja.

Microsoft Antimalware za servise u Oblaku Azure i virtualnim strojevima je mogućnost zaštita u stvarnom vremenu prepoznavanje i uklanjanje virusa, špijunskih programa i ostalih zlonamjernih programa.  Microsoft Antimalware nudi konfigurirati upozorenja kada zna zlonamjerni ili neželjeni softver pokuša instalirati ili pokrenuti Azure sustavima.

Microsoft Antimalware je rješenje jedne agent za aplikacije i okruženja za klijenta, namijenjenu izvoditi u pozadini bez Ljudski intervencije. Možete implementirati zaštite u skladu s potrebama vaše aplikacije radnih opterećenja, neki osnovni sigurne-po-zadani ili Napredna konfiguracija prilagođene, uključujući antimalware nadzor.

Kada implementacije i omogućivanje Microsoft Antimalware, dostupne su sljedeće značajke core:

- Zaštita u stvarnom vremenu - aktivnosti monitora na servise u Oblaku i na virtualnim strojevima za otkrivanje i blokiranje izvođenja zlonamjernog softvera.
- Povremeno zakazani skeniranje - izvodi ciljano pregledavanje da biste otkrili zlonamjernog softvera, uključujući aktivno pokretanje programa.
- Olakšava zlonamjernog softvera - poduzima akcije na otkriven zlonamjernog softvera, kao što su brisanje ili quarantining zlonamjernih datoteke i čišćenje stavke zlonamjerni registra.
- Potpis ažuriranja – automatski instalira najnovije potpisi zaštitu (definicije virusa) da biste bili sigurni zaštitu ažuran na unaprijed odredio učestalost.
- Modul Antimalware – automatski ažurira ažurira modul Microsoft Antimalware.
- Platforme Antimalware – automatski ažurira ažurira platformu Microsoft Antimalware.
- Aktivni zaštitu - izvješća Azure telemetrijskih metapodataka o otkriven prijetnji i sumnjiva resurse da biste bili sigurni brzi odgovor i omogućuje isporuke u stvarnom vremenu sinkrono potpis kroz Microsoft Active zaštita sustava (mape).
- Uzoraka izvješćivanja – omogućuje izvješća i uzorke sa servisom Microsoft Antimalware olakšavaju sužavanje usluge i omogućivanje otklanjanje poteškoća.
- Izostavljene – omogućuje aplikacije i administratori servisa da biste konfigurirali određene datoteke procesa, i pogone da biste ih isključiti iz zaštitu i traženje performanse i drugih razloga.
- Zbirka događaj Antimalware - zapisuje stanje servisa antimalware, sumnjive aktivnosti i potražite alat za akcije u zapisniku događaja operacijski sustav i prikuplja ih račun za Azure prostora za pohranu klijenta.

Dodatne informacije: da biste saznali više o antimalware softver za zaštitu virtualnih računala, pogledajte:

- [Microsoft Antimalware za servise u Oblaku Azure i virtualnih računala](../security/azure-security-antimalware.md)
- [Implementacija rješenja Antimalware na Azure virtualnim strojevima](https://azure.microsoft.com/blog/deploying-antimalware-solutions-on-azure-virtual-machines/)
- [Kako instalirati i konfigurirati Trend Micro precizno sigurnost kao servisa na VM za Windows](../virtual-machines/virtual-machines-windows-classic-install-trend.md)
- [Kako instalirati i konfigurirati Zaštita na krajnjoj točki Symantec na VM za Windows](../virtual-machines/virtual-machines-windows-classic-install-symantec.md)
- [Nove mogućnosti Antimalware za zaštitu virtualnim strojevima Azure – Zaštita na krajnjoj točki tvrtke McAfee](https://azure.microsoft.com/blog/new-antimalware-options-for-protecting-azure-virtual-machines/)
- [Sigurnost rješenja Azure Marketplace](https://azure.microsoft.com/marketplace/?term=security)

## <a name="hardware-security-module"></a>Sigurnost hardversko modul

Šifriranje i provjera autentičnosti nećete poboljšati sigurnost osim ako su zaštićeni tipki sami. Upravljanje i sigurnosti ključnih tajne i tipke može pojednostavniti spremanjem ih u sigurnog ključ Azure. Ključ sigurnog pruža mogućnost za pohranu ključeva u hardver sigurnost Module (HSMs) certificirani za FIPS standarde razine 2 140-2. Ključeva za šifriranje SQL Server za sigurnosno kopiranje ili [šifriranje prozirne podataka](https://msdn.microsoft.com/library/bb934049.aspx) sve moguće pohraniti u sigurnog tipke sa strelicama ili tajne iz aplikacije. Dozvole i pristup zaštićeni stavke upravlja se putem [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/).

uči više:

- [Što je sigurnog ključ Azure?](../key-vault/key-vault-whatis.md)
- [Početak rada s sigurnog ključ Azure](../key-vault/key-vault-get-started.md)
- [Azure ključ sigurnog bloga](https://blogs.technet.microsoft.com/kv/)

## <a name="virtual-machine-disk-encryption"></a>Šifriranje virtualnog računala

Azure šifriranje Disk je nova mogućnost koja omogućuje šifriranje diskova sustava Windows i Linux Azure virtualnog računala. Azure šifriranje koristi industrijskih standardne [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) značajka sustava Windows i značajka [dm crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux kako šifriranje jedinice za os-a i diskova podataka.

Rješenje integriran s Azure ključ sigurnog će vam pomoći odrediti i upravljanje ključeva za šifriranje diska i tajne u pretplatu ključa sigurnog uz jamstvo da se svi podaci u diskova virtualnog računala šifrirane na ostale u Azure prostora za pohranu.

uči više:

- [Azure šifriranje za Windows i Linux IaaS VMs](https://gallery.technet.microsoft.com/Azure-Disk-Encryption-for-a0018eb0)
- [Azure šifriranje diskovnih Linux i virtualnim strojevima sa sustavom Windows](https://blogs.msdn.microsoft.com/azuresecurity/2015/11/16/azure-disk-encryption-for-linux-and-windows-virtual-machines-public-preview-now-available/)
- [Šifriranje virtualnog računala](../security-center/security-center-disk-encryption.md)

## <a name="virtual-machine-backup"></a>Sigurnosno kopiranje virtualnog računala

Azure sigurnosne kopije je skalabilni rješenje koji štiti podataka aplikacije pomoću nula kapitalnim investicijskim i Minimalna operacijski troškovi. Pogreške aplikacije možete oštećena podataka i Ljudski pogrešaka može uzrokovati programskih pogrešaka u aplikacije. Pomoću sigurnosnog kopiranja Azure, zaštićene virtualnim računalima sustava Windows i Linux.

uči više:

- [Što je sigurnosne kopije Azure?](../backup/backup-introduction-to-azure-backup.md)
- [Azure sigurnosne kopije tečaj](https://azure.microsoft.com/documentation/learning-paths/backup/)
- [Azure sigurnosne kopije služba - najčešća Pitanja](../backup/backup-azure-backup-faq.md)

## <a name="azure-site-recovery"></a>Oporavak Azure web-mjesta

Važan dio vaše tvrtke ili ustanove BCDR strategije odrediti kako pratiti tvrtke radnih opterećenja i aplikacije te izvodi kad se pojave planirane i Neplanirana kvarove. Oporavak Azure web-mjesta omogućuje orkestrirali replikacije, prebacivanje i oporavak radnih opterećenja i aplikacije da bi bili dostupni sa sekundarnom mjesta ako svom primarnom mjestu funkcionira.

Oporavak web-mjesta:

- **Simplifies strategije BCDR** – oporavak web-mjesta olakšava rukovati replikacije, prebacivanje i oporavak većeg broja radnih opterećenja tvrtke i aplikacije s jednog mjesta. Oporavak web-mjesta orchestrates replikaciju i prebacivanje, ali ne intercept podataka aplikacije ili bilo kakve informacije o njemu.
- **Pruža fleksibilne replikacije** – pomoću oporavak web-mjesta mogu replicirati radnih opterećenja sustavom Hyper-V virtualnim strojevima, VMware virtualnim strojevima i fizičke poslužitelji Windows/Linux.
- **Prebacivanje podržava i oporavak** – oporavak web-mjesta omogućuje failovers test za podršku Izrada oporavak bušilice bez utjecaja radnog okruženja. Možete koristiti i planiranog failovers bez gubitka nula podataka za očekivani kvarove ili neplanirano failovers s minimalnim gubitka podataka ubuduće (ovisno o učestalost ponavljanja) za neočekivane disasters. Nakon prebacivanje, možete ga failback primarni web-mjestima. Oporavak web-mjesta omogućuje planove za oporavak koji obuhvaćaju skripte i Azure Automatizacija radne knjige tako da možete prilagoditi prebacivanje i oporavak aplikacija više razina.
- **Sekundarni podatkovnog centra Eliminates** – možete replikaciju sekundarnu lokalnog web-mjesta ili Azure. Korištenje Azure kao odredišta za oporavak Izrada uklanja trošak i složenost održavanje sekundarnog web-mjesta. Repliciranu podaci se pohranjuju u Azure prostora za pohranu.
- **Integrates s postojećeg tehnologijama BCDR** – partneri oporavak web-mjesta s drugim BCDR značajkama aplikacije. Na primjer, možete koristiti oporavak web-mjesta da biste zaštitili SQL Server pozadinska od tvrtke radnih opterećenja. To obuhvaća Ugrađena podrška za SQL Server AlwaysOn da biste upravljali Prebacivanje grupe dostupnosti.

uči više:

- [Što je oporavak Azure web-mjesta?](../site-recovery/site-recovery-overview.md)
- [Kako funkcionira oporavak Azure web-mjesta?](../site-recovery/site-recovery-components.md)
- [Što su radnih opterećenja zaštićen oporavak Azure web-mjesta?](../site-recovery/site-recovery-workload.md)

## <a name="virtual-networking"></a>Virtualne mreže

Virtualnim strojevima potreban vam je veza s mrežom. Da biste podržavaju taj zahtjev, Azure mora imati virtualnim strojevima biti povezani s mrežom virtualne Azure. Azure virtualne mreže je konstrukta logičke on nudi tkanina fizičke Azure mreže. Svaki logičke Azure virtualne mreže je Izolirani s sve druge Azure virtualne mreže. U ovom odvajanja olakšava osigurali mrežni promet na vaše implementacije nije dostupna u drugim klijentima Microsoft Azure.

uči više:

- [Pregled sigurnosti Azure mreže](security-network-overview.md)
- [Pregled virtualne mreže](../virtual-network/virtual-networks-overview.md)
- [Značajke i partnerstvo Enterprise scenarijima za povezivanje s mrežom](https://azure.microsoft.com/blog/networking-enterprise/)

## <a name="security-policy-management-and-reporting"></a>Upravljanje pravilima za sigurnost i izvješćivanje o pogreškama

Centar za sigurnost Azure pomaže spriječiti, otkrivanje i odgovaranje na prijetnji, a omogućuje povećati uvid u, i kontrolu nad, sigurnost Azure resurse. Pruža integriranu sigurnost nadzor i pravila upravljanja preko pretplate Azure, pomaže u otkrivanju prijetnji koje možda u suprotnom otvorite uočen i radi s Bogata paleta sigurnost rješenja.

Centar za sigurnost Azure pomaže vam optimiziranje i nadzirati sigurnost virtualnog računala:

- Pružanje virtualnog računala [preporuke o sigurnosti](../security-center/security-center-recommendations.md) kao što su Primjena ažuriranja sustava, konfiguriranje krajnje točke ACL-a, omogućivanje antimalware, omogućivanje mreže sigurnosne grupe i Primjena šifriranje.
- Praćenje stanja virtualnim strojevima

uči više:

- [Uvod u Centar za sigurnost Azure](../security-center/security-center-intro.md)
- [Najčešća pitanja o centru za sigurnost Azure](../security-center/security-center-faq.md)
- [Centar za sigurnost Azure planiranja i operacije](../security-center/security-center-planning-and-operations-guide.md)

## <a name="compliance"></a>Usklađenost

Azure virtualnim strojevima je potvrđena za SAVEZNOM, FedRAMP, HIPAA, PCI DSS razine 1 i drugim programima ključa usklađenosti. U ovom certifikata olakšava za zadovoljava preduvjete za usklađenost vlastite Azure aplikacijama, a za svoju tvrtku u adresi širokog raspona domaće i međunarodne pravnih.

uči više:

- [Microsoftova centra za pouzdanost: usklađenosti](https://www.microsoft.com/TrustCenter/Compliance/default.aspx)
- [Pouzdani oblaka: Microsoft Azure sigurnost, privatnost i usklađenost](http://download.microsoft.com/download/1/6/0/160216AA-8445-480B-B60F-5C8EC8067FCA/WindowsAzure-SecurityPrivacyCompliance.pdf)
