<properties
    pageTitle="Pregled shema i integracija paket Enterprise | Microsoft Azure"
    description="Saznajte kako pomoću sheme s aplikacijama paket Enterprise integracije i logika"
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
    ms.date="07/29/2016"
    ms.author="deonhe"/>

# <a name="learn-about-schemas-and-the-enterprise-integration-pack"></a>Dodatne informacije o sheme i integracija paket Enterprise  

## <a name="why-use-a-schema"></a>Zašto koristiti shemu?
Provjerite jesu li XML dokumente primite valjani očekivane podatke u unaprijed definiranog oblika pomoću sheme. Shema se koriste za provjeru valjanosti poruke koje su izmjenjivati u scenariju B2B.

## <a name="add-a-schema"></a>Dodavanje sheme
Na portalu Azure:  

1. Odaberite **više usluga**.  
![Snimka zaslona Azure akcije s "Dodatnih servisa" istaknuta](./media/app-service-logic-enterprise-integration-overview/overview-11.png)    

2. U okvir za pretraživanje filtar unesite **Integracija**pa odaberite **Računi za integraciju** s popisa rezultata.     
![Snimka zaslona okvira za pretraživanje filtra](./media/app-service-logic-enterprise-integration-overview/overview-21.png)  
3. Odaberite **račun za integraciju** kojoj dodate u shemi.    
![Snimka zaslona s popisa Integracija računa](./media/app-service-logic-enterprise-integration-overview/overview-31.png)  

4. Odaberite pločicu **sheme** .  
![Snimka zaslona s IEP Integracija računa, s "Shemama" istaknuta](./media/app-service-logic-enterprise-integration-schemas/schema-11.png)  

### <a name="add-a-schema-file-less-than-2-mb"></a>Dodavanje datoteke sheme manji od 2 MB  

1. U **sheme** plohu koji se otvara (prethodnih koraka) odaberite **Dodaj**.  
![Snimka zaslona sheme plohu, s istaknutim gumbom "Dodaj"](./media/app-service-logic-enterprise-integration-schemas/schema-21.png)  

2. Unesite naziv sheme. Zatim da biste prenijeli datoteke sheme, odaberite ikonu mape uz **sheme** tekstni okvir. Po dovršetku postupka za prijenos odaberite **u redu**.    
![Snimka zaslona "Dodaj shemu", "Small datoteku za" istaknuta](./media/app-service-logic-enterprise-integration-schemas/schema-31.png)  

### <a name="add-a-schema-file-larger-than-2-mb-up-to-a-maximum-of-8-mb"></a>Dodavanje datoteke sheme veće od 2 MB (maksimalno do 8 MB)  

Postupak za to ovisi o razini pristupa blob spremnik: **javno** ili **anonimni pristup**. Da biste odredili pristup razinu **Explorer Azure prostora za pohranu**, u odjeljku **Blob spremnika**, odaberite željeni spremnik blob. Odaberite **Sigurnost**, a zatim odaberite karticu **Razinu pristupa** .

1. Ako je razina pristupa za sigurnost blob **javno**, slijedite ove korake.  
  ![Snimka zaslona programa Azure prostora za pohranu Explorer, s "Blob spremnika", "Sigurnost" i "Javno" istaknuta](./media/app-service-logic-enterprise-integration-schemas/blob-public.png)  

    na. Prijenos u shemi prostora za pohranu i kopirajte URI.  
    ![Snimka zaslona s računom za pohranu, s istaknutim URI-jem](./media/app-service-logic-enterprise-integration-schemas/schema-blob.png)  

    b. **Dodaj shemu**, odaberite **velike datoteke**, a omogućuju URI u tekstni okvir **Sadržaja URI** .  
    ![Snimka zaslona sheme, pomoću gumba "Dodaj" i "Velike datoteke" istaknuta](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

2. Ako je razina pristupa za sigurnost blob **anonimni pristup**, slijedite ove korake.  
  ![Snimka zaslona programa Azure prostora za pohranu Explorer, s "Blob spremnika", "Sigurnost" i "Nema anonimni pristup" istaknuta](./media/app-service-logic-enterprise-integration-schemas/blob-1.png)  

    na. Prenesite shemi prostora za pohranu.  
    ![Snimka zaslona s računa za pohranu](./media/app-service-logic-enterprise-integration-schemas/blob-3.png)

    b. Stvaranje potpisa zajednički pristup za shemu.  
    ![Snimka zaslona sccount prostora za pohranu, s istaknuta je kartica zajednički pristup potpisi](./media/app-service-logic-enterprise-integration-schemas/blob-2.png)

    c. **Dodaj shemu**, odaberite **velike datoteke**, a omogućuje zajednički pristup potpis URI u tekstni okvir **Sadržaja URI** .  
    ![Snimka zaslona sheme, pomoću gumba "Dodaj" i "Velike datoteke" istaknuta](./media/app-service-logic-enterprise-integration-schemas/schema-largefile.png)  

3. U plohu **sheme** računa za integraciju EIP, trebali biste vidjeti sada shemom za novododani.  
![Snimka zaslona s EIP Integracija računa, s "Sheme" i novu shemu istaknuta](./media/app-service-logic-enterprise-integration-schemas/schema-41.png)
  

## <a name="edit-schemas"></a>Uređivanje sheme
1. Odaberite pločicu **sheme** .  
2. Odaberite shemu koju želite urediti iz plohu **sheme** koji će se otvoriti.
3. Na plohu **sheme** odaberite **Uređivanje**.  
![Snimka zaslona sheme plohu](./media/app-service-logic-enterprise-integration-schemas/edit-12.png)    
4. Odaberite datoteke sheme koju želite urediti pomoću alata za odabir dijaloškog okvira datoteku koja će se otvoriti.
5. Odaberite **Otvori** u alatu za odabir datoteke.  
![Snimka zaslona za odabir datoteka](./media/app-service-logic-enterprise-integration-schemas/edit-31.png)  
6. Primit ćete obavijest koja pokazuje da prijenos nije uspio.  

## <a name="delete-schemas"></a>Brisanje sheme
1. Odaberite pločicu **sheme** .  
2. Odaberite shemu koju želite izbrisati iz plohu **sheme** koji će se otvoriti.  
3. Na plohu **sheme** odaberite **Izbriši**.
![Snimka zaslona sheme plohu](./media/app-service-logic-enterprise-integration-schemas/delete-12.png)  

4. Da biste potvrdili izboru, odaberite **da**.  
![Snimka zaslona poruke o potvrdi "Izbriši sheme"](./media/app-service-logic-enterprise-integration-schemas/delete-21.png)  
5. Na kraju, obratite pozornost na to da na popisu shema u plohu **sheme** osvježava i shemi koju ste izbrisali uklonjena s popisa.  
![Snimka zaslona s EIP Integracija računa, s "Shemama" istaknuta](./media/app-service-logic-enterprise-integration-schemas/delete-31.png)    

## <a name="next-steps"></a>Daljnji koraci

- [Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Informirajte se o Integracija paket enterprise").  
