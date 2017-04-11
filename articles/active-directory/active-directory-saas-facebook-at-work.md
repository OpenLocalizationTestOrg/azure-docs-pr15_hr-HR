<properties
    pageTitle="Praktični vodič: Azure Active Directory integraciju sa servisom Facebook na poslu | Microsoft Azure"
    description="Saznajte kako konfigurirati jedinstvenu prijavu između Azure Active Directory i Facebook na poslu."
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
    ms.date="04/26/2016"
    ms.author="asmalser"/>


# <a name="tutorial-azure-active-directory-integration-with-facebook-at-work"></a>Praktični vodič: Azure Active Directory integraciju sa servisom Facebook u akciji

Cilj ovog praktičnog vodiča je da bi se prikazala integraciji servisa Facebook na poslu s Azure Active Directory (Azure AD).

Integriranje Facebook na poslu s Azure AD pruža sljedeće prednosti: 

- Možete kontrolirati u Azure AD tko ima pristup Facebook u akciji 
- Možete automatski Dodjela računa za korisnike koji imaju pristup sa servisom Facebook u akciji
- Možete omogućiti svojim korisnicima da automatski se prijavili u sa servisom Facebook na poslu (jedinstvenu prijavu) s računa za Azure AD
- Možete upravljati svoje račune na jednom središnjem mjestu 

Ako želite saznati više pojedinosti o SaaS aplikacija Integracija s Azure AD potražite u članku [što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory](active-directory-appssoaccess-whatis.md).


## <a name="prerequisites"></a>Preduvjeti 

Da biste konfigurirali Azure AD Integracija CS zvjezdica, potrebne su vam sljedeće stavke:

- Pretplatu na Azure AD
- Omogućeno Facebook na poslu s jedine prijave

Da biste testirali korake ovog praktičnog vodiča, slijedite ove preporuke:

- Nemojte koristiti okruženja radnog osim ako je to potrebno.
- Ako nemate okruženju za Azure AD za probno razdoblje, možete preuzeti na jedan mjesec probne [ovdje](https://azure.microsoft.com/pricing/free-trial/). 


## <a name="adding-facebook-at-work-from-the-gallery"></a>Dodavanje Facebook na poslu iz galerije
Da biste konfigurirali integracije s Facebooka na poslu u Azure AD, morate dodati Facebook na poslu iz galerije popis upravljani SaaS aplikacija.

**Da biste dodali Facebook na poslu iz galerije, poduzmite sljedeće korake:**

1. **Azure klasični portal**u lijevom navigacijskom oknu kliknite **Servisa Active Directory**. 

    ![Active Directory][1]

2. Popis **direktorija** odaberite direktorija za koji želite da biste omogućili integraciju direktorija.

3. Da biste otvorili prikaz aplikacija u prikazu direktorija, na gornjoj izborniku kliknite **aplikacije** .

    ![Aplikacija][2]

4. Kliknite **Dodaj** pri dnu stranice.
    
    ![Aplikacija][3]

5. U dijaloškom okviru **što želite učiniti** kliknite **Dodaj aplikaciju iz galerije**.

    ![Aplikacija][4]

6. U okvir za pretraživanje upišite **Facebook na poslu**.

7. U oknu s rezultatima odaberite **Facebook na poslu**, a zatim **Dovršeno** da biste dodali aplikaciju.


### <a name="configuring-azure-ad-single-sign-on"></a>Konfiguriranje Azure AD jedinstvene prijave

U ovom se odjeljku opisuje kako omogućiti korisnicima za provjeru sa servisom Facebook na poslu s računa u Azure Active Directory, pomoću vanjskog pristupa na temelju SAML protokol.

**Da biste konfigurirali Azure AD jedinstvenu prijavu sa servisom Facebook na poslu, poduzmite sljedeće korake:**

1.  Nakon dodavanja Facebook na poslu na portalu za Azure klasični kliknite **Konfiguriraj jedinstvenu prijavu**.

2.  Na zaslonu za **Konfiguriranje aplikacije URL** unesite URL gdje korisnici će se prijavite na Facebook u aplikaciji. Ovo je na Facebook na URL klijenta za rad (primjer: https://example.facebook.com/). Kada završite, kliknite **Dalje**.

3.  U prozoru preglednika drugoj web, prijavite se u vašem Facebook na web-mjesto tvrtke rad kao administrator.

4. Slijedite upute na sljedeći URL za konfiguriranje Facebook na poslu da biste upotrijebili Azure AD davateljem identiteta: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)

5.  Kada je dovršena, vratite prozore preglednika koji se prikazuje portala za Azure klasični, a zatim kliknite potvrdni okvir da biste potvrdili da ste dovršili postupak, a zatim kliknite **Dalje** i **Dovršeno**.


## <a name="automatically-provisioning-users-to-facebook-at-work"></a>Automatski dodjelu resursa korisnicima Facebook u akciji

Azure AD podržava mogućnost automatski synchonize pojedinosti o računu dodijeljena korisnika na Facebooku na poslu. Ovaj automatsko sinhronizaciju sa programom omogućuje Facebook na poslu Dohvati podatke potrebne da biste autorizirali korisnici za pristup, sve ih pokušaja prijave prvi put. Ga i njezini dodjeljuje korisnika s Facebooka na poslu kada access je povučen u Azure AD.

Automatsko dodjeljivanje mogu se postaviti tako da kliknete **Dodjeljivanje za konfiguriranje računa** u prozoru portala za klasični Azure poruka.

Dodatne informacije o konfiguriranju automatskog dodjele resursa potražite u članku [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)


## <a name="assigning-users-to-facebook-at-work"></a>Dodjela korisnika sa servisom Facebook u akciji

Dodjela resursa AAD korisnici mogu vidjeti Facebook na poslu njihove teksta, oni mora biti dodijeljena pristup unutar Azure klasični portal.

**Da biste korisnicima dodijelili Facebook u akciji:**

1.  Na početnoj stranici za Facebook na poslu na portalu za Azure klasični, kliknite **Dodjela računa**.

2.  Na izborniku **Prikaz** odaberite želite li Dodjela korisnika ili grupu na Facebooku na poslu, a zatim kliknite gumb s kvačicom.

3.  Na popisu rezultata odaberite korisnike ili grupe kojima želite dodijeliti Facebook na poslu.

4.  U podnožje stranice, kliknite gumb **dodijeliti** .


## <a name="additional-resources"></a>Dodatni resursi

* [Popis vodiče za integrirane aplikacije SaaS Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Što je aplikacija programa access i jedinstvenu prijavu pomoću servisa Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cs-stars-tutorial/tutorial_general_04.png




