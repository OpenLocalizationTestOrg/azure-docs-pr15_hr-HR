<properties
    pageTitle="Migriraj bazu podataka SQL poslužitelja SQL Server na na VM | Microsoft Azure"
    description="Naučite kako Migriraj bazu podataka korisnika lokalno na SQL Server u Azure virtualni stroj."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sabotta"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="carlasab"/>


# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>Migriraj bazu podataka SQL poslužitelja SQL Server Azure VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]Upravitelj resursa modela.


Postoje broj metode preseljenja lokalno SQL Server korisnik baze podataka programa SQL Server Azure VM. U ovom članku ukratko će raspravljati različite metode, preporučujemo da najbolji način za različite scenarije i uključuju [vodič](#azure-vm-deployment-wizard-tutorial) voditi pomoću čarobnjaka za **uvođenje bazu podataka SQL poslužitelja za Microsoft Azure VM** . 

Za samo model klasični uvođenja je metoda pomoću čarobnjaka za **uvođenje bazu podataka SQL poslužitelja za Microsoft Azure VM** opisane u [vodič](#azure-vm-deployment-wizard-tutorial) . 

## <a name="what-are-the-primary-migration-methods"></a>Što su primarne migracije metode?

Metode primarni migracije su:

- Koristite SQL Server baze podataka Microsoft Azure VM Čarobnjak za uvođenje
- Sigurnosno lokalno pomoću sažimanja i ručno kopirajte datoteku sigurnosne kopije u Azure virtualni stroj
- Sigurnosne kopije za vraćanje u Azure virtualni stroj i URL iz URL
- Odvojiti i zatim kopirati datoteke podataka i zapisnik Azure blobova i zatim priložite SQL Server Azure VM iz URL-a
- Pretvaranje lokalno fizičke stroj značajke Hyper-V VHD, prijenos Azure blobova i uvođenje kao nova korištenjem VM prenijeti VHD
- Otpremi tvrdi disk pomoću servisa Windows uvoz i izvoz
- Ako imate AlwaysOn uvođenja lokalno, koristite [Čarobnjak za dodavanje replika Azure](virtual-machines-windows-classic-sql-onprem-availability.md) za stvaranje repliku u Azure i zatim prebacivanje pokazivanjem korisnici na instanci Azure baze podataka
- Koristite SQL Server [transakcijskih replikacije](https://msdn.microsoft.com/library/ms151176.aspx) konfiguriranje Azure SQL Server instanci kao pretplatnika i onemogućite replikaciju, pokazivanjem korisnici na instanci Azure baze podataka



## <a name="choosing-your-migration-method"></a>Odabirom metodu preseljenja

Za optimalnu podataka prijenos performanse migracije datoteke baze podataka u VM Azure pomoću komprimirane datoteke sigurnosne kopije obično je najbolji način. To je metoda koja koristi [SQL Server baze podataka Microsoft Azure VM Čarobnjak za uvođenje](#azure-vm-deployment-wizard-tutorial) . Ovaj čarobnjak je preporučeni način za migracije se lokalno korisnik baze podataka pokrenut na SQL Server 2005 ili veći SQL Server 2014 ili veća kada je manje od 1 TB baze podataka komprimiranu datoteku sigurnosne kopije.

Da biste minimizirali pauza radi tijekom postupka migracije baze podataka, koristite mogućnost AlwaysOn ili mogućnost transakcijskih replikacije.

Ako nije moguće koristiti iznad metode, ručno migrirati bazu podataka. Pomoću ove metode će obično počinjete s sigurnosna kopija baze podataka slijedi kopiju na sigurnosne kopije u Azure baza podataka i zatim izvršiti vraćanje baze podataka. Možete kopirati datoteke baze podataka same u Azure i priložiti ih. There nekoliko metoda koje možete izvršiti ovaj ručni postupak migracije baze podataka u Azure VM.

> [AZURE.NOTE] Kada nadogradite na SQL Server 2014 ili SQL Server 2016 iz starijih verzija programa SQL Server, razmotrite hoće li promjene potrebno. Preporučujemo da adresa sve ovisnosti na značajke ne podržava novu verziju SQL Server kao dio migracije projekta. Dodatne informacije o podržanim izdanja i scenarije potražite [nadogradnju na SQL Server](https://msdn.microsoft.com/library/bb677622.aspx).

Sljedeća tablica popisuje svake metode primarni migraciju i objašnjava Kada koristiti svaku metodu je najprikladnije.

| Metoda  | Verzija izvora baze podataka  |  Verzija odredišne baze podataka | Ograničenje veličine sigurnosne kopije izvorne baze podataka  | Bilješke  |
|---|---|---|---|---|
| [Koristite SQL Server baze podataka Microsoft Azure VM Čarobnjak za uvođenje](#azure-vm-deployment-wizard-tutorial) | SQL Server 2005 ili noviji | SQL Server 2014 ili veći | < 1 TB  | Najbrži i najjednostavniji način, koristite kad god je moguće migrirati nove ili postojeće SQL Server instanci u Azure virtualni stroj | 
| [Korištenje u Azure replika Čarobnjak za dodavanje](virtual-machines-windows-classic-sql-onprem-availability.md) | SQL Server 2012 ili noviji | SQL Server 2012 ili noviji | [Ograničenje spremišta Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Minimizira pauza radi, koristite kad imate uvođenja primene programa AlwaysOn |
| [Koristite SQL Server transakcijskih replikacije](https://msdn.microsoft.com/library/ms151176.aspx) | SQL Server 2005 ili noviji | SQL Server 2005 ili noviji | [Ograničenje spremišta Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Koristite kada trebate minimiziranje pauza radi i imati uvođenja primene programa AlwaysOn |
| [Sigurnosno lokalno pomoću sažimanja i ručno kopirajte datoteku sigurnosne kopije u Azure virtualni stroj](#backup-to-file-and-copy-to-vm-and-restore) | SQL Server 2005 ili noviji | SQL Server 2005 ili noviji | [Ograničenje spremišta Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Koristite samo kada nije moguće koristiti čarobnjak, primjerice kada verzija odredišne baze podataka je manje od CU2 SP1 za SQL Server 2012 ili veličina sigurnosne kopije baze podataka je veća od 1 TB (12.8 TB s SQL Server 2016) |
| [Sigurnosne kopije za vraćanje u Azure virtualni stroj i URL iz URL](#backup-to-url-and-restore) | SQL Server 2012 SP1 CU2 ili veći | SQL Server 2012 SP1 CU2 ili veći | < 12.8 TB za SQL Server 2016, inače < 1 TB | Općenito pomoću [sigurnosne kopije URL](https://msdn.microsoft.com/library/dn435916.aspx) je ekvivalent performanse pomoću čarobnjaka i ne prilično jednostavno |
| [Odvojiti i zatim kopirati datoteke podataka i zapisnik Azure blobova i zatim priložite SQL Server u Azure virtualni stroj iz URL-a](#detach-and-copy-to-url-and-attach-from-url) | SQL Server 2005 ili noviji | SQL Server 2014 ili veći | [Ograničenje spremišta Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Ovaj postupak koristite kada planirate za [spremanje ove datoteke pomoću usluga spremišta blobova Azure](https://msdn.microsoft.com/library/dn385720.aspx) i priložiti SQL Server pokrenut Azure VM, naročito s vrlo velike baze podataka |
| [Pretvaranje lokalno računalo značajke Hyper-V VHDs, prijenos Azure blobova i uvođenje novi virtualni stroj pomoću prenesene VHD](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) | SQL Server 2005 ili noviji | SQL Server 2005 ili noviji | [Ograničenje spremišta Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Koristite kada [unošenje SQL Server licencu](../sql-database/sql-database-paas-vs-sql-server-iaas.md), prilikom migracije baze podataka koji će izvoditi na stariju verziju sustava SQL Server ili prilikom migracije sistemske i korisničke baza podataka zajedno kao dio migracije baze podataka ovise o drugim bazama podataka korisnika i/ili bazama podataka sustava. |
| [Otpremi tvrdi disk pomoću servisa Windows uvoz i izvoz](#ship-hard-drive) | SQL Server 2005 ili noviji | SQL Server 2005 ili noviji | [Ograničenje spremišta Azure VM](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | Koristite [Servis Windows uvoz izvoz](../storage/storage-import-export-service.md) kada metode ručnog kopiranja je prespor, kao što su s vrlo velike baze podataka |

## <a name="azure-vm-deployment-wizard-tutorial"></a>Praktični vodič za čarobnjak uvođenja za Azure VM

Koristite čarobnjak **uvođenja bazu podataka SQL poslužitelja za Microsoft Azure VM** u Microsoft SQL Server Management Studio preseliti SQL Server 2005, SQL Server 2008, SQL Server 2008 R2, 2012 SQL Server, SQL Server 2014 ili SQL Server 2016 lokalno korisnik baze podataka (do 1 TB) za SQL Server 2014 ili SQL Server 2016 u Azure virtualni stroj. Ovaj čarobnjak koristite za migriranje baze podataka korisnika postojeće Azure virtualni stroj ili Azure VM s SQL Server stvorio čarobnjak tijekom procesa migracije. Kada Migriraj bazu podataka noviju verziju SQL Server baze podataka automatski nadograditi tijekom procesa.

Metoda je samo klasični uvođenja modela. 

### <a name="get-latest-version-of-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Dohvati najnoviju verziju, uvođenje baze podataka SQL poslužitelja Microsoft Azure VM Čarobnjak za

Da biste bili sigurni da imate najnoviju verziju čarobnjak **uvođenja bazu podataka SQL poslužitelja za Microsoft Azure VM** koristite najnoviju verziju Microsoft SQL Server Management Studio za SQL Server. Najnoviju verziju ovog čarobnjaka sadrži netom najnovija ažuriranja Azure klasični portala i podržava najnoviji Azure VM slike u galeriji (starije verzije čarobnjaka možda neće raditi). Da biste dobili najnoviju verziju Microsoft SQL Server Management Studio za SQL Server, [preuzmite ga](http://go.microsoft.com/fwlink/?LinkId=616025) i instalirajte ga na klijentskom računalu s povezivanjem baze podataka koje ste planirali za migriranje i s Internetom.

### <a name="configure-the-existing-azure-virtual-machine-and-sql-server-instance-if-applicable"></a>Konfiguriranje postojeće Azure virtualni stroj i instance SQL Server (Ako je primjenjivo)

Ako premještate postojeće Azure VM, potrebni su sljedeći konfiguracijske korake:

- Konfigurirajte Azure VM i instanca SQL poslužitelja za omogućavanje Povezivost s drugog računala slijedeći korake za [Povezivanje s instancom SQL Server VM iz SSMS na drugom računalu](virtual-machines-windows-sql-connect.md). Podržane su samo 2014 SQL poslužitelja i SQL Server 2016 slike u galeriji ako su migracije pomoću čarobnjaka.
- Konfiguriranje Otvori krajnje točke za servis SQL Server oblak prilagodnik na Microsoft Azure pristupnik s privatnim priključak 11435. Ovaj priključak je kreiran kao deo SQL Server 2014 ili SQL Server 2016 dodjeljivanje Microsoft Azure VM. Oblak prilagodnik stvara pravilo vatrozida za Windows da biste dopustili njegovo dolazne veze TCP na zadani priključak 11435. Ovaj krajnje točke omogućuje čarobnjak mogu koristiti servis oblak prilagodnik za kopiranje datoteke sigurnosne kopije iz instance lokalno Azure VM. Za dodatne informacije pogledajte [Oblak prilagodnik za SQL Server](https://msdn.microsoft.com/library/dn169301.aspx).

    ![Stvaranje oblak prilagodnik krajnje točke](./media/virtual-machines-windows-migrate-sql/cloud-adapter-endpoint.png)

### <a name="run-the-use-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>Pokrenite korištenje uvođenja SQL Server baza podataka Microsoft Azure VM čarobnjak

1. Otvorite Microsoft SQL Server Management Studio za Microsoft SQL Server 2016 i povezati instanca SQL Server sadrži baze podataka korisnika koji će migracije na Azure VM.
2. Baze podataka koje premještate desnom tipkom miša, pokažite na Zadaci i zatim pritisnite uvođenja Microsoft Azure VM.

    ![Pokretanje čarobnjaka](./media/virtual-machines-windows-migrate-sql/start-wizard.png)

3. Na uvodnoj stranici kliknite Dalje.
4. Na stranici Postavke izvora povezati instanca SQL Server sadrži bazu podataka koju namjeravate migrirati Azure VM.
5. Navedite privremeno mjesto datoteke sigurnosne kopije. Ako se povezujete s udaljenim poslužiteljem, morate navesti mrežni pogon.

    ![Postavke izvora](./media/virtual-machines-windows-migrate-sql/source-settings.png)

6. Pritisnite Dalje.
7. Na stranici Microsoft Azure prijavu kliknite prijava i prijavu u Azure račun.
8. Odaberite pretplate koje želite koristiti i kliknite Dalje.

    ![Azure prijavu u](./media/virtual-machines-windows-migrate-sql/azure-signin.png)

9. Na stranici Postavke uvođenja možete odrediti novi ili postojeći servis oblak naziv i naziv virtualnog računala:

 - Odredite novi naziv oblaka naziv i servisa virtualni stroj za stvaranje novog servisa oblak s nove Azure virtualni stroj pomoću SQL Server 2014 ili SQL Server 2016 Galerija slika.

     - Odredite novi naziv oblaka usluge, navedite pohranu račun koji ćete koristiti.

     - Ako navedete postojeći naziv oblaka usluge, pohranu računa bit će dohvatiti i unijeli za vas.

 - Navedite naziv postojeće oblak usluge i novi naziv virtualni stroj za stvaranje nove Azure virtualni stroj u postojeći servis oblaka. Navedite samo SQL Server 2014 ili SQL Server 2016 Galerija slika.
 - Navedite naziv postojeće oblak usluge i naziv virtualni stroj za korištenje postojeće Azure virtualni stroj. Morate sliku ugrađen pomoću SQL Server 2014 ili SQL Server 2016 Galerija slika.

        ![Deployment Settings](./media/virtual-machines-windows-migrate-sql/deployment-settings.png)

10. Kliknite postavke
  - Ako ste naveli naziv postojeće oblak usluge i naziv virtualnog računala, sustav će zatražiti korisničko ime i lozinku.

        ![Postavke Azure stroj](./media/virtual-machines-windows-migrate-sql/azure-machine-settings.png)

    - Ako navedeni novi naziv virtualnog računala će zatražiti odabir slike iz popis galerije slika i unesite sljedeće informacije:
      - Slike – odaberite SQL poslužitelj 2014 ili SQL Server 2016
        - Korisničko ime
        - Nova lozinka
        - Potvrdi lozinku
        - Mjesto
        - Veličina.
    - Nadalje, kliknite da biste prihvatili sama generirani certifikata za ovaj novi Microsoft Azure virtualni stroj, a zatim kliknite u redu.

    ![Azure nove postavke stroja](./media/virtual-machines-windows-migrate-sql/azure-new-machine-settings.png)

11. Navedite naziv ciljne baze podataka ako je različit od izvora naziv baze podataka. Ako ciljna baza podataka već postoji, sustav će automatski prirast naziv baze podataka umjesto prebrisati postojeće baze podataka.
12. Kliknite Dalje, a zatim kliknite Završi.

    ![Rezultati](./media/virtual-machines-windows-migrate-sql/results.png)

13. Kad čarobnjak dovrši, povezati virtualni stroj i provjerite da je migrirati bazu podataka.
14. Ako ste stvorili novi virtualni stroj, konfigurirajte Azure virtualni stroj i SQL Server instanci slijedeći korake za [Povezivanje s instancom SQL Server VM iz SSMS na drugom računalu](virtual-machines-windows-sql-connect.md).

## <a name="backup-to-file-and-copy-to-vm-and-restore"></a>Sigurnosne kopije datoteka i Kopiraj VM i vraćanje

Ovaj postupak koristite kada nije moguće koristiti SQL Server baze podataka Microsoft Azure VM Čarobnjak za uvođenje ili jer su migracije za verziju sustava SQL Server prije 2014 SQL poslužitelja ili je veća od 1 TB datoteku sigurnosne kopije. Ako je veća od 1 TB sigurnosne kopije datoteka, morate je stripe jer je maksimalna veličina VM disk 1 TB. Koristite sljedeće općenite korake Migriraj bazu podataka korisnika pomoću ručna metoda:

1.  Sigurnosno kopiranje cijelog baze podataka na lokalno mjesto.
2.  Stvaranje ili prenesite virtualni stroj s verzijom sustava SQL Server to želite.
3.  Postavljanje connectivity prema svojim potrebama. Pogledajte [Povezivanje s SQL Server virtualni stroj na Azure (Upravitelj resursa)](virtual-machines-windows-sql-connect.md).
4.  Kopiranje sigurnosne kopije datoteke vašeg VM pomoću udaljene radne površine, Windows Explorer ili naredbe Kopiraj iz naredbenog retka.

## <a name="backup-to-url-and-restore"></a>Sigurnosno kopiranje i vraćanje URL

Koristite metoda [stvaranja sigurnosne kopije URL](https://msdn.microsoft.com/library/dn435916.aspx) kada uvođenje baze podataka SQL poslužitelja Microsoft Azure VM Čarobnjak za nije moguće koristiti jer je veća od 1 TB datoteku sigurnosne kopije i su migracije iz i SQL Server 2016. Za baze podataka manja od 1 TB ili verziju sustava SQL Server prije 2016 SQL poslužitelja, preporučuje se korištenje čarobnjaka. S SQL Server 2016 prugama sigurnosne kopije skupova podržani, su preporučeno za performanse i potrebna premašuje ograničenja veličine po blob. Za vrlo velike baze podataka preporučuje se korištenje [Servisa Windows uvoz i izvoz](../storage/storage-import-export-service.md) .

## <a name="detach-and-copy-to-url-and-attach-from-url"></a>Odvojiti i kopirajte URL i priložiti iz URL-a

Ovaj postupak koristite kada planirate za [spremanje ove datoteke pomoću usluga spremišta blobova Azure](https://msdn.microsoft.com/library/dn385720.aspx) i priložiti SQL Server pokrenut Azure VM, naročito s vrlo velike baze podataka. Koristite sljedeće općenite korake Migriraj bazu podataka korisnika pomoću ručna metoda:

1.  Odvajanje datoteke baze podataka iz lokalno instanca baze podataka.
2.  Kopirajte datoteke odvojeni baze podataka u Azure blobova pomoću [uslužnog programa naredbenog retka AZCopy](../storage/storage-use-azcopy.md).
3.  Priložiti datoteke baze podataka Azure URL instance sustava SQL Server u Azure VM.

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>Pretvaranje VM i prenesite URL i uvesti kao novi VM

Koristite ovu metodu za migraciju sve baze podataka sustava i korisniku u SQL Server instanci lokalno Azure virtualni stroj. Koristite sljedeće korake Općenito preseliti cijelu SQL Server instanci pomoću ručna metoda:

1.  Pretvorite fizičke ili virtualne strojeva značajke Hyper-V VHDs pomoću [Microsoft virtualni stroj pretvornik](http://technet.microsoft.com/library/dn873998.aspx).
2.  Prenesite datoteke VHD pohranu Azure pomoću [cmdleta Dodaj AzureVHD](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx).
3.  Uvođenje novi virtualni stroj pomoću prenesene VHD.

> [AZURE.NOTE] Za migriranje cijelu aplikaciju, razmislite o korištenju [Azure oporavak web-mjesta](../site-recovery/site-recovery-overview.md)].

## <a name="ship-hard-drive"></a>Otpremi tvrdi disk

Koristite [način uvoza i izvoza servisa Windows](../storage/storage-import-export-service.md) za prijenos velikih količina datoteka podataka spremišta blobova Azure u situacijama u kojima prijenos putem mreže je previsok ili nije izvedivo. S ovom uslugom poslati jednu ili više tvrdih diskova koji sadrže podatke Azure podataka centar, gdje prenijeti podatke u spremište računa.

## <a name="next-steps"></a>Sljedeći koraci

Dodatne informacije o izvodi SQL Server na Azure virtualnih računala potražite [SQL Server na pregled Azure virtualnih računala](virtual-machines-windows-sql-server-iaas-overview.md).

Upute za stvaranje virtualni stroj Azure SQL Server iz snimljenu sliku, pogledajte [savjete & štihova na 'kloniranje' Azure SQL virtualnih računala iz uhvaćene slike](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/) na blogu inženjeri CSS SQL Server.
