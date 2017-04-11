<properties
    pageTitle="Azure Active Directory B2C: Samostalno ponovno postavljanje lozinke | Microsoft Azure"
    description="Kako postaviti samostalno ponovno postavljanje lozinke za vaše koje korisnici u Azure Active Directory B2C takvi teme"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure Active Directory B2C: Postavljanje samostalno ponovno postavljanje lozinke za vaše koje korisnici

Pomoću značajke samostalno ponovno postavljanje vašeg koje korisnici (koji se registrirali za lokalni računi) možete ponovno postaviti lozinke na vlastitu. To znatno smanjuje teret na osoblju za podršku, osobito ako aplikacija ima milijune koje korisnici upotrebljavaju redovito. Trenutno ne možemo podržavaju samo pomoću adrese e-pošte potvrđeni kao način za oporavak. Dodat ćemo metode dodatne oporavka (provjereno telefonski broj, sigurnosna pitanja, itd.) u budućnosti.

> [AZURE.NOTE]
Ovaj se članak odnosi na samostalno ponovno postavljanje koristi u kontekstu pravila za prijavu. Ako je potrebno u potpunosti prilagoditi za ponovno postavljanje lozinke pravila pozvanih iz aplikacije, potražite u [ovom članku](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy).

Prema zadanim postavkama direktorija neće imati samostalno ponovno postavljanje uključena. Da biste ga uključili, poduzmite sljedeće korake:

1. Prijavite se na [portal za Azure klasični](https://manage.windowsazure.com/) kao Administrator pretplate. Ovo je iste tvrtke ili obrazovne ustanove ili isti Microsoftov račun koji ste koristili za stvaranje direktorija.
2. Dođite do proširenje servisa Active Directory na navigacijskoj traci na lijevoj strani.
3. Pronađite direktorija na kartici **direktorija** i kliknite ga.
4. Kliknite karticu **Konfiguracija** .
5. Pomaknite se prema dolje do odjeljka **pravila za ponovno postavljanje lozinke korisničkog** pa isključite mogućnost **korisnici kojima je omogućeno za lozinke ponovno postavite** na **da**. Obratite pozornost na to da je potvrđen okvir mogućnost **Zamjensku adresu e-pošte** ; Nemojte mijenjati.

    ![Samostalno ponovno postavljanje lozinke](./media/active-directory-b2c-reference-sspr/sspr.png)

6. Kliknite **Spremi** pri dnu stranice. Gotovi ste!

Da biste testirali, pomoću značajke "Izvedi sada" na sve prijavu pravila koja ima lokalni računi kao davateljem identiteta. Prilikom prijave za lokalni račun stranici (koje unesete adresu e-pošte i lozinku, ili korisničko ime i lozinku), kliknite **ne možete pristupiti računu?** da biste provjerili korisnička sučelja.

> [AZURE.NOTE]
Samostalno ponovno postavljanje stranice moguće je prilagoditi pomoću [brendiranje značajka tvrtke](../active-directory/active-directory-add-company-branding.md).
