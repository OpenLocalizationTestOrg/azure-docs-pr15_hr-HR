<properties 
    pageTitle="Praktični vodič: Azure Active Directory integracije sa softverom OfficeSpace | Microsoft Azure" 
    description="Saznajte kako koristiti softver OfficeSpace s Azure Active Directory da biste omogućili jedinstvenu prijavu, automatiziranog dodjele resursa i više!" 
    services="active-directory" 
    authors="jeevansd"  
    documentationCenter="na" 
    manager="femila"/>
<tags 
    ms.service="active-directory" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="identity" 
    ms.date="09/29/2016" 
    ms.author="jeedes" />

#<a name="tutorial-azure-active-directory-integration-with-officespace-software"></a>Praktični vodič: Azure Active Directory integracije sa softverom OfficeSpace
  
Cilj ovog praktičnog vodiča je da bi se prikazala integraciju sa servisom Azure i OfficeSpace softver.  
Scenarij navedene u ovom ćete praktičnom vodiču pretpostavlja da ste već sljedećih stavki:

-   Valjani Azure pretplate
-   Na softver OfficeSpace jedinstvenu prijavu omogućeno pretplate
  
Po dovršetku ovog praktičnog vodiča Azure AD korisnika koje ste dodijelili OfficeSpace softver će moći znak u aplikaciju na vašem OfficeSpace softver tvrtke (znak servisa davatelja pokrenut na), ili pomoću [Uvod u Access ploča](active-directory-saas-access-panel-introduction.md).
  
Scenarij navedene u ovom ćete praktičnom vodiču sastoji se od sljedećih sastavni blokovi:

1.  Omogućivanje integraciju aplikacija za OfficeSpace softver
2.  Konfiguriranje jedinstvenu prijavu
3.  Konfiguriranje dodjeljivanje korisnika
4.  Dodjela korisnika

![Scenarij] (./media/active-directory-saas-officespace-software-tutorial/IC777764.png "Scenarij")
##<a name="enabling-the-application-integration-for-officespace-software"></a>Omogućivanje integraciju aplikacija za OfficeSpace softver
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti integraciju aplikacija za OfficeSpace softvera.

###<a name="to-enable-the-application-integration-for-officespace-software-perform-the-following-steps"></a>Da biste omogućili integraciju aplikacija za OfficeSpace softver, učinite sljedeće:

1.  Azure klasični portalu na lijevom navigacijskom oknu kliknite **Servisa Active Directory**.

    ![Active Directory] (./media/active-directory-saas-officespace-software-tutorial/IC700993.png "Active Directory")

2.  Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3.  Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija] (./media/active-directory-saas-officespace-software-tutorial/IC700994.png "Aplikacija")

4.  Kliknite **Dodaj** pri dnu stranice.

    ![Dodavanje aplikacije] (./media/active-directory-saas-officespace-software-tutorial/IC749321.png "Dodavanje aplikacije")

5.  U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Dodavanje aplikacije iz gallerry] (./media/active-directory-saas-officespace-software-tutorial/IC749322.png "Dodavanje aplikacije iz gallerry")

6.  U **okvir za pretraživanje**upišite **OfficeSpace softvera**.

    ![Galerija aplikacije] (./media/active-directory-saas-officespace-software-tutorial/IC777765.png "Galerija aplikacije")

7.  U oknu s rezultatima odaberite **OfficeSpace softver**, a zatim **Dovršeno** da biste dodali aplikaciju.

    ![OfficeSpace softver] (./media/active-directory-saas-officespace-software-tutorial/IC781007.png "OfficeSpace softver")
##<a name="configuring-single-sign-on"></a>Konfiguriranje jedinstvenu prijavu
  
Cilj ovaj odjeljak je Strukturiranje kako omogućiti korisnicima za provjeru autentičnosti OfficeSpace softver sa svojim računom u Azure AD pomoću vanjskog pristupa na temelju SAML protokol.  
Konfiguriranje jedinstvene prijave za OfficeSpace softver potreban za dohvaćanje vrijednosti otisak prsta s certifikat.  
Ako niste upoznati s taj postupak, potražite u članku [upute za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI).

###<a name="to-configure-single-sign-on-perform-the-following-steps"></a>Da biste konfigurirali jedinstvenu prijavu, poduzmite sljedeće korake:

1.  Azure klasični portalu na stranici za integraciju aplikacije **Softver OfficeSpace** kliknite **Konfiguriraj jedinstvenu prijavu** da biste otvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje znak = na] (./media/active-directory-saas-officespace-software-tutorial/IC777766.png "Konfiguriranje znak = na")

2.  Na stranici **kako biste željeli korisnika da biste se prijavili OfficeSpace softver** , odaberite **Microsoft Azure AD jedinstvenu prijavu**, a zatim kliknite **Dalje**.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-officespace-software-tutorial/IC777767.png "Konfiguriranje jedinstvenu prijavu")

