
<properties
    pageTitle="Pomoću certifikata na paket Enterprise Integracija | Microsoft Azure"
    description="Saznajte kako pomoću certifikata pomoću paket Enterprise integracije i logike aplikacija"
    services="logic-apps"
    documentationCenter=".net,nodejs,java"
    authors="msftman"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="deonhe"/>

# <a name="learn-about-certificates-and-enterprise-integration-pack"></a>Dodatne informacije o potvrde i paket Enterprise Integracija

## <a name="overview"></a>Pregled
Integracija Enterprise koristi certifikata radi zaštite B2B komunikacije. U aplikacijama za integraciju enterprise možete koristiti dvije vrste certifikata:

- Javni potvrde, koje morate kupili od ustanove za izdavanje certifikata (CA).
- Privatni potvrde, koje možete sami izdati. Ove potvrde su ponekad se naziva samopotpisane potvrde.


## <a name="what-are-certificates"></a>Što su certifikati?
Certifikati su digitalnih dokumenata koji provjeri identitet sudionika u elektroničkom komunikacije i i sigurne elektroničke komunikacije.

## <a name="why-use-certificates"></a>Zašto koristiti certifikata?
Ponekad B2B komunikacije mora biti zadržane Povjerljivo. Integracija Enterprise koristi certifikata radi zaštite takve poruke na dva načina:

- Šifriranjem sadržaj poruke
- Po digitalno potpisivanje poruka  

## <a name="how-do-you-upload-certificates"></a>Kako prenijeti certifikata

### <a name="public-certificates"></a>Javni certifikata
Da biste koristili *javni certifikat* u aplikacijama za logiku s mogućnostima B2B, najprije morate prenijeti certifikata na račun za integraciju. Da biste koristili *samopotpisani certifikat*, s druge strane, morate najprije je prenijeti [Azure ključ sigurnog](../key-vault/key-vault-get-started.md "Informirajte se o sigurnog ključ").

Kada prenesete certifikat, on je dostupan da biste lakše sigurne poruke B2B prilikom definiranja njihova svojstva u [ugovore](./app-service-logic-enterprise-integration-agreements.md) koje ste stvorili.  

Evo detaljne upute za prijenos javnih potvrda na račun za integraciju nakon prijave na portal sustava Azure:

1. Odaberite **Pregledaj**.  
    ![Odaberite Pregledaj](./media/app-service-logic-enterprise-integration-overview/overview-1.png)  

2. **Integracija** unesite u okvir za pretraživanje filtar, a zatim **Račune za integraciju** s popisa rezultata.     
    ![Odaberite računi Integracija](./media/app-service-logic-enterprise-integration-overview/overview-2.png)

3. Odaberite račun integracija u koju želite dodati certifikata.  
    ![Odaberite račun za integraciju u koju želite dodati certifikata](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4.  Odaberite pločicu **certifikata** .  
    ![Odaberite pločicu certifikata](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)

5. U **Certifikati** plohu koji će se otvoriti odaberite gumb **Dodaj** .
    ![Odaberite gumb Dodaj](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Unesite **naziv** za potvrdu, a zatim odaberite vrstu certifikata. (U ovom se primjeru koristi vrstu javni certifikat.) Zatim odaberite ikonu mape na desnoj strani tekstnog okvira **certifikata** .

7. Kad se otvori alata za odabir datoteke, pronađite i odaberite certifikat datoteku koju želite prenijeti na račun za integraciju.

8. Odaberite certifikat, a zatim odaberite **u redu** u alatu za odabir datoteke. Provjerava i prenosi certifikata na račun za integraciju.

8. Na kraju, ponovno uključite plohu **Dodaj certifikat** odaberite gumb **u redu** .  
    ![Odaberite gumb u redu](./media/app-service-logic-enterprise-integration-certificates/certificate-3.png)  

9. U minutu, vidjet ćete obavijest koja pokazuje da prijenos potvrdu dovršetka.

10. Odaberite pločicu **certifikata** . Trebali biste vidjeti novododani certifikata.  
    ![Pogledajte novi certifikat](./media/app-service-logic-enterprise-integration-certificates/certificate-4.png)  

### <a name="private-certificates"></a>Privatne potvrde
Privatne potvrde koje možete prenijeti na račun za integraciju tako da poduzmete sljedeće korake:  

1. [Prijenos vaš privatni ključ za sigurnog ključ] (../key-vault/key-vault-get-started.md "Dodatne informacije o ključa zbirke ključeva").  

    > [AZURE.TIP] Morate Autorizirajte značajku logike aplikacije Azure aplikacije servisa za izvođenje operacije na sigurnog ključ. Dopustiti pristup upravitelju logike aplikacije servisa pomoću sljedeće naredbe ljuske PowerShell:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`  

2. Stvaranje privatne certifikata.  

3. Privatni certifikat prenijeti na račun za integraciju.

Nakon što ste snimili prethodne korake, privatni certifikat možete koristiti da biste stvorili ugovore.

Ovo su detaljne upute za prijenos privatne potvrde na račun za integraciju nakon prijave na portal sustava Azure:  

1. Odaberite **Pregledaj**.  
    ![Prijenos privatne potvrde na račun za integraciju](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    

2. **Integracija** unesite u okvir za pretraživanje filtar, a zatim **Račune za integraciju** s popisa rezultata.     
    ![Odaberite računi Integracija](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  

3. Odaberite račun integracija u koju želite dodati certifikata.  
    ![Odaberite račun za integraciju u koju želite dodati certifikata](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  

4. Odaberite pločicu **certifikata** .  
    ![Odaberite pločicu certifikata](./media/app-service-logic-enterprise-integration-certificates/certificate-1.png)  

5. U **Certifikati** plohu koji će se otvoriti odaberite gumb **Dodaj** .
    ![Odaberite gumb Dodaj](./media/app-service-logic-enterprise-integration-certificates/certificate-2.png)

6. Unesite **naziv** za potvrdu, a zatim odaberite vrstu certifikata. (U ovom se primjeru koristi vrstu javni certifikat.) Zatim odaberite ikonu mape na desnoj strani tekstnog okvira **certifikata** .

7. Kad se otvori alata za odabir datoteke, pronađite i odaberite certifikat datoteku koju želite prenijeti na račun za integraciju.

8. Nakon što ste odabrali certifikata, odaberite **u redu** u alatu za odabir datoteke. Ova akcija Provjeri valjanost certifikat i prenosi na račun za integraciju.

9. Na kraju, ponovno uključite plohu **Dodaj certifikat** odaberite gumb **u redu** .  
    ![Dodavanje certifikata](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-1.png)  

10. U minutu, vidjet ćete obavijest koja pokazuje da prijenos potvrdu dovršetka.

11. Odaberite pločicu **certifikata** . Trebali biste vidjeti novododani certifikata.
    ![Pogledajte novi certifikat](./media/app-service-logic-enterprise-integration-certificates/privatecertificate-2.png)  

Kada prenesete certifikat, on je dostupan da biste lakše sigurne poruke B2B prilikom definiranja njihova svojstva u [ugovore](./app-service-logic-enterprise-integration-agreements.md).  

## <a name="next-steps"></a>Daljnji koraci
- [Stvaranje logike aplikacije koja koristi B2B značajke](./app-service-logic-enterprise-integration-b2b.md)  
- [Stvaranje B2B ugovor](./app-service-logic-enterprise-integration-agreements.md)  
- [Dodatne informacije o sigurnog ključ] (../key-vault/key-vault-get-started.md "Dodatne informacije o ključa zbirke ključeva")  
