<properties
   pageTitle="Azure Active Directory B2B suradnje | Microsoft Azure"
   description="Azure Active Directory B2B suradnje omogućuje poslovni partneri za pristup tvrtke aplikacija, sa svim svojim korisnicima predstavlja jednu Azure AD računa"
   services="active-directory"
   documentationCenter=""
   authors="curtand"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="08/23/2016"
   ms.author="curtand"/>

# <a name="azure-active-directory-b2b-collaboration"></a>Azure Active Directory B2B suradnje

Azure Active Directory (Azure AD) B2B suradnje možete omogućiti pristup svojim aplikacijama tvrtke od partnera upravlja identiteta. Možete stvarati odnose unakrsno tvrtke pozivanje i dopuštanja korisnika iz tvrtke partnera za pristup resurse. Složenosti smanjena je jer svaki tvrtke jednom federates s Azure Active Directory i svaki korisnik predstavlja jednu Azure AD računa. Sigurnost povećava se ako poslovni partneri upravljati njihove račune za Azure AD jer access je povučen prilikom partnera korisnici su prekinuti iz svoje tvrtke ili ustanove, a zatim je spriječio neželjeni pristup putem članstvom u interni direktorija. Za poslovne partnere koji je već nemate Azure AD B2B suradnje sadrži pojednostavnjeno sučelje za prijavu radi olakšavanja Azure AD računi poslovni partneri.

-   Poslovni partneri pomoću vlastitih prijavu vjerodajnica, što omogućuje vam iz upravljanje u imeniku vanjskih partnera i drugih potrebe za uklanjanje pristupa kada korisnici ostavite partner tvrtke ili ustanove.

-   Upravljanje pristupom biste aplikacijama neovisno o životnom ciklusu računa poslovnog partnera. To znači da, na primjer, opozvati pristup bez potrebe za zatražite od IT odjela tvrtke partnera ništa.

## <a name="capabilities"></a>Mogućnosti

B2B suradnje pojednostavnjuje upravljanje i poboljšava sigurnost pristup partnera web-mjesto tvrtke resurse uključujući SaaS aplikacija, kao što su Office 365, Salesforce, servisa Azure i svaki mobile u oblaku i u okvir za lokalni zahtjevima umu aplikacije. B2B Suradnja omogućuje partnerima upravljanje svoje račune i velike tvrtke možete primijeniti sigurnosna pravila za pristup partnera.

Azure Active Directory B2B suradnju je lako da biste konfigurirali pojednostavnjeni registracije za partnere svih veličina čak i ako nemaju vlastite Azure Active Directory putem proces provjeriti za e-pošte. I jednostavno je da bi se zadržao s nema vanjskih imenika ili po konfiguracije vanjski pristup partnera.

## <a name="b2b-collaboration-process"></a>Postupak B2B suradnje

1. Azure AD B2B suradnje omogućuje administrator tvrtke da biste pozvali i autorizirali skup vanjskim korisnicima tako da prijenosa datoteke vrijednosti odvojenih zarezom (CSV) više od 2000 redaka B2B portal za suradnju.

  ![Dijaloški okvir prijenos za CSV datoteke](./media/active-directory-b2b-collaboration-overview/upload-csv.png)

2. Na portalu će poslati e-pošte pozivnice za vanjske korisnike.

3. Pozvani korisnik će prijavite se na postojeći račun za rad s programom Microsoft (upravlja se iz Azure AD) ili se na novi račun za rad u Azure AD.

4. Kada se prijavite, korisnik će biti preusmjereni na aplikaciju koju je omogućeno zajedničko korištenje s njima.

Pozivnice za korisničke adrese e-pošte (na primjer, Gmail ili [*comcast.net*](http://comcast.net/)) trenutno nisu podržani.

Dodatne informacije o funkcioniranje B2B suradnje, pogledajte [Ovaj videozapis](http://aka.ms/aadshowb2b).

## <a name="next-steps"></a>Daljnji koraci
Pronađite naše ostale članke Azure AD B2B suradnje.

- [Što je Azure AD B2B suradnje?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kako funkcionira](active-directory-b2b-how-it-works.md)
- [Detaljni vodič](active-directory-b2b-detailed-walkthrough.md)
- [Referenca za oblikovanje CSV datoteke](active-directory-b2b-references-csv-file-format.md)
- [Tokena oblik vanjskog korisnika](active-directory-b2b-references-external-user-token-format.md)
- [Promjene atributa objekt vanjskog korisnika](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Trenutni pretpregled ograničenja](active-directory-b2b-current-preview-limitations.md)
- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
