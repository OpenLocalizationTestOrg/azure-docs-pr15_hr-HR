<properties
   pageTitle="Stvaranje i registriranje računa programa publisher | Microsoft Azure"
   description="Upute za stvaranje računa za Microsoft Developer tako da na odobrenje, možete prodajete različite nude vrste trgovine Windows Azure."
   services="Azure Marketplace"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="hascipio"/>

# <a name="create-a-microsoft-developer-account"></a>Stvaranje računa za Microsoft Developer
U ovom se članku vodit će vas kroz stvaranje potrebne računa i postupak registracije postati odobrene Microsoft Developer za Azure Marketplace.

## <a name="1-create-a-microsoft-account"></a>1. stvaranje Microsoftova računa
Da biste započeli postupak objavljivanja, morat ćete stvoriti Microsoftova računa. Račun će se koristiti da biste registrirali **Microsoftova centra za razvojne inženjere** i **Azure Portal za objavljivanje**. Imat ćete samo jedan Microsoftov račun za vaše ponude servisa Azure Marketplace. Ne smije biti specifična za servise ili ponude.

Adresu koja je korisničko ime mora biti u svojoj domeni i upravlja IT timom. Sve aktivnosti objavljivanja povezane računajući putem ovog računa.

  >[AZURE.WARNING] Riječi kao što su **"Azure"** i **"Microsoft"** nisu podržani za registraciju za Microsoftov račun. Izbjegavanje korištenja ove riječi da biste dovršili postupak registracije i stvaranje računa.

### <a name="instructions"></a>Upute

1. Stvaranje popisa za raspodjelu (DL) ili sigurnosne grupe (SG) unutar domene vaše tvrtke. Na DL omogućuje većem broju osoba za primanje obavijesti e-pošte koje su važne za izvješćivanje o payout. Također provjerava vlasništvo nad Microsoftov račun može prenijeti, a ne uz jednu osobu.
Slijedite upute navedene u nastavku.

    1. Dodajte vaš tim za uhodavanje u DL.
    2. Provjerite je li DL/SG je adresa e-pošte aktivni i primati poruke e-pošte jer plaćanja, porez podataka i izvješćivanja će biti proslijeđene putem ovog računa.
    3. Preporučujemo korištenje nešto poput marketplace@partnercompany.com kao adresu e-pošte za DL/SG.

