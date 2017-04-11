<properties
   pageTitle="Navedite sigurnost podataka o kontaktima u centru za sigurnost Azure | Microsoft Azure"
   description="Ovaj dokument prikazuje kako unijeti sigurnost podataka o kontaktima u centru za sigurnost Azure."
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

# <a name="provide-security-contact-details-in-azure-security-center"></a>Navedite sigurnost podataka o kontaktima u centru za sigurnost Azure

Centar za sigurnost Azure će preporučujemo da unesete sigurnost podataka o kontaktima za pretplatu Azure ako to već niste učinili. Ti podaci će se koristiti Microsoft s vama u kontakt ako Microsoft sigurnost odgovor centra (MSRC) otkrije da korisničkih podataka pristupanja strana nezakonitom ili neovlaštenih. MSRC izvodi odaberite sigurnost nadzor Azure mreže i Infrastruktura i prima prijetnju obavještavanje i zloupotreba pritužbe iz trećim stranama.

Obavijest e-poštom šalje se na prvo pojavljivanje dnevnih upozorenja i samo za visoke težinu upozorenja. Postavke e-pošte moguće je konfigurirati za pretplatu pravila. Grupa resursa u pretplatu će naslijediti ove postavke.

> [AZURE.NOTE] Ovaj dokument predstavlja servis pomoću implementacije sustava primjer.  Ovo nije Postupni vodič.

## <a name="implement-the-recommendation"></a>Implementacija u preporuka

1. U plohu **preporuke** odaberite **sigurnost osiguraj pojedinosti o kontaktu**.
![Navedite sigurnost kontakta][1]

2. Otvorit će se plohu **osiguraj sigurnost pojedinosti o kontaktu**. Odaberite Azure pretplatu za navođenje podataka za kontakt na.
![Navedite sigurnost podataka o kontaktima][2]

3. Otvorit će se drugi plohu **unijeti sigurnosni detalje o kontaktu** .

  - Unesite adresu e-pošte za kontakt za sigurnost ili adresa odvojenih točkama sa zarezom. Nema ograničenja broja adrese e-pošte koje možete unijeti.
  - Unesite jedan kontakt međunarodnog telefonskog broja za sigurnost.
  - Da biste primali visoke težinu upozorenja o porukama e-pošte, uključite mogućnost **Pošalji mi poruke e-pošte upozorenja o**.
  - U budućnosti, imat ćete mogućnost slanja obavijesti e-poštom vlasnicima pretplate. Ta mogućnost trenutno zasivljena.
  - Odaberite **u redu** da biste primijenili sigurnost podataka za kontakt za svoju pretplatu.

## <a name="see-also"></a>Vidi također

Da biste saznali više o centru za sigurnost, pogledajte sljedeće:

- [Postavljanje pravila zaštite u centru za sigurnost Azure](security-center-policies.md) – upute za konfiguriranje sigurnosnih pravilnika za Azure pretplate i grupa resursa.
- [Upravljanje preporuke o sigurnosti u centru za sigurnost Azure](security-center-recommendations.md) – Saznajte kako preporuke olakšavaju zaštita Azure resurse.
- [Nadzor sigurnost stanja u centru za sigurnost Azure](security-center-monitoring.md) – upute za praćenje stanja Azure resurse.
- [Upravljanje i odgovarati na njih sigurnosnih upozorenja u centru za sigurnost Azure](security-center-managing-and-responding-alerts.md) – upute za upravljanje i odgovaranje na sigurnosnih upozorenja.
- [Nadzor partnerskih rješenja s centar za sigurnost Azure](security-center-partner-solutions.md) – upute za praćenje stanja status partnerskih rješenja.
- [Najčešća Pitanja za centar za sigurnost azure](security-center-faq.md) – traženje često postavljana pitanja o korištenju servisa.
- [Sigurnost azure blog](http://blogs.msdn.com/b/azuresecurity/) – traženje najnovije vijesti Azure sigurnost i informacija.

<!--Image references-->
[1]: ./media/security-center-provide-security-contacts/provide-contacts.png
[2]:./media/security-center-provide-security-contacts/provide-contact-details.png