3.  Na stranici **Konfiguriranje URL adresa Web App** u tekstni okvir **OfficeSpace softver na URL za prijavu** upišite URL koji se koristi korisnika da biste se prijavili aplikacija OfficeSpace softver (npr.: "*https://company.officespacesoftware.com*"), a zatim kliknite **Dalje**.

    ![Konfiguriranje URL adresa Web App] (./media/active-directory-saas-officespace-software-tutorial/IC775556.png "Konfiguriranje URL adresa Web App")

4.  Na stranici **Konfiguracija jedinstvenu prijavu na OfficeSpace softver** da biste preuzeli certifikata, kliknite **Preuzmite certifikat**i spremanje datoteka certifikata lokalno na vašem računalu.

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-officespace-software-tutorial/IC793769.png "Konfiguriranje jedinstvenu prijavu")

5.  U prozoru preglednika drugoj web, prijavite se u web-mjesto tvrtke OfficeSpace softver kao administrator.

6.  Idite na **administrator \> poveznika**.

    ![Administrator] (./media/active-directory-saas-officespace-software-tutorial/IC777769.png "Administrator")

7.  Kliknite **SAML autorizacije**.

    ![Poveznika] (./media/active-directory-saas-officespace-software-tutorial/IC777770.png "Poveznika")

8.  U odjeljku **SAML autorizacije** poduzeti sljedeće korake:

    ![Konfiguriranje SAML] (./media/active-directory-saas-officespace-software-tutorial/IC777771.png "Konfiguriranje SAML")

    1.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na softver OfficeSpace** kopirajte **URL daljinskog prijava** vrijednost i pa ih zalijepite u tekstni okvir **url davatelja odjavite** .
    2.  Azure klasični portalu na dijaloški okvir stranici **Configure jedinstvenu prijavu na softver OfficeSpace** kopirajte **URL daljinskog odjavite** vrijednost i pa ih zalijepite u tekstni okvir **url ciljne idp klijenta** .
    3.  Kopirajte vrijednost **otisak prsta** iz izvezene potvrde i pa ih zalijepite u tekstni okvir **otiska prsta klijent idp certifikata** .  

        >[AZURE.TIP]
        Dodatne informacije potražite u članku [za dohvaćanje vrijednosti otisak prsta na certifikata](http://youtu.be/YKQF266SAxI)

    4.  Kliknite **Spremi postavke**.

9.  Azure klasični portala odaberite potvrdu jedan konfiguracije za prijavu, a zatim **Dovršeno** da biste zatvorili dijaloški okvir **Konfiguriranje jedinstvenu prijavu** .

    ![Konfiguriranje jedinstvenu prijavu] (./media/active-directory-saas-officespace-software-tutorial/IC777772.png "Konfiguriranje jedinstvenu prijavu")
##<a name="configuring-user-provisioning"></a>Konfiguriranje dodjeljivanje korisnika
  
Da biste omogućili Azure AD korisnika da se prijavite u OfficeSpace softver, oni mora biti dodijeljena u OfficeSpace softver. U slučaju softver OfficeSpace dodjele resursa je automatizirana zadatak.  
Postoji nijedna stavka akcija za vas.  
Korisnicima se automatski stvaraju po potrebi tijekom prvog jedan prijave pokušaj.

>[AZURE.NOTE]Možete koristiti bilo koji drugi OfficeSpace softver korisnički račun alate za stvaranje ili API-ji nudi OfficeSpace softver za dodjeljivanje AAD korisničke račune.

##<a name="assigning-users"></a>Dodjela korisnika
  
Da biste testirali konfiguraciju, morate dodijeliti Azure AD korisnicima želite omogućiti korištenje aplikacije programa access da biste ga tako da im dodijelite.

###<a name="to-assign-users-to-officespace-software-perform-the-following-steps"></a>Da biste korisnicima dodijelili OfficeSpace softver, poduzmite sljedeće korake:

1.  Azure klasični portalu stvorite testnom računu.

2.  Na stranici za integraciju aplikacije **Softver OfficeSpace **kliknite **dodijeliti korisnicima**.

    ![Dodjela korisnika] (./media/active-directory-saas-officespace-software-tutorial/IC777773.png "Dodjela korisnika")

3.  Odaberite korisnički test, kliknite **Dodijeli**, a zatim **da** da biste potvrdili vaše dodjele.

    ![Da] (./media/active-directory-saas-officespace-software-tutorial/IC767830.png "Da")
  
Ako želite testirajte postavke jedan prijave, otvorite ploča programa Access. Dodatne pojedinosti o ploča programa Access potražite u članku [Uvod u oknu programa Access](active-directory-saas-access-panel-introduction.md).