2. Otvorite novi Inkognito Chrome ili Internet Explorer InPrivate koji sesija pregledavanja da biste bili sigurni da niste prijavljeni postojeći račun.
3. Registrirajte se DL stvorena u koraku 1 kao Microsoftov račun pomoću veze [https://signup.live.com/signup.aspx](https://signup.live.com/signup.aspx). Slijedite upute u nastavku.

    1. Tijekom proces registriranja računa kao Microsoftov račun, morate navesti valjan telefonski broj za sustav da biste poslali koda za potvrdu računa kao tekstnu poruku ili poziv automatiziranog.
    2. Tijekom proces registriranja računa kao Microsoftov račun, morate omogućavaju primanja automatiziranog e-pošte za potvrdu račun valjana e-pošte ID-a.

4. Provjerite je li adresa e-pošte poslane na popis Raspodjele.
5. Sada ste spremni za korištenje novog Microsoftova računa u Microsoft Developer Center.

## <a name="2-create-your-microsoft-developer-center-account"></a>2. stvaranje računa za Microsoft Developer Center
Da biste registrirali podatke tvrtke jednom se koristi Microsoft Developer Center. Na registrant moraju biti valjane predstavnika tvrtke i navedite njihove osobne podatke kao način da biste provjerili njihov identitet. Osoba Registracija morate koristiti Microsoftov račun koji se zajednički koristi za tvrtke, **i isti račun mora se koristiti u Azure Portal za objavljivanje.** Provjerite da biste provjerili je li vaša tvrtka još nema račun sustava Microsoft Developer Center prije nego se pokušate da biste je stvorili. Tijekom postupka, ne možemo će prikupljanje informacija adresu tvrtke, informacije bankovnog računa i poreza informacije. Ovo su najčešće obtainable financija ili poslovnih kontakata.

> [AZURE.IMPORTANT] Da bi se proći kroz različite faze ponudu stvaranja i implementacija morate obaviti sljedeće komponente profila za razvojne inženjere.


| Profil za razvojne inženjere | Da biste pokrenuli skica | Pripremna | Objavljivanje besplatno i rješenje predloška | Objavljivanje komercijalne |
|----|----|----|----|----|
|Registracija za tvrtke | Morate imati | Morate imati | Morate imati | Morate imati |
|ID za PDV profila | Neobavezno | Neobavezno | Neobavezno | Morate imati |
|Bankovnih računa | Neobavezno | Neobavezno | Neobavezno | Morate imati |


> [AZURE.NOTE] Premjesti vaše vlastite licence (BYOL) je podržano samo za virtualnim strojevima i smatra **besplatnu** ponudu.


### <a name="register-your-company-account"></a>Registrirati račun za tvrtke
1. Otvorite novi Internet Explorer InPrivate ili Chrome Inkognito sesija pregledavanja da biste bili sigurni da niste prijavljeni osobni račun.

2. Idite na [http://dev.windows.com/registration?accountprogram=azure](http://dev.windows.com/registration?accountprogram=azure) da biste registrirali sami kao Prodavač u Razvojni centar. Pročitajte sljedeće važne napomene prije nastavka.

    >[AZURE.IMPORTANT] Provjerite je li u e-pošte ID-a ili popis za raspodjelu (popis za raspodjelu preporučuje se da biste uklonili ovisnost osoba) koje ćete koristiti za registriranje u Razvojni centar na prvo registrirana kao Microsoftov račun. Ako nije, zatim registrirajte pomoću ove [veze](https://signup.live.com/signup?uaid=e479342fe2824efeb0c3d92c8f961fd3&lic=1). Osim toga, **bilo e-pošte ID-a u odjeljku domene tvrtke Microsoft odnosno @microsoft.com nije moguće koristiti** za Razvojni centar za registraciju.

    ![Crtanje][img-signin]

3. Dovršite čarobnjak "Pridonesite zaštiti vašeg računa", koji će se provjeriti vaš identitet putem adrese telefonski broj ili e-pošte.

    ![Crtanje][img-verify]

4. U odjeljku "Informacije o registraciji računu" odaberite **račun država/regija** na padajućem izborniku.

    ![Crtanje](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_04.png)

    > [AZURE.WARNING] **"Pošiljke s" države:** Da biste prodaju servisa na trgovine Windows Azure, vaše registrirani entitet mora biti iz jednog od odobrene "pošiljke izvora" države iznad. To ograničenje je payout i taxation razloga. Ne možemo aktivno traženi da biste proširili popis države u skorijoj budućnosti, tako ostati definirati. Dodatne informacije potražite u članku [pravila sudjelovanje trgovine](http://go.microsoft.com/fwlink/?LinkID=526833).

5. Odaberite "Vrstu računa" kao **tvrtka** , a zatim kliknite gumb **Dalje** .

    > [AZURE.IMPORTANT] Da biste bolje razumjeli vrste računa, a koji je oblik najprikladniji za odabir, pogledajte stranicu [vrste računa, mjesta, a naknade](https://msdn.microsoft.com/library/windows/apps/jj863494.aspx)

    ![Crtanje](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_05.png)

6. Unesite u **Publisher zaslonsko ime**, obično naziv vaše tvrtke.

    > [AZURE.TIP] Publisher zaslonsko ime uneseno u Razvojni centar ne prikazuju se u trgovine Windows Azure kada dolazi navedenih tu ponudu. No to se mora ispuniti da biste dovršili postupak registracije.

7. Unesite **podatke za kontakt** radi provjere valjanosti računa.

    > [AZURE.IMPORTANT] Navedite točne podatke za kontakt jer će se koristiti u našem postupak provjere valjanosti za svoju tvrtku odobriti u centru za razvojne inženjere.

8. Unesite podatke za kontakt za **Odobravatelj tvrtke**. Tvrtka odobravatelj je osoba koja možete provjeriti koji imaju dozvolu za stvaranje računa u Razvojni centar ime tvrtke ili ustanove. Kliknite **Dalje** da biste premjestili na **"Plaćanja odjeljak"** kada završite.

    ![Crtanje](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_08.png)

9. Unesite svoje podatke za plaćanje platiti za vaš račun. Ako imate Promotivni kod koji prekriva trošak Registracija, unosite koji ovdje. U suprotnom, navedite svoje podatke o kreditnoj kartici (ili putem Paypala na podržani tržištima). Kada završite, kliknite **Dalje** da biste premjestili na **"Pregled zaslona"**.

    ![Crtanje](media/marketplace-publishing-accounts-creation-registration/imgRegisterCo_09.png)

10. Pregledajte podatke za račun i provjerili sve ispravno. Zatim, pročitajte i prihvatite uvjete i odredbe za [Microsoft Azure Marketplace Publisher ugovor](http://go.microsoft.com/fwlink/?LinkID=699560). Potvrdite okvir da biste označili ste čitati i prihvatiti ove termine.

11. Kliknite **Završi** da biste potvrdili svoju registraciju. Poruke o potvrdi ćemo poslati na adresu e-pošte.

12. Ako namjeravate objaviti samo besplatne ponuda, kliknite **Idi na Azure Marketplace Portal za objavljivanje** , a prijeđite na odjeljak 3 ovog dokumenta [registrirati svoj račun na portalu za objavljivanje](#3-register-your-account-in-the-publishing-portal).

Ako namjeravate objaviti komercijalne nudi (npr. virtualnog računala ponuda uz svaki sat naplate modela), kliknite **Ažuriranje podataka o računu** gdje morate ispuniti porez i bankovne podatke na vašem računu Razvojni centar za.

Ako želite ažurirati porez i banke podatke o kasnije, a zatim možete premjestiti na sljedeću sekciju odnosno sekciji 3 ovaj dokument [registrirati svoj račun na portalu za objavljivanje](#3-register-your-account-in-the-publishing-portal), i dolaze ponovno kasnije pomoću veze u Azure Portal za objavljivanje.

> [AZURE.IMPORTANT] U slučaju komercijalne ponuda, nećete moći automatske vaše ponude za proizvodnju bez dovršetka informacije poreza i bankovnog računa.

Ako biste radije da biste ažurirali porez informacija o i banke kasnije, možete i otvoriti odjeljak 3, [registrirati svoj račun na portalu za objavljivanje](#3-register-your-account-in-the-publishing-portal), i dolaze ponovno kasnije pomoću veze u Azure Portal za objavljivanje.

### <a name="add-tax-and-banking-information"></a>Dodavanje poreznog i bankovnih informacije
 Ako želite objaviti komercijalne ponuda za kupnju, morate dodati payout poreza informacije i slanje provjere valjanosti u centru za razvojne inženjere. Ako ćete objaviti samo besplatne ponuda (ili BYOL nudi), zatim nije potrebno da biste dodali taj podatak. Možete je dodati kasnije, ali potrebno neko vrijeme da biste provjerili valjanost podataka poreza. Ako znate da će ponuditi komercijalne ponuda za kupnju, preporučujemo da ga dodate čim.

**Bankovni podaci**

1. Prijavite se pomoću Microsoftova računa u [Microsoftova centra za razvojne inženjere](http://dev.windows.com/registration?accountprogram=azure) .

2. Kliknite **račun Payout** na lijevom izborniku pa u odjeljku **Odaberite način plaćanja** **bankovnih računa** ili **putem Paypala**.

    > [AZURE.IMPORTANT] Ako imate komercijalne ponuda za kupce kupiti na tržištu, to je račun gdje će primiti payout za te nabavu.

3. Unesite podatke za plaćanje pa kliknite **Spremi** kada ste zadovoljni.

    > [AZURE.IMPORTANT] Ako vam je potrebna da biste ažurirali ili promijenili računa payout, poduzmite iste korake iznad trenutne informacije zamjenjuje nove informacije. Promjena računa za payout može usporiti vaše plaćanja do jedne ciklusa naplate. Odgode pojavljuje jer je potrebna da biste potvrdili promjene računa, kao što smo jeste li kada najprije postavite račun payout. Će i dalje se plaćene puni iznos nakon potvrdio računa; sve uplate krajnji rok za isplatu trenutni ciklus dodat će se na sljedeću.

4. Kliknite **Dalje**.

**Informacije o poreza**

1. Prijavite se na [Razvojni centar za Microsoft](http://dev.windows.com/registration?accountprogram=azure) pomoću Microsoftova računa (Ako je potrebno).

2. Kliknite **profil porez** na lijevom izborniku.

3. Na stranici **Postavljanje obrasca za Porezno** odaberite državu ili regiju u kojoj imate trajna residency, a zatim odaberite državu ili regiju u kojoj držite primarni citizenship. Kliknite **Dalje**.

4. Unesite svoje detalje o porezu, a zatim kliknite **Dalje**.

> [AZURE.WARNING] Nećete moći automatske radnog vaše komercijalne nudi bez dovršetka informacije o porez i bankovnih računa na vašem računu Microsoft Developer Center.

Ako imate problema s Razvojni centar za registraciju, ponovno prijaviti zahtjev za podršku možete kao dolje

1. Idite na vezu za podršku [https://developer.microsoft.com/windows/support](https://developer.microsoft.com/windows/support)
2. U odjeljku **Kontaktirajte nas** kliknite gumb **Pošalji incident** (kao što je prikazano u nastavku snimka)

    ![Crtanje](media/marketplace-publishing-accounts-creation-registration/imgAddTax_02.png)

3. Odaberite "Pomoć s Razvojni centar za" kao **Vrsta problema** i "objavljivanje i upravljanje tim aplikacijama" kao **kategoriju**. Nakon toga kliknite gumb "Početak e-pošte".

    ![Crtanje](media/marketplace-publishing-accounts-creation-registration/imgAddTax_03.png)

4. Će se navesti sa stranicom za prijavu. Pomoću bilo kojeg Microsoftova računa za prijavu. Ako nemate Microsoftov račun, zatim stvorite pomoću ove [veze](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
5. Ispunite detalje problema i subit ulaznice klikom na gumb **Pošalji** .

    ![Crtanje](media/marketplace-publishing-accounts-creation-registration/imgAddTax_05.png)

## <a name="3-register-your-account-in-the-publishing-portal"></a>3. registrirali račun u portal za objavljivanje
[Portal za objavljivanje](http://publish.windowsazure.com) koristi se za objavljivanje i upravljanje vaše offer(s).

1. Otvorite novi Inkognito Chrome ili Internet Explorer InPrivate koji sesija pregledavanja da biste bili sigurni da niste prijavljeni osobni račun.

2. Idite na [http://publish.windowsazure.com](http://publish.windowsazure.com).

3. Ako ste novi korisnik i prva prijava u objavljivanje portala prvi put, zatim morate se prijaviti pomoću isti id e-pošte kojom se račun Razvojni centar za registriran. Na taj način vaš Razvojni centar za račun i račun za portala za objavljivanje će se povezati s drugom. Naknadno možete dodati ostalih članova tvrtke, koji rade na aplikaciju, ko-administrator u objavljivanje portala slijedeći korake u nastavku.

Ako ste dodali ko-administrator u objavljivanje portala, pa se možete prijaviti pomoću ko administratorskog računa.

  > [AZURE.TIP] Pravila za sudjelovanje opisani su na sustava [Azure web-mjesta](https://azure.microsoft.com/support/legal/marketplace/participation-policies/).

## <a name="4-steps-to-add-a-co-admin-in-the-publishing-portal"></a>4. korake da biste dodali administratore ko u objavljivanje portal
**Uz pretpostavku da ste administrator sustava** dolje su korake da biste dodali administrator ko

>[AZURE.NOTE] **Za nove korisnike,** prije nego što dodate ko administrator u objavljivanje portala bili sigurni da ste stvorili barem jedan aplikacije u objavljivanje portal. Ovo je obavezan kao što je kartica **IZDAVAČI** pojavljuje se tek nakon stvaranja barem jedan aplikacije u objavljivanje portal.

1. Provjerite je li ko administrator e-pošte ID-a Microsoft account(MSA). Ako nije, registrirati kao MSA pomoću ove [veze](https://signup.live.com/signup?uaid=0089f09ccae94043a0f07c2aaf928831&lic=1).
2. Provjerite postoji li barem jedan aplikacije u odjeljku administratorskog računa prije no što pokušate dodati administrator ko
3. Po završetku prema gore navedenim uputama se prijave za objavljivanje portala s id ko administrator e-pošte i zatim zapisnika odgovor.
4. Sada prijave za objavljivanje portala administrator e-pošte ID-ja.
5. Dođite do izdavači -> odaberite svoj račun -> administratori -> Dodaj ko-administrator (snimka dolje)

  ![Crtanje](media/marketplace-publishing-accounts-creation-registration/imgAddAdmin_05.png)

## <a name="5-steps-to-delete-a-co-admin-in-the-publishing-portal"></a>5. korake da biste izbrisali je administrator ko u objavljivanje portal
**Uz pretpostavku da ste administrator sustava** navedene u nastavku su navedeni koraci za da biste izbrisali administrator ko

1. Prijavite se na objavljivanje portala administrator e-pošte ID-ja.
2. Dođite do **izdavači** -> odaberite svoj račun -> **Administratori** -> **- Suadministratora**.
3. Kliknite gumb sa **znakom X** pokraj ko-administrator želite osiguranje Izbriši (snimka dolje).

    ![Crtanje](media/marketplace-publishing-accounts-creation-registration/imgDeleteAdmin_03.png)

## <a name="next-steps"></a>Daljnji koraci
Sad kad račun se stvara i registrirana, provjerite je li ispunjavanje ili zadovoljavaju sve nisu tehničke prirode stara requisites da biste objavili tu ponudu tako da pročitate [nisu tehničke prirode prije requisites](marketplace-publishing-pre-requisites.md).

## <a name="see-also"></a>Vidi također
- [Početak rada: kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md)

[img-msalive]:media/marketplace-publishing-accounts-creation-registration/creating-msa-account-msa-live.jpg
[img-email]:media/marketplace-publishing-accounts-creation-registration/creating-msa-account-msa-verifyemail.jpg
[img-sd-url]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-incognito.jpg
[img-signin]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-login.jpg
[img-verify]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-verify.jpg
[img-sd-top]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-acc-details.jpg
[img-sd-info]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal.jpg
[img-sd-type]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-acc-type.jpg
[img-sd-mktg1]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-det1.jpg
[img-sd-mktg2]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-det2.jpg
[img-sd-addr]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-comp-add.jpg
[img-sd-legal]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-personal-cmp.jpg
[img-sd-submit]:media/marketplace-publishing-accounts-creation-registration/seller-dashboard-approval.jpg

[link-msdndoc]: https://msdn.microsoft.com/library/jj552460.aspx
[link-sellerdashboard]: http://sellerdashboard.microsoft.com/
[link-pubportal]: https://publish.windowsazure.com
[link-single-vm]:marketplace-publishing-vm-image-creation.md
[link-single-vm-prereq]:marketplace-publishing-vm-image-creation-prerequisites.md
[link-multi-vm]:marketplace-publishing-solution-template-creation.md
[link-multi-vm-prereq]:marketplace-publishing-solution-template-creation-prerequisites.md
[link-datasvc]:marketplace-publishing-data-service-creation.md
[link-datasvc-prereq]:marketplace-publishing-data-service-creation-prerequisites.md
[link-devsvc]:marketplace-publishing-dev-service-creation.md
[link-devsvc-prereq]:marketplace-publishing-dev-service-creation-prerequisites.md
[link-pushstaging]:marketplace-publishing-push-to-staging.md
