<properties
    pageTitle="Dodavanje davatelja Web Apps resursa Azure snop | Microsoft Azure"
    description="Detaljne upute o implementaciji Web Apps u stogu Azure"
    services="azure-stack"
    documentationCenter=""
    authors="ccompy, apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>

# <a name="add-a-web-apps-resource-provider-to-azure-stack"></a>Dodavanje davatelja resursa web-aplikacije snop Azure

> [AZURE.NOTE] Sljedeće informacije odnosi se samo na Azure stogu TP1 implementacije.

Dodavanje davatelja usluge Web Apps resursa Azure snop ima sedam koraka:

1.  Preuzmite potrebne komponente.
2.  Stvaranje certifikata da se koriste Azure stogu web-aplikacije.
3.  Pomoću instalacijski program preuzimanje faza i instalirajte Azure stogu web-aplikacije. 
4.  Provjerite valjanost instalacije aplikacija za Web.
5.  Stvaranje DNS zapisa za sučelje i poslužitelj za upravljanje učitavanja balancers.
6.  Registrirajte se upravo distribuiranih davatelja resursa web-aplikacije pomoću OKVIRA.
7.  Probnu vožnju davatelja resursa za aplikacije za Web.

## <a name="download-required-components"></a>Preuzmite potrebne komponente

