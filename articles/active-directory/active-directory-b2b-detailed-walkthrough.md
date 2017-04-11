<properties
   pageTitle="Detaljno objašnjenje te pomoću pretpregleda suradnju Azure Active Directory B2B | Microsoft Azure"
   description="Azure Active Directory B2B suradnje podržava više tvrtke odnosa omogućivanjem poslovni partneri za selektivno pristup tvrtke aplikacija"
   services="active-directory"
   documentationCenter=""
   authors="viv-liu"
   manager="cliffdi"
   editor=""
   tags=""/>

<tags
   ms.service="active-directory"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="identity"
   ms.date="05/09/2016"
   ms.author="viviali"/>

# <a name="azure-ad-b2b-collaboration-preview-detailed-walkthrough"></a>Azure AD B2B suradnju pretpregled: detaljni vodič

Ovaj vodič navode upute za korištenje Azure AD B2B suradnje. Kao IT administrator tvrtke Contoso želimo zajedničko korištenje aplikacije s zaposlenici tri partnera tvrtki. Nijedan od partnera tvrtke moraju imati Azure AD.

- Alice iz jednostavne Partner tvrtke ili ustanove
- Teo od srednje velike tvrtke ili ustanove partnera, treba pristup skup aplikacije
- Katarina iz složene Partner tvrtke ili ustanove, treba pristup skup aplikacije i članstvo u grupama pri Contoso

Nakon pozivnice pošalju korisnicima partnera, ćemo ih konfigurirati u Azure AD da biste dodijelili pristup aplikacijama sustava i članstvo u grupama putem portala za Azure. Započnimo dodavanjem Alice.

## <a name="adding-alice-to-the-contoso-directory"></a>Dodavanje Alice u imeniku tvrtke Contoso
1. Stvaranje .csv datoteke pomoću zaglavlja kao što je prikazano puniti samo Alice na **e-pošte**, **riješiti problem**i **InviteContactUsUrl**. **Riješiti problem** je naziv koji se pojavljuje u pozivnici i naziv koji će se pojaviti u imeniku tvrtke Contoso Azure AD. **InviteContactUsUrl** je način za Alice da se obratite Contoso. U sljedećem primjeru InviteContactUsUrl određuje LinkedIn profila tvrtke Contoso. Važno je da pravopisa natpise u prvi redak .csv datoteke točno onako kao što je navedeno u [Referenca za oblikovanje CSV datoteke](active-directory-b2b-references-csv-file-format.md).  
![Ogledna CSV datoteka za Alice](./media/active-directory-b2b-detailed-walkthrough/AliceCSV.png)

2. Na portalu Azure Dodavanje korisnika u imeniku tvrtke Contoso (Active Directory > Contoso > korisnici > Dodavanje korisnika). "Upišite korisnik" padajući izbornik, odaberite "Korisnici u tvrtkama partnera". Prenesite .csv datoteke. Provjerite jesu li .csv datoteke zatvorena prije prijenosa.  
![Prijenos datoteke CSV za Alice](./media/active-directory-b2b-detailed-walkthrough/AliceUpload.png)

3. Alice sada predstavljen kao vanjskog korisnika u imeniku tvrtke Contoso Azure AD.  
![Alice nalazi se u Azure AD](./media/active-directory-b2b-detailed-walkthrough/AliceInAD.png)

4. Alice dobiva sljedeće e-pošte.  
![Pozivnicu e-pošte za Alice](./media/active-directory-b2b-detailed-walkthrough/AliceEmail.png)

5. Alice klikne vezu, a ona se to od vas zatraži da biste prihvatili pozivnicu i prijavite se pomoću vjerodajnica svoj rad. Ako Alice ne nalazi u direktoriju Azure AD, Alice se to od vas zatraži da biste se registrirali.  
![Prijavite se nakon pozivnicu za Alice](./media/active-directory-b2b-detailed-walkthrough/AliceSignUp.png)

6. Alice se preusmjerava na aplikaciju programa Access ploču, prazan dok se ona je omogućen pristup aplikacijama.  
![Ploča za pristup za Alice](./media/active-directory-b2b-detailed-walkthrough/AliceAccessPanel.png)

Ovaj postupak omogućuje najjednostavniji obliku B2B suradnje. Kao korisnika u imeniku tvrtke Contoso Azure AD Alice mogu dodijeliti pristup aplikacijama i grupe putem portala za Azure. Sada dodajte Teo koji treba pristup aplikacijama Moodle i Salesforce.

## <a name="adding-bob-to-the-contoso-directory-and-granting-access-to-apps"></a>Dodavanje Teo u imeniku tvrtke Contoso i dopuštanje pristupa web-aplikacije
1. Korištenje komponente Windows PowerShell s modul Azure AD instaliran da biste pronašli aplikacije ID-a Moodle i Salesforce. ID-ove mogu biti dohvaćeni pomoću cmdlet: `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId` tako ćete prikazati popis svih dostupnih aplikacija u Contoso i njihovih AppPrincialIds.  
![Dohvaćanje ID-a za Teo](./media/active-directory-b2b-detailed-walkthrough/BobPowerShell.png)

