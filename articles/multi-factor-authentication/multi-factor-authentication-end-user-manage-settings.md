<properties
    pageTitle="Upravljanje postavkama provjere dva koraka | Microsoft Azure"
    description="Upravljanje načinom koriste Azure višestruka provjera autentičnosti obuhvaća promjenu podataka o kontaktu ili konfiguriranje uređaja."
    services="multi-factor-authentication"
    keywords = "multifactor provjere autentičnosti klijent, problem provjere autentičnosti, ID korelacije"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="manage-your-settings-for-two-step-verification"></a>Upravljanje postavkama za potvrdu dva koraka

U ovom se članku navedeni odgovori na pitanja o ažuriranju postavki za provjeru potvrdu ili višestruke provjere autentičnosti. Ako imate poteškoća s prijavom u svoj račun, pogledajte [imate li poteškoća s dva koraka potvrdu](multi-factor-authentication-end-user-troubleshoot.md) za pomoć za otklanjanje poteškoća.


## <a name="where-to-find-the-settings-page"></a>Gdje pronaći na stranici Postavke
Vaša tvrtka postavljen Azure višestruke provjere autentičnosti, postoji nekoliko mjesta gdje možete promijeniti postavke kao što je telefonski broj.

Ako je vaš administrator šalje određeni URL ili koraka da biste upravljali potvrdu dva koraka, slijedite te upute. U suprotnom surađivati sljedeće upute za svih drugih. Ako slijedite ove korake, ali ne vidite iste mogućnosti, to znači da vaše tvrtke ili obrazovne ustanove prilagoditi vlastite portal. Veza na portal za Azure višestruka provjera autentičnosti zatražite od administratora sustava.


1. Prijavite se u [https://myapps.microsoft.com](https://myapps.microsoft.com)  
2. Pri vrhu odaberite **profil**.  
3. Odaberite **dodatne sigurnosti provjere**.  

    ![Myapps](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Dodatne sigurnosti potvrdu učitavanja stranice s postavkama.

    ![Proofup](./media/multi-factor-authentication-end-user-manage-myapps/proofup.png)


## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>Želim da biste promijenili Moji telefonski broj ili dodajte drugi broj

Važno je da konfiguriranje sekundarne provjere autentičnosti telefonski broj.  Budući da primarni telefonski broj i mobilna aplikacija vjerojatno na istom telefonu, sekundarne telefonski broj je jedini način moći da biste se vratili na račun ako izgubite ili krađe telefonu.

> [AZURE.NOTE]
> Ako ne imati pristup primarni telefonski broj, a potrebna pomoć za otvaranje vašem računu, potražite u našem temama pomoći u [imate li poteškoća s dva koraka provjere](multi-factor-authentication-end-user-troubleshoot.md).

**Da biste promijenili primarnog telefonski broj:**  

1. Na stranici Provjera dodatne sigurnosti odaberite tekstni okvir s trenutnom telefonski broj, a zatim Uredi pomoću novi broj telefona.  
2. Odaberite **Spremi**.  
3. Ako je broj koji koristite za mogućnosti zeleni, morate provjeriti novi broj prije možete ga spremiti.  


**Da biste dodali sekundarne telefonski broj:**  

1. Na stranici Provjera dodatne sigurnosti potvrdite okvir uz stavku **zamjenski provjere autentičnosti telefonski.**  
2. U tekstni okvir unesite broj sekundarne telefona.  
3. Odaberite **Spremi** i promjene se dovrši.  


## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>Kako očistiti Microsoft Authenticator iz stare uređaj i premjestiti u novi?
Kada ste deinstalirali aplikaciju na uređaju ili izvorne postavke uređaja, uklonite aktivaciju na pozadinska. Trebali biste slijedite korake navedene u [Prijelaz na novi uređaj](multi-factor-authentication-microsoft-authenticator.md#how-to-move-to-the-new-microsoft-authenticator-app).

## <a name="next-steps"></a>Daljnji koraci
- Savjeti za otklanjanje poteškoća i Olakšajte na [imate li poteškoća s dva koraka provjere](multi-factor-authentication-end-user-troubleshoot.md)
- Postavljanje [lozinke za aplikaciju](multi-factor-authentication-end-user-app-passwords.md) za sve aplikacije koji ne podržavaju potvrdu dva koraka.
