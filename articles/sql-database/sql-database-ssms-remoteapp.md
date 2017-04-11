<properties
    pageTitle="Povezivanje s bazom podataka SQL Azure RemoteApp pomoću SQL Server Management Studio | Microsoft Azure"
    description="Pomoću ovog praktičnog vodiča da biste saznali kako koristiti SQL Server Management Studio Azure RemoteApp za sigurnost i performanse prilikom povezivanja s bazom podataka SQL"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>Korištenje SQL Server Management Studio Azure RemoteApp da biste se povezali s bazom podataka SQL

## <a name="introduction"></a>Uvod  
Pomoću ovog praktičnog vodiča pokazuje kako koristiti SQL Server Management Studio (SSMS) u Azure RemoteApp da biste se povezali s bazom podataka SQL. Ga vodit će vas kroz postupak postavljanja sustava SQL Server Management Studio RemoteApp Azure, u članku se objašnjava prednosti i prikazuje sigurnosnim značajkama koje možete koristiti u servisu Azure Active Directory.

**Procijenjena vrijeme da biste dovršili:** 45 minuta

## <a name="ssms-in-azure-remoteapp"></a>SSMS Azure RemoteApp

Azure RemoteApp je usluga ZAPISI u Azure koje nudi aplikacije. Dodatne informacije o tome ovdje: [što je RemoteApp?](../remoteapp/remoteapp-whatis.md)

SSMS koji se izvodi u Azure RemoteApp daje iste iskustvo kao SSMS izvodi lokalno.

![Snimka zaslona s prikazom SSMS koji se izvodi u Azure RemoteApp][1]



## <a name="benefits"></a>Prednosti

Postoje brojne prednosti korištenja SSMS RemoteApp Azure, uključujući:

- Priključkom 1433 na Azure SQL Server nemate izložiti vanjsko (izvan Azure).
- Ne morate zadržati dodavanju i uklanjanju IP adresa u vatrozidu za Azure SQL Server.
- Sve veze za Azure RemoteApp odvijaju putem HTTP priključak 443 pomoću šifrirane protokola udaljene radne površine
- To je više korisnika, a mogu mijenjati veličinu.
- Postoji performanse rast onemogućite SSMS u području isti kao SQL baze podataka.
- Možete nadzirati korištenje Azure RemoteApp s Premium edition Azure Active Directory koja ima izvješća o aktivnosti korisnika.
- Možete omogućiti višestruke provjere autentičnosti (MFA).
- Pristup SSMS bilo gdje pri korištenju bilo koji podržani klijenti Azure RemoteApp koji obuhvaća iOS, Android, Mac, Windows Phone i Windows PC.


## <a name="create-the-azure-remoteapp-collection"></a>Stvaranje zbirke Azure RemoteApp

Evo nekoliko koraka da biste stvorili zbirka Azure RemoteApp SSMS:


### <a name="1-create-a-new-windows-vm-from-image"></a>1. stvoriti novi Windows VM sa slike
Da bi vaše nove VM koristiti "Windows Server udaljene radne površine sesiju glavno računalo Windows Server 2012 R2" slike iz galerije.


### <a name="2-install-ssms-from-sql-express"></a>2. Instalirajte SSMS SQL Express

