<properties 
    pageTitle="Dodatne informacije o tvrtki paket za integraciju kodiranje X12 poruka Connctor | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
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

# <a name="get-started-with-encode-x12-message"></a>Početak rada s porukom Kodiraj X12

Provjeri valjanost svojstva specifične za partnera i uređivanje, pretvara XML šifrirana poruka u skupove transakcije uređivanje u razmjenu i zahtjeve Tehnički i/ili Functional potvrdu

## <a name="create-the-connection"></a>Stvaranje veze

### <a name="prerequisites"></a>Preduvjeti

* Račun Azure; možete stvoriti [pomoću računa](https://azure.microsoft.com/free)

* Za korištenje Kodiraj x12 poruke connector potreban je račun za integraciju. Podrobnije informirali o tome kako stvoriti [Račun integracije](./app-service-logic-enterprise-integration-create-integration-account.md), [partnere](./app-service-logic-enterprise-integration-partners.md) i [X12 ugovor](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-encode-x12-message-using-the-following-steps"></a>Povezivanje s Kodiraj X12 poruke na sljedeći način:

1. [Stvaranje aplikacije za logiku](./app-service-logic-create-a-logic-app.md) nudi primjer

2. Ovaj poveznik ne sadrži sve okidača. Da biste pokrenuli aplikaciju logike, kao što je zahtjev za pokretanje pomoću drugih okidača.  U dizajneru logike aplikaciju, dodajte okidač i dodali akciju.  Odaberite Prikaži Microsoft upravljani API-ji padajući popis, a zatim unesite "x12" u okvir za pretraživanje.  Odaberite neki X12-kodiranje X12 poruka prema nazivu ugovor ili X12-kodiranje 12 za X poruku tako da identiteta.  

    ![pretraživanje x12](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png) 

3. Ako već niste stvorili sve veze s računom za integraciju, Zatraži pojedinosti veze

    ![veza s računom Integracija](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage1.png) 


4. Unesite detalje Integracija računa.  Svojstva zvjezdicom su potrebni

  	| Svojstvo | Pojedinosti |
  	| -------- | ------- |
  	| Naziv veze * | Unesite naziv neke za vezu |
  	| Integracija račun * | Unesite naziv računa za integraciju. Može li se račun integracije i logike aplikacije na istom mjestu Azure |

    Kada se dovrši, pojedinosti veze izgleda otprilike ovako

    ![Integracija s programom stvorena veza s računom](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage2.png) 


5. Odaberite **Stvaranje**

6. Obratite pozornost na to je stvorena veza.

    ![pojedinosti o računu veze za integraciju](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage3.png) 

#### <a name="x12---encode-x12-message-by-agreement-name"></a>X12-kodiranje X12 poruka prema nazivu ugovor

7. Odaberite X12 ugovor s padajućeg popisa i xml poruke za kodiranje.

    ![Navedite obavezna polja](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage4.png) 

#### <a name="x12---encode-x12-message-by-identities"></a>X12-kodiranje X12 poruku putem identiteta

7.  Navedite identifikator pošiljatelja, razdjelnik pošiljatelja, identifikator tekstnog okvira i razdjelnik tekstnog okvira konfigurirano u na X12 ugovor.  Odaberite poruku xml za kodiranje

    ![Navedite obavezna polja](./media/app-service-logic-enterprise-integration-x12connector/x12encodeimage5.png) 

## <a name="x12-encode-does-following"></a>Kodiranje X12 ne pratite:

* Razlučivost ugovor tako da uspoređuje pošiljatelj i primatelj kontekst svojstva.
* Serializes razmjenu uređivanje, pretvaranje XML šifrirana poruka u skupove transakcije uređivanje u razmjenu.
* Primjenjuje transakcije skup zaglavlja i Filmska glazba odsječaka
* Generira broja kontrola za razmjenu, broj za kontrolu grupe i transakcija određenog broja kontrola za svaki odlazne razmjenu
* Zamjenjuje razdjelnici tereta podataka
* Provjeri valjanost svojstva specifične za partnera i uređivanje
    * Provjera valjanosti sheme elemenata transakcije skupa podataka na temelju poruke sheme
    * Uređivanje provjere valjanosti izvesti elementi transakcije skupu podataka.
    * Prošireni provjere valjanosti izvršiti na elementi transakcije skupu podataka
* Tehnički i/ili Functional potvrdu zahtjeve (Ako je konfiguriran).
    * Tehnički potvrdu generira uslijed zaglavlja provjere valjanosti. Tehnički potvrdu izvješća stanja obrada zaglavlja razmjenu i Filmska glazba tako da adresu primatelja
    * Funkcionalno potvrdu generira uslijed tijelo provjere valjanosti. Funkcionalno potvrdu izvješća svaki pogreška tijekom obrade primljene dokumenta

## <a name="next-steps"></a>Daljnji koraci

[Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Dodatne informacije o paket Enterprise Integracija") 