2. Stvaranje .csv datoteku koja sadrži Teo na e-pošte i riješiti problem, **InviteAppID**, **InviteAppResources**i InviteContactUsUrl. Popunjavanje **InviteAppResources** s AppPrincipalIds Moodle i Salesforce pronaći iz komponente PowerShell razdvojena razmakom. Popunjavanje **InviteAppId** s na istom AppPrincipalId od Moodle brandiranje e-pošte i prijavite se na stranicama s Moodle logotip.  
![Ogledna CSV datoteka za Teo](./media/active-directory-b2b-detailed-walkthrough/BobCSV.png)

3. Prenesite .csv datoteke putem portala za Azure baš kao i to je učinjeno za Alice. Teo sada je vanjskog korisnika u imeniku tvrtke Contoso Azure AD.

4. Teo dobiva sljedeće e-pošte.  
![Pozivnicu e-pošte za Teo](./media/active-directory-b2b-detailed-walkthrough/BobEmail.png)

5. Teo klikne vezu, a je zatražiti da biste prihvatili pozivnicu. Kada on se prijavite, on je usmjerena na ploči programa Access i već pomoću Moodle i Salesforce.  
![Ploča za pristup za Teo](./media/active-directory-b2b-detailed-walkthrough/BobAccessPanel.png)

Dodat ćemo Katarina nakon toga koji treba pristup aplikacijama, kao i članstvo u grupama u imeniku tvrtke Contoso.

## <a name="adding-carol-to-the-contoso-directory-granting-access-to-apps-and-giving-group-membership"></a>Dodavanje Katarina u imeniku tvrtke Contoso, dopuštanje pristupa web-aplikacije i održavanje članstvo u grupi

1. Korištenje komponente Windows PowerShell s modul Azure AD instaliran za traženje ID-ove i ID oznake grupe unutar tvrtke Contoso.
 - Dohvaćanje AppPrincipalId pomoću cmdleta `Get-MsolServicePrincipal | fl DisplayName, AppPrincipalId`, ista kao Teo
 - Dohvaćanje ID objekta za grupe pomoću cmdleta `Get-MsolGroup | fl DisplayName, ObjectId`. Tako ćete prikazati popis svih grupa u Contoso i njihovih ObjectIds. ID-ove grupe mogu biti dohvaćeni kao Identifikator objekta na kartici svojstva grupe na portalu za Azure.  
![Dohvaćanje ID-ova i grupe za Katarina](./media/active-directory-b2b-detailed-walkthrough/CarolPowerShell.png)

2. Stvaranje .csv datoteke, popunjavanje Katarina na e-pošte, riješiti problem, InviteAppID, InviteAppResources, **InviteGroupResources**i InviteContactUsUrl. **InviteGroupResources** je popunjena ObjectIds grupe MyGroup1 i Externals, odvojenih zarezom.  
![Ogledna CSV datoteka za Katarina](./media/active-directory-b2b-detailed-walkthrough/CarolCSV.png)

3. Prijenos datoteke .csv putem portala za Azure.

4. Katarina korisnika u imeniku tvrtke Contoso, a ujedno član grupe MyGroup1 i Externals, kao što se vidi na portalu za Azure.  
![Katarina nalazi se u grupi u Azure AD](./media/active-directory-b2b-detailed-walkthrough/CarolGroup.png)

5. Katarina Prima poruke e-pošte koja sadrži vezu da biste prihvatili pozivnicu. Nakon što se ona se prijavi, ona se preusmjerava na ploču aplikacije programa Access da biste imali pristup Moodle i Salesforce.  

To je sve postoji Dodavanje korisnika iz tvrtke partnera Azure AD B2B suradnje. Ovaj vodič prikazivao kako dodati korisnike Alice, Teo i Katarina imenik tvrtke Contoso pomoću tri zasebna .csv datoteke. Ovaj postupak možete izvršiti bilo lakše na condensing zasebnom .csv datoteke u jednu datoteku.  
![Ogledna CSV datoteka za Alice "," Teo "i" Katarina](./media/active-directory-b2b-detailed-walkthrough/CombinedCSV.png)

## <a name="related-articles"></a>Povezani članci
Pregled naše ostale članaka na Azure AD B2B suradnje:

- [Što je Azure AD B2B suradnje?](active-directory-b2b-what-is-azure-ad-b2b.md)
- [Kako funkcionira](active-directory-b2b-how-it-works.md)
- [Referenca za oblikovanje CSV datoteke](active-directory-b2b-references-csv-file-format.md)
- [Tokena oblik vanjskog korisnika](active-directory-b2b-references-external-user-token-format.md)
- [Promjene atributa objekt vanjskog korisnika](active-directory-b2b-references-external-user-object-attribute-changes.md)
- [Trenutni pretpregled ograničenja](active-directory-b2b-current-preview-limitations.md)
- [Članak indeks za upravljanje aplikacijama servisa Azure Active Directory](active-directory-apps-index.md)