Idite na novu VM, a zatim otvorite stranicu za preuzimanje: [Express Microsoft® SQL Server® 2014.](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

Postoji mogućnost da biste preuzimali samo SSMS. Nakon preuzimanja odlaze direktorija za instalaciju i pokrenite instalaciju SSMS.

Morate instalirati SQL Server 2014 Service Pack 1. Ovdje preuzmite: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 sadrži ključne funkcije za rad s bazom podataka SQL Azure.


### <a name="3-run-validate-script-and-sysprep"></a>3. Pokrenite provjeru skripte i Sysprep

Na radnoj površini u VM skriptu PowerShell zove Provjera valjanosti. Pokrenite tako da dvokliknete. Će provjeriti je li na VM spremni se koristi kao glavno računalo udaljene aplikacija. Po dovršetku provjere će vas pitati pokrenite sysprep-odaberite da biste ga pokrenuti.

Kada se dovrši sysprep, ona će se isključiti na VM.

Da biste saznali više o stvaranju Azure RemoteApp slike, pogledajte: [kako stvoriti predložak sliku RemoteApp servisu Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4. snimite sliku

Kada u VM prestao je pokrenut, pronađite je na portalu za trenutni, a ga osvojili.

Dodatne informacije o snimanju slike, potražite u članku [snimite sliku sadržaja sustava Azure Windows virtualnog računala stvorene pomoću model klasični implementacije](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. dodati slika Azure RemoteApp predložaka

U odjeljku Azure RemoteApp portala za trenutni idite na karticu slika predložaka, a zatim kliknite Dodaj. U okviru blokatora odaberite "Uvoz sliku iz biblioteke virtualnim strojevima", a zatim odaberite sliku koju ste upravo stvorili.



### <a name="6-create-cloud-collection"></a>6. Stvaranje zbirke oblaka

Na portalu trenutni stvorite novu zbirku Azure RemoteApp oblaka. Odaberite predložak sliku koju ste upravo uvezli SSMS na njemu instalirano.

![Stvaranje nove zbirke oblaka][2]


### <a name="7-publish-ssms"></a>7. objavljivanje SSMS

Objavljivanje na kartici nove zbirke oblaka, odaberite Objavi aplikacije s izbornika Start, a zatim odaberite SSMS s popisa.

![Objavljivanje aplikacija][5]

### <a name="8-add-users"></a>8. dodavanje korisnika

Na kartici korisničkog pristupa možete odabrati korisnike koji će imati pristup zbirku Azure RemoteApp koja sadrži samo SSMS.

![Dodavanje korisnika][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9. Instalirajte klijentske aplikacije Azure RemoteApp

Možete preuzeti i instalirati Azure RemoteApp klijent: [Preuzimanje | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Konfiguriranje Azure SQL Server

Konfiguracija samo potrebno je da biste bili sigurni servisa Azure Services omogućena za vatrozida. Ako koristite rješenja, pa ne morate da biste dodali sve IP adrese da biste otvorili vatrozid. Mrežni promet koji je dopušteno SQL Server je s drugih servisa za Azure.


![Dopusti Azure][4]



## <a name="multi-factor-authentication-mfa"></a>Višestruka provjera autentičnosti (MFA)

MFA mogu omogućiti za ovu aplikaciju posebno. Idite na karticu aplikacije Azure Active Directory. Stavku ćete pronaći Microsoft Azure RemoteApp. Ako kliknete tu aplikaciju, a zatim konfigurirati, vidjet ćete stranice ispod gdje možete omogućiti MFA za ovu aplikaciju.

![Omogućivanje MFA][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Nadzor aktivnosti korisnika s Azure Active Directory Premium

Ako nemate Azure AD Premium, morate ga uključili u odjeljku licence imenik. S Premium omogućena, možete dodijeliti korisnicima razinu Premium.

Kada se korisnik servisa Azure Active Directory, možete zatim idite na karticu aktivnosti da biste vidjeli podatke za prijavu za Azure RemoteApp.



## <a name="next-steps"></a>Daljnji koraci

Nakon dovršetka prema gore navedenim uputama će pokretati Azure RemoteApp klijenta i zapisnika u pomoću dodijeljenog korisnika. Koje će se prikazivati s SSMS kao jedan aplikacija, a možete ga pokrenuti kao i ako su instalirani na računalu s pristupom Azure SQL Server.

Dodatne informacije o tome kako povezivanje s bazom podataka SQL potražite u članku [za povezivanje s bazom podataka SQL s SQL Server Management Studio i obaviti primjer T SQL upita](sql-database-connect-query-ssms.md).


Sve to je za sada. Uživajte!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png
