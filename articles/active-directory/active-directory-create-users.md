<properties
    pageTitle="Dodavanje novih korisnika Azure Active Directory | Microsoft Azure"
    description="U članku se objašnjava kako dodati novi korisnici ili promijeniti korisničke informacije u Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/22/2016"
    ms.author="curtand"/>

# <a name="add-new-users--or-users-with-microsoft-accounts-to-azure-active-directory"></a>Dodavanje novih korisnika ili korisnicima s računima za Microsoft Azure Active Directory

Dodajte korisnike za popunjavanje direktorija. U ovom se članku objašnjava dodavanje novih korisnika u tvrtki ili ustanovi i dodali korisnike koji imaju Microsoftova računa. Dodatne informacije o dodavanju korisnika iz druge mape u servisu Azure Active Directory ili dodavanje korisnika u tvrtki partnera potražite u članku [Dodavanje korisnika iz drugih direktorija ili partner tvrtke servisa Azure Active Directory](active-directory-create-users-external.md). Dodane korisnike Nemate administratorske dozvole po zadanom, no možete dodijeliti uloge na njih u bilo kojem trenutku.

## <a name="add-a-user"></a>Dodavanje korisnika

1. Prijavite se u [Azure klasični portal](https://manage.windowsazure.com) pomoću računa koji je globalni administrator za direktorij.
2. Odaberite **Servisa Active Directory**, a zatim odaberite naziv imenika tvrtke ili ustanove.
3. Odaberite karticu **korisnika** , a zatim na naredbenoj traci odaberite **Dodaj korisnika**.
4. Na stranici **Recite nam o korisniku** , u odjeljku **Vrsta korisnika**, odaberite nešto od sljedećeg:

    - **Novi korisnik u tvrtki ili ustanovi** – dodaje novi račun u direktoriju.
    - **Korisnik s postojećeg računa za Microsoft** – dodaje postojećeg računa za Microsoft potrošača direktorija (na primjer, račun programa Outlook)

5. Ovisno o **vrsti korisnika**, unesite korisničko ime (za novi korisnik) ili adresu e-pošte (za korisnika pomoću Microsoftova računa).
6. Na stranici **profila** korisnika Navedite naziv imena i prezimena, naziv jednostavnih i korisnička uloga na popisu **uloge** . Dodatne informacije o ulogama korisnika i administratora potražite u članku [Dodjela administratorskih uloga u Azure AD](active-directory-assign-admin-roles.md). Odredite želite li **Omogućiti višestruke provjere autentičnosti** korisnika.
7. Na stranici **Početak privremenu lozinku** , odaberite **Stvori**.

> [AZURE.IMPORTANT] Ako vaša tvrtka ili ustanova koristi više domena, informacije o sljedeći problemi prilikom dodavanja korisnički račun:
>
> - Korisnički računi s na istom korisnikovo Glavno ime (UPN) svim domenama, **prvi** dodati, na primjer, geoffgrisso@contoso.onmicrosoft.com, **Slijedi** geoffgrisso@contoso.com.
> - **Nemoj** dodati geoffgrisso@contoso.com prije nego što dodate geoffgrisso@contoso.onmicrosoft.com. Ovim redoslijedom važno je, a može biti nezgodno da biste poništili.

## <a name="change-user-information"></a>Promjena korisničkih podataka

Možete promijeniti bilo koji korisnik atribut osim ID objekta.

1. Otvorite direktorija.
2. Odaberite karticu **korisnika** , a zatim odaberite zaslonsko ime korisnika kojeg želite promijeniti.
3. Unesite promjene, a zatim kliknite **Spremi**.

Ako korisnik koju mijenjate sinkronizira se s vašeg lokalnog servisa Active Directory, ne možete promijeniti korisničke informacije u ovim postupkom. Da biste promijenili korisnika, pomoću alata za lokalni Active Directory upravljanje.

## <a name="guest-user-management-and-limitations"></a>Upravljanje korisnicima za goste i ograničenja

Korisnici s drugim direktorija koji ste pozvani da direktorija za pristup dokumente sustava SharePoint, aplikacije i ostale resurse za Azure su račune za goste. Račun za gosta u direktoriju ima njegov podlozi atribut UserType postavljeno na "Gosta." Obični korisnici (Konkretno, članovi direktorija) imaju atribut UserType "Člana."

Gosti imati ograničenim pravima u direktoriju. Ta prava ograničiti mogućnost za goste s ciljem otkrivanja informacije o ostalim korisnicima u direktoriju. Međutim, Gosti i dalje možete raditi korisnici i grupe koji su povezani s resursima rade. Korisnicima gostima učiniti sljedeće:

- U odjeljku ostali korisnici i grupe pridruženim pretplati na Azure u koju se dodijeli
- U odjeljku članovi grupa koje pripadaju
- Traženje drugih korisnika u imeniku, ako znaju na punu adresu e-pošte korisnika
- Vidjeti samo ograničeni skup atributa korisnici mogu potražiti – ograničeni zaslonsko ime, adresu e-pošte, korisnikovo Glavno ime (UPN) i minijature fotografije
- Dobit ćete popis provjerene domene u imeniku
- Pristanak aplikacijama, dodjelu isti članovi imaju u imeniku programa access

## <a name="set-guest-user-access-policies"></a>Postavljanje pravila za pristup gosta korisnika

**Konfiguriranje** karticu imenik sadrži mogućnosti za kontrolu pristupa za korisnike goste. Ove mogućnosti se mogu mijenjati samo u Azure klasični portal direktorija globalni administrator. Trenutno ne postoji PowerShell ili API način.

Da biste otvorili karticu **Konfiguracija** na portalu za Azure klasični, odaberite **Servisa Active Directory**, a zatim naziv direktorija.

![Konfiguriranje kartica u Azure Active Directory][1]

Zatim možete urediti mogućnosti za kontrolu pristupa za korisnike goste.

![mogućnosti kontrole pristupa za korisnike goste][2]


## <a name="whats-next"></a>Daljnji koraci

- [Dodavanje korisnika iz druge mape ili partner tvrtke servisa Azure Active Directory](active-directory-create-users-external.md)
- [Administriranje Azure AD](active-directory-administer.md)
- [Upravljanje lozinkama u Azure AD](active-directory-manage-passwords.md)
- [Upravljanje grupama u Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png
