<properties 
    pageTitle="Pregled računa za integraciju i integracija paket Enterprise | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
    description="Saznajte sve o računima za integraciju, paket Enterprise integracije i logike aplikacije" 
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
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-integration-accounts"></a>Pregled računa za integraciju

## <a name="what-is-an-integration-account"></a>Što je račun za integraciju?
Račun za integraciju je račun za Azure koji omogućuje integraciju Enterprise aplikacije da biste upravljali artefakte uključujući sheme, karte, certifikata, partnere i ugovore. Bilo koju aplikaciju za integraciju stvorite ćete morati koristiti Integracija račun da biste pristupili shemu, mapa ili certifikat, na primjer.

## <a name="create-an-integration-account"></a>Stvorite račun Integracija 
1. Odaberite **Pregledaj**   
![](./media/app-service-logic-enterprise-integration-accounts/account-1.png)  
2. Unesite **Integracija** u okvir za pretraživanje filtar, a zatim odaberite **Računi za integraciju** s popisa rezultata     
 ![](./media/app-service-logic-enterprise-integration-accounts/account-2.png)  
3. Na izborniku pri vrhu stranice odaberite gumb *Dodaj*      
![](./media/app-service-logic-enterprise-integration-accounts/account-3.png)  
4. Unesite **naziv**, odaberite **pretplatu** u koju želite koristiti, ili stvoriti novu **grupu resursa** ili odaberite postojeću grupu resursa, odaberite **mjesto** gdje računa za integraciju će se nalaziti **sloju određivanje cijena**, a zatim odaberite gumb **Stvori** .   

  Sada na račun za integraciju dodijelit će se Resursi na mjestu koje ste odabrali. Dovršite treba unutar 1 minute.    
![](./media/app-service-logic-enterprise-integration-accounts/account-4.png)  
5. Osvježite stranicu. Prikazat će se na novi račun Integracija na popisu. Čestitamo!  
![](./media/app-service-logic-enterprise-integration-accounts/account-5.png) 

## <a name="how-to-link-an-integration-account-to-a-logic-app"></a>Upute za povezivanje poslovnog subjekta Integracija logike aplikacije
Bi logike aplikacije da biste pristupili karte, sheme, ugovore i druge artefakte koje se nalaze na vašem računu integracije, morate najprije povezati račun za integraciju logike aplikacije.

### <a name="here-are-the-steps-to-link-an-integration-account-to-a-logic-app"></a>Evo nekoliko koraka da biste se povezali račun za integraciju logike aplikacije 

#### <a name="prerequisites"></a>Preduvjeti
- Račun za integraciju
- Logika aplikacije

>[AZURE.NOTE]Provjerite je li Integracija računa i logike aplikacije su na **istom mjestu Azure** prije početka

1. Odaberite **Postavke** veze iz izbornika aplikacije logike  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-1.png)   
2. Odaberite stavku **Računa za integraciju** s plohu postavke  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-2.png)   
3. Odaberite račun za integraciju se želite povezati logiku aplikacije iz **Odaberite račun Integracija** padajući okvir popisa  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-3.png)   
4. Spremanje rezultata rada  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-4.png)   
5. Prikazat će se obavijest koja pokazuje povezanih da računa za integraciju s aplikacijom logiku i sve artefakte na vašem računu Integracija dostupnosti sada u aplikaciju za logiku.  
![](./media/app-service-logic-enterprise-integration-accounts/linkaccount-5.png)   

Sad kad se računa za integraciju povezan s aplikacijom logike, možete da otvorite logike aplikacije i stvaranje aplikacije sa značajkama B2B pomoću poveznika B2B kao što je provjera valjanosti XML, Nehijerarhijskom datotekom kodiranje/dekodiranje ili pretvorbe.  
    
## <a name="how-to-delete-an-integration-account"></a>Kako izbrisati račun Integracija?
1. Odaberite **Pregledaj**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Unesite **Integracija** u okvir za pretraživanje filtar, a zatim odaberite **Računi za integraciju** s popisa rezultata     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Odaberite **račun za integraciju s** koje želite izbrisati  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Odaberite **Izbriši** vezu koja se nalazi na izborniku   
![](./media/app-service-logic-enterprise-integration-accounts/delete.png)  
5. Potvrdite svoj izbor    

## <a name="how-to-move-an-integration-account"></a>Kako premjestiti Integracija račun?
Račun za integraciju jednostavno možete premjestiti u novu pretplatu i novu grupu resursa. Ako vam je potrebna da biste premjestili računa za integraciju, slijedite ove korake:

>[AZURE.IMPORTANT] Morate li ažurirati sve skripte da biste koristili novi resurs ID-a nakon premještanja račun za integraciju.

1. Odaberite **Pregledaj**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Unesite **Integracija** u okvir za pretraživanje filtar, a zatim odaberite **Računi za integraciju** s popisa rezultata     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Odaberite **račun za integraciju s** koje želite izbrisati  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4. Odaberite **Premjesti** vezu koja se nalazi na izborniku   
![](./media/app-service-logic-enterprise-integration-accounts/move.png)  
5. Potvrdite svoj izbor    

## <a name="next-steps"></a>Daljnji koraci
- [Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Dodatne informacije o paket Enterprise Integracija")  
- [Dodatne informacije o ugovora] (./app-service-logic-enterprise-integration-agreements.md "Informirajte se o ugovore Integracija enterprise")  


 