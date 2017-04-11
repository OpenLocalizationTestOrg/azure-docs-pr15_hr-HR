<properties
   pageTitle="Remediate OS slabe točke u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument pokazuje kako implementirati preporuke centar za sigurnost Azure **slabe točke Remediate OS**."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/17/2016"
   ms.author="terrylan"/>

# <a name="remediate-os-vulnerabilities-in-azure-security-center"></a>Remediate OS slabe točke u centru za sigurnost Azure

Centar za sigurnost Azure svakodnevno analizira operacijski sustav virtualnog računala (VM) (OS) za konfiguracije koji nije na VM provjerite više ranjivo preporučuje promjene konfiguracije za rješavanje tih slabe točke. Dodatne informacije o određenim konfiguracije prate potražite na [popisu preporučena konfiguracija pravila](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Centar za sigurnost preporučuje riješiti slabe točke kada vaš VM OS konfiguracije podudaraju preporučena konfiguracija pravila.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. U plohu **preporuke** odaberite **slabe točke Remediate OS**.
![Remediate OS slabe točke][1]

    Plohu **Remediate OS slabe točke** otvorit će se i popise na VMs s operacijskim Sustavom konfiguracijama koje zadovoljavaju pravila preporučena konfiguracija.  Za svaki VM na plohu navodi:

   - **Nije uspjelo pravila** – broj pravila koja se VM OS Konfiguriranje nije uspjelo.
   - **Vrijeme ZADNJEG SKENIRANJE** – datum i vrijeme centar za sigurnost zadnje skenirana u VM OS konfiguracije.
   - **STANJE** – trenutno stanje u slabe:

        - Otvaranje: U slabe sadrži ne je adresirana još
        - U tijeku: U slabe je trenutno primijenjena, koji je potreban nijedna akcija
        - Rješava: U slabe je već odgovorili (kad je problem riješen, stavka zasivljena je)
  - **TEŽINU** – sve slabe točke postavljene na težinu Nisko, što znači da je slabe moraju biti adresirane, ali ne zahtijeva hitnoj temi.

Odaberite na VM. Plohu za taj VM otvara i prikazuje pravila koja nije uspjela.
   ![Konfiguriranje pravila koja nije uspjela][2]

Odaberite pravilo. U ovom primjeru omogućuje odabir **Lozinka mora zadovoljavati složenosti**. Otvorit će se na plohu s opisom i utjecaj nije uspjelo pravila. Pregledajte pojedinosti i razmotrite primjene konfiguracije operacijski sustav.
  ![Opis pravila nije uspio][3]

  Centar za sigurnost koristi uobičajenih konfiguracije Enumeracije (CCE) za dodjelu odredišnih stilova za konfiguriranje pravila. Na ovom plohu daju se sljedeće informacije:

  - Naziv – Naziv pravila
  - TEŽINU – CCE težinu vrijednost ključnih, važne ili upozorenje
  - CCIED – CCE Jedinstveni identifikator pravila
  - Opis – Opis pravila
  - SLABE – Objašnjenje slabe ili rizika ako pravilo ne primjenjuje
  - UTJECAJ – Tvrtke utjecaj kada se pravilo primjenjuje
  - – OČEKIVANI vrijednost bi trebala kada je centar za sigurnost analizira konfiguraciju VM OS na temelju pravila
  - OPERACIJA pravilo – Pravilo operacija koristi centar za sigurnost tijekom analize konfiguracije VM OS na temelju pravila
  - STVARNA vrijednost – Nakon analizu konfiguracije VM OS na temelju pravila vraća vrijednost
  - REZULTAT IZVOĐENJA –-rezultat analize: prosljeđivanje nije uspjelo


## <a name="see-also"></a>Vidi također

U ovom se članku prikazivao kako implementirati preporuke centar za sigurnost "Remediate OS slabe točke." Možete pregledati skup pravila konfiguracije [ovdje](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335). Centar za sigurnost koristi CCE (Enumeracije uobičajenih konfiguracije) da biste dodijelili odredišnih stilova za konfiguriranje pravila. Posjetite web-mjesto [CCE](http://cce.mitre.org) dodatne informacije.

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – blog traženje članaka o Azure sigurnost i usklađenost

<!--Image references-->
[1]: ./media/security-center-remediate-os-vulnerabilities/recommendation.png
[2]:./media/security-center-remediate-os-vulnerabilities/vm-remediate-os-vulnerabilities.png
[3]: ./media/security-center-remediate-os-vulnerabilities/vulnerability-details.png
