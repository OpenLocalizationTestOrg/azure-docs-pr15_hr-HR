<properties 
    pageTitle="Dodatne informacije o tvrtki paket za integraciju dekodiranje EDIFACT poruke poveznik | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
    description="Saznajte kako koristiti partnere s aplikacijama paket Enterprise integracije i logika" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="padmavc" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="padmavc"/>

# <a name="get-started-with-decode-edifact-message"></a>Početak rada s dekodiranje EDIFACT poruke

Provjeri valjanost svojstva specifične za partnera i uređivanje, stvara XML dokument za svaki skup transakcije i generira potvrdu obrađeni transakcije.

## <a name="create-the-connection"></a>Stvaranje veze

### <a name="prerequisites"></a>Preduvjeti

* Račun Azure; možete stvoriti [pomoću računa](https://azure.microsoft.com/free)

* Za korištenje dekodiranje EDIFACT poruke connector potreban je račun za integraciju. Podrobnije informirali o tome kako stvoriti [Račun za integraciju](./app-service-logic-enterprise-integration-create-integration-account.md), [partnere](./app-service-logic-enterprise-integration-partners.md) i [EDIFACT ugovor](./app-service-logic-enterprise-integration-edifact.md)

### <a name="connect-to-decode-edifact-message-using-the-following-steps"></a>Povezivanje s dekodiranje EDIFACT poruke pomoću sljedećih koraka:

1. [Stvaranje aplikacije za logiku](./app-service-logic-create-a-logic-app.md) navedeni primjer.

2. Ovaj poveznik ne sadrži sve okidača. Da biste pokrenuli aplikaciju logike, kao što je zahtjev za pokretanje pomoću drugih okidača.  U dizajneru logike aplikaciju, dodajte okidač i dodali akciju.  Odaberite Prikaži Microsoft upravljani API-ji padajući popis, a zatim unesite "EDIFACT" u okvir za pretraživanje.  Odaberite dekodiranje EDIFACT poruke

    ![pretraživanje EDIFACT](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage1.png)
    
3. Ako već niste stvorili sve veze s računom za integraciju, Zatraži pojedinosti veze

    ![Stvaranje računa za integraciju](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage2.png)  

4. Unesite detalje Integracija računa.  Svojstva zvjezdicom su potrebni

  	| Svojstvo | Pojedinosti |
  	| -------- | ------- |
  	| Naziv veze * | Unesite naziv neke za vezu |
  	| Integracija račun * | Unesite naziv računa za integraciju. Može li se račun integracije i logike aplikacije na istom mjestu Azure |

    Kada se dovrši, pojedinosti veze izgleda otprilike ovako

    ![račun za integraciju stvorili](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage3.png)  

5. Odaberite **Stvaranje**

6. Obratite pozornost na to je stvorena veza

    ![pojedinosti o računu veze za integraciju](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

7. Odaberite poruku nehijerarhijskom datotekom EDIFACT za dekodiranje

    ![Navedite obavezna polja](./media/app-service-logic-enterprise-integration-edifactorconnector/edifactdecodeimage5.png)  

## <a name="edifact-decode-does-following"></a>EDIFACT dekodiranje ne pratite

* Podudaranje pošiljatelj razdjelnik & identifikator i tekstnog okvira razdjelnik & identifikator razriješiti ugovor
* Dijeli više interchanges u jednu poruku u zasebna.
* Provjeri valjanost omotnica s partnerom trgovina
* Rastavlja razmjenu.
* Provjeri valjanost svojstva specifične za partnera i uređivanje obuhvaća
    * Provjera valjanosti strukture razmjenu omotnice.
    * Provjera valjanosti sheme omotnice u odnosu na shemu kontrolu.
    * Provjera valjanosti sheme elemenata transakcije skupu podataka prema shemi poruke.
    * Uređivanje provjere valjanosti izvršiti na elementi transakcije skupu podataka
* Potvrđuje da brojeva kontrola skup razmjenu, grupa i transakcije nisu duplicirani (Ako je konfiguriran) 
    * Provjerava broj kontrole za razmjenu protiv prethodno primljene interchanges. 
    * Provjerava broj kontrola grupe s ostalim brojevima grupu kontrola u razmjenu. 
    * Provjerava transakcije postavljanje kontrolu broja odnosu na druge transakcije skup kontrola brojeve u toj grupi.
* Stvara XML dokument za svaki skup transakcije.
* Pretvara cijelu razmjenu XML 
    * Podijeli razmjenu kao skupove transakcije – privremeno obustavljanje transakcije skupove pogreške: raščlanjuje pojedine transakcije Postavi u programa za razmjenu u zasebnom XML dokument. Ako je jedan ili više skupova transakcija u razmjenu ne prođu provjeru, zatim EDIFACT dekodiranje obustavlja samo one skupove transakcije. 
    * Podjela razmjenu kao skupove transakcije - odgoditi za razmjenu pogreške: raščlanjuje pojedine transakcije Postavi u programa za razmjenu u zasebnom XML dokument.  Ako je jedan ili više skupova transakcija u razmjenu ne prođu provjeru, zatim EDIFACT dekodiranje obustavlja cijelu razmjenu.
    * Očuvanje razmjenu – privremeno obustavljanje transakcije skupove pogreške: stvara XML dokument za cijelu odbacivanja razmjenu. EDIFACT dekodiranje obustavlja samo one skupove transakcije koje ne prođu provjeru tijekom nastavka za obradu sve druge skupove transakcije
    * Očuvanje razmjenu – privremeno obustavljanje razmjenu pogreške: stvara XML dokument za cijelu odbacivanja razmjenu. Ako je jedan ili više transakcije postavlja razmjenu ne prođu provjeru, zatim EDIFACT dekodiranje obustavlja cijelu razmjenu 
* Stvara Tehnički (kontrola) i/ili funkcionalni potvrdu (Ako je konfiguriran).
    * Tehnički potvrdu ili CONTRL ACK izvješća rezultate provjere syntactical dovršavanje primljenih razmjenu.
    * Funkcionalno potvrdu priznaje prihvatiti ili odbiti primljene razmjenu ili grupe

## <a name="next-steps"></a>Daljnji koraci

[Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Dodatne informacije o paket Enterprise Integracija") 