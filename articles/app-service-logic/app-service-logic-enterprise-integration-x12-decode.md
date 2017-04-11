<properties 
    pageTitle="Dodatne informacije o tvrtki paket za integraciju dekodiranje X12 poruka Connctor | Aplikacije servisa za Microsoft Azure | Microsoft Azure" 
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

# <a name="get-started-with-decode-x12-message"></a>Početak rada s porukom dekodiranja X12

Provjeri valjanost svojstva specifične za partnera i uređivanje, stvara XML dokument za svaki skup transakcije i generira potvrdu obrađeni transakcije.

## <a name="create-the-connection"></a>Stvaranje veze

### <a name="prerequisites"></a>Preduvjeti

* Račun Azure; možete stvoriti [pomoću računa](https://azure.microsoft.com/free)

* Za korištenje dekodiranja X12 poruke connector potreban je račun za integraciju. Podrobnije informirali o tome kako stvoriti [Račun integracije](./app-service-logic-enterprise-integration-create-integration-account.md), [partnere](./app-service-logic-enterprise-integration-partners.md) i [X12 ugovor](./app-service-logic-enterprise-integration-x12.md)

### <a name="connect-to-decode-x12-message-using-the-following-steps"></a>Povezivanje s dekodiranja X12 poruke na sljedeći način:

1. [Stvaranje aplikacije za logiku](./app-service-logic-create-a-logic-app.md) nudi primjer

2. Ovaj poveznik ne sadrži sve okidača. Da biste pokrenuli aplikaciju logike, kao što je zahtjev za pokretanje pomoću drugih okidača.  U dizajneru logike aplikaciju, dodajte okidač i dodali akciju.  Odaberite Prikaži Microsoft upravljani API-ji padajući popis, a zatim unesite "x12" u okvir za pretraživanje.  Odaberite X12 – dekodiranje X12 poruke

    ![pretraživanje x12](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage1.png)  

3. Ako već niste stvorili sve veze s računom za integraciju, Zatraži pojedinosti veze

    ![veza s računom Integracija](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage4.png)    

4. Unesite detalje Integracija računa.  Svojstva zvjezdicom su potrebni

  	| Svojstvo | Pojedinosti |
  	| -------- | ------- |
  	| Naziv veze * | Unesite naziv neke za vezu |
  	| Integracija račun * | Unesite naziv računa za integraciju. Može li se račun integracije i logike aplikacije na istom mjestu Azure |

    Kada se dovrši, pojedinosti veze izgleda otprilike ovako
    
    ![Integracija s programom stvorena veza s računom](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage5.png) 

5. Odaberite **Stvaranje**
    
6. Obratite pozornost na to je stvorena veza.

    ![pojedinosti o računu veze za integraciju](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage6.png) 

7. Odaberite X12 plošna poruka datoteke za dekodiranje

    ![Navedite obavezna polja](./media/app-service-logic-enterprise-integration-x12connector/x12decodeimage7.png) 

## <a name="x12-decode-does-following"></a>X12 dekodiranje ne pratite

* Provjeri valjanost omotnica s partnerom trgovina
* Stvara XML dokument za svaki skup transakcije.
* Provjeri valjanost svojstva specifične za partnera i uređivanje
    * Uređivanje strukturnih provjere valjanosti, a zatim Provjera valjanosti prošireni sheme
    * Provjera valjanosti strukture razmjenu omotnice.
    * Provjera valjanosti sheme omotnice u odnosu na shemu kontrolu.
    * Provjera valjanosti sheme elemenata transakcije skupu podataka prema shemi poruke.
    * Uređivanje provjere valjanosti izvršiti na elementi transakcije skupu podataka 
* Provjerava brojeva kontrola skup razmjenu, grupa i transakcije nisu duplicirani
    * Provjerava broj kontrole za razmjenu protiv prethodno primljene interchanges.
    * Provjerava broj kontrola grupe s ostalim brojevima grupu kontrola u razmjenu.
    * Provjerava transakcije postavljanje kontrolu broja odnosu na druge transakcije skup kontrola brojeve u toj grupi.
* Pretvara cijelu razmjenu XML 
    * Podijeli razmjenu kao skupove transakcije – privremeno obustavljanje transakcije skupove pogreške: raščlanjuje pojedine transakcije Postavi u programa za razmjenu u zasebnom XML dokument. Ako je jedan ili više transakcije postavlja razmjenu ne prođu provjeru, X12 dekodiranja obustavlja samo one skupove transakcije.
    * Podjela razmjenu kao skupove transakcije - odgoditi za razmjenu pogreške: raščlanjuje pojedine transakcije Postavi u programa za razmjenu u zasebnom XML dokument.  Ako je jedan ili više transakcije postavlja razmjenu ne prođu provjeru, X12 dekodiranja obustavlja cijelu razmjenu.
    * Očuvanje razmjenu – privremeno obustavljanje transakcije skupove pogreške: stvara XML dokument za cijelu odbacivanja razmjenu. X12 dekodiranja obustavlja samo one skupove transakcije koje ne prođu provjeru, tijekom nastavka za obradu druge transakcije postavlja
    * Očuvanje razmjenu – privremeno obustavljanje razmjenu pogreške: stvara XML dokument za cijelu odbacivanja razmjenu. Ako je jedan ili više transakcije postavlja razmjenu ne prođu provjeru, X12 dekodiranja obustavlja cijelu razmjenu 
* Tehnički i/ili Functional potvrdu generira (Ako je konfiguriran).
    * Tehnički potvrdu generira uslijed zaglavlja provjere valjanosti. Tehnički potvrdu izvješća stanja obrada zaglavlja razmjenu i Filmska glazba tako da adresu primatelja.
    * Funkcionalno potvrdu generira uslijed tijelo provjere valjanosti. Funkcionalno potvrdu izvješća svaki pogreška tijekom obrade primljene dokumenta

## <a name="next-steps"></a>Daljnji koraci

[Dodatne informacije o Integracija paket Enterprise] (./app-service-logic-enterprise-integration-overview.md "Dodatne informacije o paket Enterprise Integracija") 


