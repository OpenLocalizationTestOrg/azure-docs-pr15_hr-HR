<properties
   pageTitle="Kako zadržati konstantnim virtualna IP adresa za servise u oblaku | Microsoft Azure"
   description="Saznajte kako da biste bili sigurni da ne mijenja virtualne IP adrese (VIP) na servisu Azure oblaka."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-retain-a-constant-virtual-ip-address-for-a-cloud-service"></a>Kako zadržati konstantnim virtualna IP adresa za servise u oblaku

Kada ažurirate servis u oblaku koja se nalazi u Azure, možda morati provjerite ne mijenjaju virtualne IP adrese (VIP) servis. Mnoge servise za upravljanje domenom pomoću sustava naziva domena (DNS) za registriranje naziva domena. DNS-a funkcionira samo ako je na VIP ostaje isto. **Čarobnjak za objavljivanje** u alatima za Azure možete koristiti da biste bili sigurni da VIP servis u oblaku ne mijenja prilikom ažuriranja. Dodatne informacije o načinu korištenja upravljanja DNS-a domene za servise u oblaku, potražite u članku [Konfiguriranje prilagođenog naziva domene za Azure oblaku](./cloud-services/cloud-services-custom-domain-name.md).

## <a name="publishing-a-cloud-service-without-changing-its-vip"></a>Objavljivanje na servis u oblaku bez promjene njegova VIP

VIP servis u oblaku dodijeljeno kada prvi put njegove implementacije kod Azure u okruženju, kao što su radnog okruženja. Na VIP ne mijenja osim ako izričito brisanje implementacijskih ili ga implicitno izbrisao postupka implementacije ažuriranja. Da biste zadržali na VIP, ne morate izbrisati implementaciju sustava, a morate i provjeriti je li Visual Studio ne briše implementaciju sustava automatski. Ponašanje možete kontrolirati tako da navedete postavki uvođenja u **Čarobnjak za objavljivanje**koji podržava nekoliko mogućnosti implementacije. Možete navesti Osvježi uvođenje i implementaciju ažuriranja, što može biti rastuće ili istodobno, i obje vrste ažuriranja implementacijama zadržali na VIP. Definicija različite vrste implementaciju, potražite u članku [Objavljivanje čarobnjak aplikacije za Azure](vs-azure-tools-publish-azure-application-wizard.md).  Osim toga, možete kontrolirati li prethodne implementacije servis u oblaku izbrisati ako dođe do pogreške. Na VIP neočekivano može se promijeniti ako ne postavite tu mogućnost pravilno.

### <a name="to-update-a-cloud-service-without-changing-its-vip"></a>Da biste ažurirali servis u oblaku bez promjene njegova VIP

1. Kada se barem jednom implementirali servis u oblaku, otvorite izbornički prečac za čvor Azure projekta, a zatim odaberite **Objavi**. Pojavit će se čarobnjak za **Objavljivanje Azure aplikacije** .

1. Na popisu pretplate odaberite onu koju želite uvesti, a zatim odaberite gumb **Dalje** . Pojavit će se **Postavke** stranice čarobnjaka.

1. Na kartici **Uobičajene postavke** naziv servisa u oblaku na koji ste implementirate, **okruženje**, **Sastavljanje konfiguraciju**i **Konfiguracija servisa** provjerite jesu li sve ispravno.

1. Na kartici **Napredne postavke** računa za pohranu i oznaku implementaciju provjerite jesu li točno, je li poništen okvir **Brisanje implementacije pogreške** i je li potvrđen okvir **implementaciju** ažuriranja. Tako da potvrdite okvir Ažuriraj **implementaciju** provjerite implementaciju sustava neće se izbrisati i vaše VIP neće biti izgubljene kad ponovno objavite aplikaciju. Poništavanjem **Brisanje implementacije na pogreške potvrdni okvir**osigurati da vaše VIP ne biste izgubili ako dođe do pogreške tijekom implementacije.

1. Da biste dodatno odredili kako želite da se uloge ažurirati, odaberite vezu **Postavke** pokraj okvira za **implementaciju ažurirati** , a zatim rastuće ažuriranje ili mogućnost istodobnog ažuriranja u dijaloškom okviru **Ažuriranje implementacije** postavke. Ako odaberete rastuće ažuriranja, svaku instancu ažurirati nakon međusobno, tako da se aplikacija biti uvijek dostupna. Ako odaberete istodobno ažuriranja, sve instance se ažuriraju u isto vrijeme. Istodobno ažuriranje je brže, ali vaš servis možda neće biti dostupne tijekom postupka ažuriranja.

1. Kada budete zadovoljni s vašim postavkama, odaberite gumb **Dalje** .

1. Na stranici **Sažetak** čarobnjaka provjerite postavke, a zatim odaberite gumb **Objavi** .

  >[AZURE.WARNING] Uvođenje ne uspije, trebali biste adresa zašto je nije uspio i ponovno implementirate nas, ako ne želite bilježiti servis u oblaku oštećene.

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali o objavljivanju Azure s Visual Studio, potražite u članku [Čarobnjak aplikacije za objavljivanje Azure](vs-azure-tools-publish-azure-application-wizard.md).
