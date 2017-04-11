<properties
    pageTitle="Praktični vodič: Azure Active Directory Integracija s Evidence.com | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Evidence.com."
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/23/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-evidencecom"></a>Praktični vodič: Azure Active Directory Integracija s Evidence.com

Cilj ovog praktičnog vodiča je da biste prikazali kako postaviti jedinstvenu prijavu između Azure Active Directory (AAD) i Evidence.com. Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:
    
* Valjanu pretplatu Microsoft Azure
* Pretplatu na Evidence.com s jedine prijave omogućeno (e-pošte earlyaccess@evidence.com ako utemeljene na SAML jedine prijave je omogućeno)

Po dovršetku ovog praktičnog vodiča AAD korisnika kojemu ste dodijelili pristup Evidence.com će moći znak u aplikaciju pomoću AAD ploče programa Access.

## <a name="add-evidencecom-to-your-directory"></a>Dodavanje Evidence.com direktorija

U ovom se odjeljku opisuje kako dodati Evidence.com kao integrirane aplikacije servisa Azure Active Directory.

**Da biste omogućili integraciju aplikacija za dokaz:**

1.  [Azure klasični portal](https://manage.windowsazure.com)u lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

4.  Da biste otvorili galeriju aplikaciju, kliknite **Dodaj**, a zatim **Dodavanje aplikacije iz galerije**.

5.  U okvir za pretraživanje upišite **Evidence.com**.

6.  U oknu s rezultatima odaberite **Evidence.com**, a zatim **Dovršeno** da biste dodali aplikaciju.


## <a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu

U ovom se odjeljku opisuje kako omogućiti korisnicima za provjeru autentičnosti Evidence.com s računa u Azure Active Directory, pomoću vanjskog pristupa na temelju SAML protokol.

**Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:**

1.  Nakon dodavanja Evidence.com na portalu za Azure klasični, kliknite **Konfiguriraj jedinstvenu prijavu**. 
 
2.  Na sljedećem zaslonu odaberite **Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

3.  Na zaslonu za konfiguriranje aplikacije URL unesite URL gdje korisnici će se prijavite Evidence.com klijentu URL-a (primjer: https://yourtenant.evidence.com, a zatim kliknite **Dalje**. 

4.  Kliknite vezu za **Preuzimanje certifikat** i spremanje na lokalni disk. Da biste postavili jedinstvene Prijave na web-mjestu Evidence.com će se koristiti certifikat i metapodataka URL-ovima (ID entitet, SSO URL za prijavu i znak Out URL-a). 

5.  U prozoru zasebnom web-preglednika, prijavite se na vaše Evidence.com smjernice kao administrator, a zatim otvorite karticu za **administratore**
      
6.  Kliknite na **agencija jedine prijave**
 
7.  Odaberite **SAML na temelju znak**
 
8.  Kopirajte **URL izdavač** **Jedinstvenu prijavu** i **Jedan Odjava** vrijednosti prikazane na portalu za Azure klasični i odgovarajuća polja u Evidence.com.

9.  Otvaranje certifikat preuzeti u koraku 4 u uređivaču teksta kao što su Notepad.exe, i kopirajte i zalijepite sadržaj u okvir za **Sigurnosni certifikat** . 

10. Spremite konfiguraciju u Evidence.com.
 
11. Azure klasični portalu potvrdite **potvrdite da ste konfigurirali jedinstvenu prijavu kao što je opisano iznad**. Provjera to će se omogućiti trenutni certifikat za početak rada za ovaj okvir aplikacije.
 
12. Na stranici za potvrdu jedan prijave kliknite **dovrši**.  


## <a name="creating-an-evidencecom-test-user"></a>Stvaranje korisnik za testiranje sustava Evidence.com

Za Azure AD da korisnici mogu se prijaviti, oni mora biti dodijeljena za pristup unutar Evidence.com aplikacije. U ovom se odjeljku opisuje kako stvoriti korisničke račune za Azure AD unutar Evidence.com

**Dodjela resursa za korisnički račun u Evidence.com:**

1.  U prozoru web-preglednika, prijavite se u web-mjesto tvrtke Evidence.com kao administrator.

2.  Idite na karticu **administrator** .

3.  Kliknite **Dodaj korisnika**.

4.  Kliknite gumb **Dodaj** .

5.  **Adresu e-pošte** korisnika dodani mora podudarati korisničko ime korisnika u Azure AD koji želite dopustiti pristup. Ako je adresa korisničko ime i e-pošte nisu iste vrijednosti u tvrtki ili ustanovi, možete koristiti u **Evidence.com > atribute > jedinstvenu prijavu** dio Azure klasični portal da biste promijenili nameidenitifer poslati Evidence.com se adresa e-pošte.


## <a name="assigning-users-to-evidencecom"></a>Dodjeljivanje korisnika Evidence.com

Dodjela resursa AAD korisnici mogu vidjeti Evidence.com njihove teksta, oni mora biti dodijeljena pristup unutar Azure klasični portal.

**Da biste korisnicima dodijelili Evidence.com:**

1.  Na stranici brzi početak rada za Evidence.com na portalu za Azure klasični kliknite **dodijeliti korisnicima Evidence.com**.
 
2.  Na izborniku **Prikaz** odaberite želite li dodijeliti korisnika ili grupu Evidence.com, a zatim kliknite gumb s kvačicom.
 
3.  Na popisu **korisnici** odaberite korisnika u grupu u kojoj želite dodijeliti Evidence.com.
 
4.  U podnožje stranice, kliknite gumb **dodijeliti** .

