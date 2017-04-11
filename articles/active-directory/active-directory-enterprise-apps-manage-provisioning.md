<properties
    pageTitle="Korisnik upravljanje za enterprise aplikacije u pretpregledu Azure Active Directory za dodjelu resursa | Microsoft Azure"
    description="Informirajte se o upravljanju dodjele resursa korisnički račun za enterprise aplikacije pomoću pretpregleda Azure Active Directory"
    services="active-directory"
    documentationCenter=""
    authors="asmalser"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="09/12/2016"
    ms.author="asmalser"/>

#<a name="preview-managing-user-account-provisioning-for-enterprise-apps-in-the-new-azure-portal"></a>Pregled: Upravljanje korisničkim računom dodjele resursa za enterprise aplikacije na novom portalu Azure

U ovom se članku opisuje kako koristiti [Azure portal](https://portal.azure.com) za upravljanje automatsko korisnički račun dodjele resursa i Poništi dodjelu resursa za aplikacije koje podržava, osobito one koji su dodani u kategoriji "istaknute" u [galeriju aplikacija Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery). U ovom sučelje za upravljanje na novom portalu Azure trenutno javno pretpregled, a u ovom se članku opisuju nove značajke, kao i nekoliko privremene ograničenja koja će se prikazati tijekom razdoblja za pretpregled. [Novosti u pretpregledu?](active-directory-preview-explainer.md)

Da biste saznali više o dodjele resursa automatskog korisnički račun i kako funkcionira, potražite u članku [automatizirati dodjele resursa korisnika i Deprovisioning SaaS aplikacije pomoću servisa Azure Active Directory](active-directory-saas-app-provisioning.md).

##<a name="finding-your-apps-in-the-new-portal"></a>Pronalaženje aplikacija na novom portalu

Od 2016 rujan sve aplikacije podešena za jednu prijave u imeniku administrator directory pomoću [galerije aplikacije Azure Active Directory](active-directory-appssoaccess-whatis.md#get-started-with-the-azure-ad-application-gallery) unutar [Azure klasični portal](https://manage.windowsazure.com), sada može pregledavati i upravlja na novom portalu Azure.

Te aplikacije pronaći ćete u odjeljku **Aplikacijama** novi Azure portalu kojem se može pristupiti putem izbornika **Više servisa** u području za navigaciju. Enterprise aplikacije sadrži aplikacije koje su u uveden i koriste korisnicima unutar tvrtke ili ustanove.

![Plohu aplikacijama][0]

Odabirom veze za **sve programe** s lijeve strane i prikazat će se popis svih aplikacija koje su konfigurirana, uključujući aplikacije koje su dodane iz galerije. Odabir aplikacije učitava plohu resursa za tu aplikaciju, gdje izvješća moguće je prikazati za tu aplikaciju i upravljati mogu različite postavke.

Korisnički račun dodjeljivanje postavke upravlja se tako da odaberete **Provisioning** na lijevoj strani.

![Plohu resursa aplikacije][1]


##<a name="provisioning-modes"></a>Načini rada za dodjelu resursa

Plohu **Provisioning** započinje izbornički **način** koji pokazuje koje načini rada za dodjelu resursa podržani za enterprise aplikaciju, a im omogućuje da je konfigurirati. Dostupne mogućnosti obuhvaćaju sljedeće:

* **Automatsko** - Ova mogućnost pojavljuje se ako Azure AD podržava automatsko koji se temelji na API dodjele resursa i/ili deaktivirali dodjeljivanje korisničkih računa za ovu aplikaciju. Odabir tom načinu prikazuje sučelja koji vodi administratori kroz konfiguriranje Azure AD za povezivanje za upravljanje korisnicima u aplikaciji API-JA, stvaranje mapiranja računa i tijekove rada koje definiraju kako treba tijeka korisničkih računa podataka između Azure AD i aplikaciju te upravljanje Azure AD dodjeljivanje servisa.

* Prikazan je **ručno** - tu mogućnost ako Azure AD ne podržava automatsko dodjeljivanje korisničkih računa za ovu aplikaciju. Tu mogućnost, znači da korisnik zapise o poslovnim subjektima pohranjene u aplikaciji njome se upravlja pomoću vanjski postupak, na temelju korisnik upravljanje i dodjeljivanje mogućnosti nudi tu aplikaciju (što može obuhvaćati dodjele resursa ispravljanje SAML).


##<a name="configuring-automatic-user-account-provisioning"></a>Konfiguriranje dodjele resursa automatsko korisničkog računa

Odaberite mogućnost **automatski** prikazuju se zaslon na kojem je podijeljen u četiri sekcije:

###<a name="admin-credentials"></a>Administratorske vjerodajnice
To je mjesto potrebne vjerodajnice za Azure AD za povezivanje za upravljanje korisnicima aplikacije koje se unose API-JA. Unos potrebna ovisi o aplikaciji. Da biste saznali više o vjerodajnica i preduvjeti za određene aplikacije, potražite u članku [Konfiguracija vodič za tu određenu aplikaciju](active-directory-saas-app-provisioning.md#list-of-apps-that-support-automated-user-provisioning).

Odabir gumb **Testiraj vezu** omogućuje da biste testirali vjerodajnica tako da vas Azure AD pokušaj povezivanja s aplikaciju je dodjeljivanje aplikaciju putem unesene vjerodajnice.

###<a name="mappings"></a>Mapiranja
Ovo je koje administratori smije prikazivati i uređivati koje korisnik atribute tijeka između Azure AD i ciljnu aplikaciju, kada se korisničke račune dodjeli ili ažurirati.

Postoji unaprijed konfiguriranom skup mapiranja između Azure AD korisnika i svaku aplikaciju SaaS korisnik objekata. Neke se aplikacije upravljanje druge vrste objekata, kao što su grupe ili kontakata. Odabirom jedne od ovih mapiranja u tablici su navedeni uređivaču mapiranje udesno, gdje se može pregledavati i prilagoditi.

![Plohu resursa aplikacije][2]

Podržani prilagodbe tijekom prvog pretpregleda obuhvaćaju sljedeće:

* Omogućivanje i onemogućivanje mapiranja za određene objekte, kao što su korisnički objekt Azure AD za aplikaciju SaaS korisničkom objektu.

* Uređivanje atribute tijeka iz korisničkom objektu Azure AD aplikacije korisničkom objektu. Dodatne informacije o mapiranje atributa potražite u članku [objašnjenje atribut mapiranje vrste](active-directory-saas-customizing-attribute-mappings.md#understanding-attribute-mapping-types).

* Filtriranje koje akcije za dodjelu resursa Azure AD provoditi na ciljnu aplikaciju, što je nova značajka na portalu za Azure. Umjesto da Azure AD u potpunosti-sinkronizirati objekata, možete ograničiti akcije izvršiti. Na primjer, tako da samo odabirom **Ažuriranje**Azure AD samo ažuriranja postojećeg korisnika računa u aplikaciji i stvarati nove. Odabirom samo **Stvori**, Azure samo stvara novih korisničkih računa, ali neće ažurirati postojeće. Ova značajka omogućuje administratori za stvaranje različitih mapiranja za stvaranje računa i ažuriranje tijekova rada. Puni mogućnost da biste stvorili više mapiranja po aplikacija se planira za kasnije u razdoblju pretpregled.

###<a name="settings"></a>Postavke
U ovom se odjeljku omogućuje administratori za pokretanje i zaustavljanje Azure AD dodjeljivanje servis za odabranu aplikaciju, kao i po želji Očisti predmemoriju za dodjelu resursa i ponovno pokrenite servis.

Ako dodjele resursa je omogućeni prvi put za aplikaciju, uključite servis tako da promijenite **Stanje dodjele resursa** **na**. To uzrokuje Azure AD servis za dodjelu resursa za izvođenje početne sinkronizacije, u kojem piše korisnici dodijeljeni u odjeljku **korisnici i grupe** , upiti za ciljnu aplikaciju za njih te izvodi definiranih u odjeljku Azure AD **mapiranja** akcija za dodjelu resursa. Tijekom tog postupka servis za dodjelu resursa pohranjuje predmemoriranim podacima o koje korisničkim računima upravljanje, tako da ne upravlja računima unutar ciljnim aplikacijama koje su se nikad ne u opseg za dodjelu ne utječu Poništi dodjelu resursa operacije. Nakon početnog sinkronizacije, servis za dodjelu resursa automatski sinkronizira korisnika i grupa objekata na interval deset minuta.

**Dodjeljivanje Status** mijenja u **izvan** samo zaustaviti servis za dodjelu resursa. U ovom stanju Azure ne stvaranje, ažuriranje ili uklanjanje svaki korisnik ili grupa objekata u aplikaciji. Promjena stanja natrag na uzrokuje servis mogao preuzeti gdje ga stali.

Odabir potvrdni okvir **poništite trenutno stanje i ponovno pokrenuti sinkronizaciju** i spremanje zaustavlja servis za dodjelu resursa ispisi predmemoriranim podacima o koje računima Azure AD upravlja, ponovnog pokretanja servisa te izvodi ponovno početne sinkronizacije. Ova mogućnost dopušta administratorima da biste započeli postupak implementacije dodjele resursa ponovno.

###<a name="synchronization-details"></a>Detalji o sinkronizaciji
Ovo poglavlje sadrži Zbrajanje detalje o postupka dodjele resursa servisa, uključujući vrijeme imena i prezimena servis za dodjelu resursa pokrenuli izvoditi na aplikaciju i koliko korisnika i grupa objekata koji se upravlja.

Veze su navedene **Provisioning izvješće o aktivnosti**, koji pruža zapisnik svi korisnici i grupe stvorene, ažurirane i uklonjene između Azure AD i detaljan ciljne aplikacije, a **Provisioning izvješće o pogrešci** koja sadrži više poruke o pogreškama za korisnika i grupa objekata koje nije uspjelo čitati, stvorite, ažurirati ili ukloniti. 

[0]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-blade.PNG
[1]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning.PNG
[2]: ./media/active-directory-enterprise-apps-manage-provisioning/enterprise-apps-provisioning-mapping.PNG