1.  Preuzmite [instalacijski program za pretpregled aplikacije servisa za Azure stogu](http://aka.ms/azasinstaller). 
2.  Preuzimanje [aplikacije servisa za Azure stogu implementacije Pomoćnik za skripte](http://aka.ms/azashelper). 
3.  Izdvajanje datoteka iz Pomoćnik za skripte zip datoteke, mora biti tri skripte:
    - Stvaranje AppServiceCerts.ps1
    - Stvaranje AppServiceDnsRecords.ps1
    - REGISTER-AppServiceResourceProvider.ps1 

## <a name="create-certificates-to-be-used-by-azure-stack-web-apps"></a>Stvaranje certifikata da se koriste Azure stogu web-aplikacije

U ovom prvom skripte funkcionira s Azure stogu ustanova za izdavanje potvrda da biste stvorili 3 potvrde koje su potrebne za web-aplikacije. Pokrenuti skriptu na ClientVM jamči da koristite PowerShell kao azurestack\administrator:
1.  U sesije PowerShell izvodi kao **azurestack\administrator**izvršiti skripte za **Stvaranje AppServiceCerts.ps1** .  Time ste stvorili tri potvrde u istu mapu kao i skripte koje su potrebne za web-aplikacije.
2.  Unesite lozinku za sigurnu pfx datotekama i zabilježite ga kao morat ćete unijeti u Azure stogu Web Apps instalacijski program sustava.

## <a name="use-the-installer-to-download-and-install-azure-stack-web-apps"></a>Korištenje instalacijski program da biste preuzeli i instalirali Azure stogu web-aplikacije

Instalacijski program appservice.exe će:
1.  Pitaj korisnika da biste prihvatili Microsoft i trećih strana EULAs.
2.  Prikupljanje informacija implementacije Azure stogu.
3.  Stvaranje spremnika blob u račun za pohranu Azure stogu naveden.
4.  Preuzimanje datoteke potrebne za instalaciju davatelja resursa Azure stogu web-aplikacije.
5.  Priprema instalacije za implementaciju davatelja resursa web-aplikacije u okruženju Azure stogu.
6.  Prenesite datoteke na račun aplikacije servisa za pohranu.
7.  Nude informacije potrebne za izbaciti predloška Azure Voditelj resursa.

Sljedeće će vas voditi kroz stadije instalacije:

>[AZURE.NOTE] Da biste pokrenuli instalacijski program morate koristiti povećane račun (Lokalna ili domene administrator). Ako se prijaviti kao azurestack\azurestackuser, zatražit će se za dodatnim vjerodajnice. 

1.  Pokrenite appservice.exe kao **azurestack\administrator**. 
2.  Kliknite **uvođenja pomoću upravitelja resursa Azure**.

![Instalacijski program za Tehnički pretpregled 1 aplikacije servisa Azure stogu][1]

3.  Pregledajte i prihvatite licencne odredbe za Microsoftov softver predizdanje i zatim kliknite **Dalje**.
4.  Pregledajte i prihvatite uvjete treći partylicense i zatim kliknite **Dalje**.
5.  Pregledajte podatke o konfiguraciji aplikacije u Oblaku, a zatim kliknite **Dalje**.

![Azure stogu aplikacije servisa za Tehnički pretpregled 1 aplikacije servisa za oblak konfiguracija][2]

6. Kliknite **Poveži** (pokraj okvira Azure stogu pretplate).

![Azure stogu aplikacije servisa za Tehnički pretpregled 1 aplikacije servisa za oblak konfiguracija zaslona dva][3]

7.  U prozoru za provjeru autentičnosti stogu Azure pružaju **Azure Active Directory administrator servisa račun** i **lozinku**, a zatim kliknite **Prijava**.
**Bilješke:** Ovo je račun za Azure Active Directory koju ste naveli prilikom implementiran Azure stogu.
    - Kliknite **Strelicu dolje** na desnoj strani okvir pokraj **Azure stogu pretplate** , a zatim odaberite svoju pretplatu.

![Azure stogu aplikacije servisa za Tehnički pretpregled 1 pretplate odabira][5]

8.  Kliknite **Strelicu dolje** na desnoj strani okvir pokraj **Azure stogu mjesta**.
    - Odaberite **lokalnu**.
9. Unesite **naziv** za administratora.
10. Unesite **lozinku** za administratora.
11. Pregledajte **pojedinosti SQL Server** i napraviti promjene po potrebi.
12. Pregled **Računa za prijavu sustava** i napraviti promjene po potrebi.
13. Unesite **Lozinku sustava**.
14. Kliknite **Dalje**.  Sada instalacijski program sada ćete provjeriti pojedinosti veze za SQL Server navedene.

![Azure stogu aplikacije servisa za Tehnički pretpregled 1 pretplate odabira][4]    

15. Kliknite **Pregledaj** pokraj **Web-aplikacije zadani SSL certifikat datoteke** , a zatim otvorite **webapps. AzureStack.Local** certifikata [ste ranije stvorili](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
16. Unesite **lozinku za potvrdu** postavili prilikom stvaranja potvrde.
17. Kliknite **Pregledaj** pokraj **Resursa davatelja SSL certifikat datoteku** , a zatim otvorite Upravljanje **. AzureStack.Local** certifikata [ste ranije stvorili](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
18. Unesite **lozinku za potvrdu** postavili prilikom stvaranja potvrde.
19. Kliknite **Pregledaj** pokraj **Resursa davatelja korijenski certifikat datoteku** , a zatim otvorite Upravljanje **. AzureStack.Local** certifikata [ste ranije stvorili](#Create-Certificates-To-Be-Used-By-Azure-Stack-Web-Apps).
20. Kliknite **Dalje** instalacijski program će provjeriti certifikat lozinka.

![Detalji o certifikatu Tehnički pretpregled 1 za aplikacije servisa Azure stogu][6]

Uvođenje se oko 45 60 minuta da biste dovršili.

![Tijek instalacije za Azure stogu aplikacije servisa za Tehnički pretpregled 1][7]

21. Kada se dovrši instalacijski program uspješno kliknite **Izlaz**.

## <a name="validate-web-apps-installation"></a>Provjerite valjanost instalacije aplikacija za Web

1.  Na **Glavnom računalu za Azure snop** otvorite **Upravitelj Hyper-V**.
2.  Pronađite **CN0 VM** i **Povezivanje** na VM.
![Upravitelj Hyper-V Tehnički pretpregled 1 aplikacije servisa za Azure stogu][8]
3.  Na radnoj površini ovaj VM dvokliknite **Web oblaka Management Console**.
![Azure stogu aplikacije servisa za Tehnički pretpregled 1 Management Console][9]
4.  Dođite do **upravlja poslužiteljima**.
5.  Kada se na svim računalima **spremni** nastaviti na sljedeći korak. 
![Azure stogu aplikacije servisa za Tehnički pretpregled 1 upravljanih poslužitelje statusa][10]

## <a name="create-dns-records-for-the-management-server-and-front-end-load-balancers"></a>Stvaranje DNS zapisa za poslužitelj za upravljanje i balancers sučelje učitavanja
1.  Otvorite instancu komponente PowerShell kao **azurestack\administrator**.
2.  Pomaknite se do mjesta skripte preuzimaju i izdvojene u [pripremni korak](#Download-Required-Components).
3.  Pokrenuti skriptu **Stvaranje AppServiceDnsRecords.ps1** , time se stvara DNS stavke da biste omogućili portal i web-aplikacije zahtjeve moguće usmjeriti na prednju poslužitelje Završi.  Tijekom implementacije ARM dva softver učitavanja Balancers (SLBs), web-aplikacijama stvorene u davatelja resursa mreže. Oni pokažite na poslužitelje za upravljanje i poslužitelji sučelje. Portal i ARM temelji zahtjevi za davatelja resursa aplikacije servisa za Azure stogu idite na poslužitelju za upravljanje.

## <a name="register-the-newly-deployed-azure-stack-web-apps-provider-with-arm"></a>Morate registrirati upravo distribuiranih davatelja Azure stogu web-aplikacije ARM
1.  Otvorite instancu komponente PowerShell kao **azurestack\administrator**.
2.  Pomaknite se do mjesta skripte preuzimaju i izdvojene u [pripremni korak](#Download-Required-Components).
3.  Pokrenuti skriptu **Register AppServiceResourceProvider.ps1** . 

>[AZURE.NOTE] Unesite korisničko ime i lozinku **točno (uključujući velika i mala slova)** kao što je unesen za **Virtual_Machine Administrator** i polja za **lozinku** u instalaciji ili svoju registraciju davatelja resursa neće uspjeti.

## <a name="test-drive-azure-stack-web-apps"></a>Testiranje pogon Azure stogu web-aplikacije

Sad kad ste implementiran i registrirana davatelja resursa web-aplikacije, možete testirati da biste provjerili jesu li drugih korisnika možete implementirati web-aplikacije.

1.  Na portalu za Azure stogu kliknite Novo, kliknite Web + Mobile pa kliknite web-aplikacija.
2.  U plohu Web App, upišite naziv u okvir Web app.
3.  U odjeljku grupe resursa, kliknite Novo, a zatim upišite naziv u okvir za grupu resursa. 
4.  Kliknite aplikacije servisa za planiranje/mjesto, a zatim kliknite Stvori novu.
5.  U plan plohu aplikacije servisa za upišite naziv u okvir aplikacije servisa za planiranje.
6.  Kliknite sloju određivanje cijena kliknite besplatno zajedničko ili zajednički se koristi zajednički, kliknite Odaberi, kliknite u redu i kliknite Stvori.
7.  U u odjeljku nekoliko minuta, pločica za nova web-aplikaciji pojavit će se na nadzornoj ploči. Kliknite pločicu.
8.  U aplikaciji plohu web kliknite Pregledaj da biste pogledali zadano web-mjesto za ovu aplikaciju.


**Implementacija WordPress, DNN ili Django web-mjesto (neobavezno)**

1. Na portalu za Azure stogu kliknite "+", otvorite trgovinu Azure implementacija Django web-mjesta i pričekajte za uspješan dovršetak. Platforme web Django koristi utemeljene na sustavu datoteke baze podataka, a ne zahtijeva Neki davatelji dodatne resurse kao što su SQL ili MySQL.  

2. Ako se implementiran davatelja MySQL resursa, možete implementirati WordPress web-mjesto iz trgovine. Kada se od vas zatraži za parametre baze podataka, unos korisničko ime kao *User1@Server1* (pomoću korisničkog imena i naziva poslužitelja po izboru).

3. Ako se implementiran davatelja resursa za SQL Server, možete implementirati DNN web-mjesto iz trgovine. Kada se od vas zatraži za parametre baze podataka, odabir baze podataka na računalu s operacijskim sustavom SQL Server koji je povezan s davatelja resursa.

## <a name="next-steps"></a>Daljnji koraci

Možete isprobajte i druge [platforme kao servisi za servis (PaaS)](azure-stack-tools-paas-services.md), kao što je [davatelj usluga za SQL Server resursa](azure-stack-sql-rp-deploy-short.md) i [davatelja MySQL resursa](azure-stack-mysql-rp-deploy-short.md).

<!--Image references-->
[1]: ./media/azure-stack-webapps-deploy/AppService_exe_Start.png
[2]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep1.png
[3]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2.png
[4]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_populated.png
[5]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep2_SubscriptionSelection.png
[6]: ./media/azure-stack-webapps-deploy/AppService_exe_DefaultEntriesStep3_Certificates.png
[7]: ./media/azure-stack-webapps-deploy/AppService_exe_InstallationProgress.png
[8]: ./media/azure-stack-webapps-deploy/HyperV.png
[9]: ./media/azure-stack-webapps-deploy/MMC.png
[10]: ./media/azure-stack-webapps-deploy/ManagedServers.png


<!--Links-->
[Azure_Stack_App_Service_preview_installer]: http://go.microsoft.com/fwlink/?LinkID=717531
[WebAppsDeployment]: http://go.microsoft.com/fwlink/?LinkId=723982
[AppServiceHelperScripts]: http://go.microsoft.com/fwlink/?LinkId=733525
