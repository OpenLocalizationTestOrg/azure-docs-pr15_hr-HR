<properties 
    pageTitle="Dodatne informacije o partnera i paket Enterprise Integracija | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
    description="Saznajte kako koristiti partnere s aplikacijama paket Enterprise integracije i logika" 
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

# <a name="learn-about-partners-and-enterprise-integration-pack"></a>Dodatne informacije o partnera i paket Enterprise Integracija

## <a name="overview"></a>Pregled
Da biste mogli stvarati partnera, i tvrtke ili ustanove namjeravate poslujete morate zajedničko korištenje informacija koje će vam prepoznavanje i poruke koje se šalju putem međusobno za provjeru valjanosti. Kada imate te raspravu i spremni ste započeti poslovni odnos, možete stvoriti *partnera* na vašem računu integracije.

## <a name="what-is-a-partner"></a>Što je partnera?
Partneri su entiteti koje sudjeluju u Business-To-Business (B2B) poruka i transakcije. 

## <a name="how-are-partners-used"></a>Kako se koriste partnerima?
Partneri se koriste za stvaranje ugovora. Ugovor definira detalje o poruke koje će se razmjenjivati između partnera. 

Da biste mogli stvarati ugovor, morate dodani najmanje dva partnera na račun za integraciju. Jedan od partnera za ugovor mora biti tvrtke ili ustanove. Partnera za tvrtke ili ustanove se nazivaju se **partnera glavnog računala**. Drugi partnera predstavljala druge tvrtke ili ustanove s kojom vašoj tvrtki ili ustanovi što poruke. Drugi partnera zove **partnera za goste**. Partnera za goste mogu biti neke druge tvrtke ili čak i na odjel u vašoj tvrtki ili ustanovi.  

Nakon što dodate na partnere, koristit ćete te partnere da biste stvorili ugovor. 

Primanja i slanja su postavke usmjeren iz prikaza slobode točke partnera hostira. Ako, na primjer, postavke primanje u ugovor određuju način glavnom računalu partnera Prima poruke poslane s partnera za goste. Isto tako, postavke za slanje ugovor pokazuju kako glavnom računalu partnera šalje poruke partnera za goste.

## <a name="how-to-create-a-partner"></a>Kako stvoriti partnera?
Na portalu Azure:  
1. Odaberite **Pregledaj**  
![](./media/app-service-logic-enterprise-integration-overview/overview-1.png)    
2. Unesite **Integracija** u okvir za pretraživanje filtar, a zatim odaberite **Računi za integraciju** s popisa rezultata     
 ![](./media/app-service-logic-enterprise-integration-overview/overview-2.png)  
3. Odaberite **račun za integraciju** na koje ćete dodati na partnere  
![](./media/app-service-logic-enterprise-integration-overview/overview-3.png)  
4.  Odaberite pločicu za **partnere**  
![](./media/app-service-logic-enterprise-integration-partners/partner-1.png)  
5. Odaberite gumb **Dodaj** u plohu partnere koji će se otvoriti  
![](./media/app-service-logic-enterprise-integration-partners/partner-2.png)  
6. Unesite **naziv** za partnera, a zatim odaberite **Razdjelnik **, na kraju, unesite **vrijednost**. Vrijednost se koristi da biste lakše identificirali dokumente koji se isporučuju u aplikacijama.  
![](./media/app-service-logic-enterprise-integration-partners/partner-3.png)  
7. Odaberite ikonu obavijesti *zvono* da biste vidjeli tijeku postupak stvaranja partnera.  
![](./media/app-service-logic-enterprise-integration-partners/partner-4.png)  
8. Odaberite pločicu **partnera** . To osvježava pločicu, a trebali biste vidjeti broj povećava partnera, o novog partnera dodana uspješno.    
![](./media/app-service-logic-enterprise-integration-partners/partner-5.png)  
10. Nakon što odaberete pločicu partnera, vidjet ćete i novododani partnera velikim plohu partnera.    
![](./media/app-service-logic-enterprise-integration-partners/partner-6.png)  

## <a name="how-to-edit-a-partner"></a>Upute za uređivanje partnera

Slijedite ove korake da biste uredili partnera koji već postoji u vašem računu Integracija:  
1. Odaberite pločicu za **partnere**  
2. Odaberite partnera koji želite uređivati kada otvara plohu za partnere  
3. Na pločici **Ažuriranje partnera** unesite željene promjene morate  
4. Ako ste zadovoljni s vašim promjenama, odaberite **Spremi** vezu, još, odaberite vezu **Odbaci** da bi Odsutan promjene.  
![](./media/app-service-logic-enterprise-integration-partners/edit-1.png)  

## <a name="how-to-delete-a-partner"></a>Kako izbrisati partnera
1. Odaberite pločicu za **partnere**  
2. Odaberite partnera koji želite uređivati kada otvara plohu za partnere  
3. Odaberite vezu za **Brisanje**    
![](./media/app-service-logic-enterprise-integration-partners/delete-1.png)   

## <a name="next-steps"></a>Daljnji koraci
- [Dodatne informacije o ugovora] (./app-service-logic-enterprise-integration-agreements.md "Informirajte se o ugovore Integracija enterprise")  


