<properties
   pageTitle="Tehnički stara requisites za stvaranje podatkovnog servisa za Marketplace | Microsoft Azure"
   description="Razumijevanje preduvjeti za stvaranje podatkovnog servisa i Prodaja na Azure Marketplace"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Tehnički stara requisites za stvaranje podatkovnog servisa ponuda za Azure Marketplace

>[AZURE.IMPORTANT] **Trenutno ne možemo više nisu za uhodavanje sve nove izdavači podatkovnog servisa. Novi dataservices će dobiti odobrene za unos.** Ako imate aplikaciju SaaS tvrtke koje želite objaviti na AppSource možete pronaći dodatne informacije [u nastavku](https://appsource.microsoft.com/partners). Ako imate programa IaaS aplikacije ili servisa za razvojne inženjere koje želite objaviti na Azure Marketplace možete pronaći dodatne informacije [u nastavku](https://azure.microsoft.com/marketplace/programs/certified/).

Čitanje postupka temeljito prije početka i razumijevanje gdje i zašto se izvode svakog koraka. Najveću moguću, koje treba Priprema podataka o tvrtki i ostalih podataka, preuzmite potrebne alate i/ili stvorite tehničke komponente prije početka postupak stvaranja ponudu.

Trebali biste dobiti jeste li spremni prije početka postupka sljedećih stavki:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Donošenje odluka na koju tehnologiju će se koristiti za objavljivanje tu ponudu podatkovnog servisa

U programu Publisher možete odabrati više tehnologije pri objavljivanju podatkovnog servisa u Azure Marketplace. Glavni tehnologije podržani opisan u nastavku. Regardless koju tehnologiju koristi se za objavljivanje podatkovnog servisa, krajnji korisnik troši podataka putem na **OData sažetka sadržaja** koji prikazuje servisa Azure Marketplace. Potpuna informacije o servisu OData možete pronaći na [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure baze podataka

Imate dataset je spremna u SQL Azure je odgovornosti programa Publisher. Morate se pretplatiti na Azure, dodijelite odgovarajuću veličinu baze podataka i prenijeti podatke u baze podataka SQL Azure. Publisher je zadužen da se svojem podaci uvijek ažurni. Dodatne informacije o pretplate na usluge Azure možete pronaći na [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


Prilikom pomicanja podatke u SQL Azure, trgovine Windows Azure, možete izložiti tablice i prikaze. U programu Publisher možete odrediti koji su tablice i prikaze i stupaca su na raspolaganju krajnji korisnik. Daljnje davatelj sadržaja možete odrediti stupce koji se mogu mu tako da krajnji korisnik, a koje korisnik su samo u opseg. Tako ćete dobiti visoku razinu fleksibilnost koji treba izložiti podataka u bazi podataka. Stupce koje možete mu moraju biti sigurnosno po jednu ili više indeksi baze podataka.

## <a name="rest-based-web-service"></a>Web-servisa REST temelji

Podržava protokol: **HTTPS samo**

Postojeće usluge OSTALE temelji mogu otkriti putem servisa Azure Marketplace. Jer skup podataka uvijek izložen krajnjeg korisnika kao OData sažetak sadržaja, potrebama servisa Azure Marketplace da biste mogli mapiranje usluge OData sustava temelji servisa. Da biste učinili da OSTALE temelji krajnje točke potrebne da bi se prikazale sve parametre kao parametar HTTP.

Opseg mora biti u obliku koji se mogu mapirati u je ATOM odgovor. Dakle odgovor na services mora biti u obliku XML i samo mogu sadržavati jedan ponavljajući element koji sadrži vrijednosti tereta (kao što je skup zapisa). Servisa Azure Marketplace mapirati čvor ponavljajuće čvor unos u ATOM i tereta vrijednosti u svojstvo čvorove unutar čvor unosa.

Informacija o provjeri autentičnosti (primjerice API ključ, provjere autentičnosti tokena, itd.) mora biti navedeno kao parametar HTTP ili u zaglavlju HTTP (par vrijednost ključa) – Osnovna provjera autentičnosti podržano je i. Valjani ključ mora se navesti i sve zahtjeve za putem trgovine Windows Azure vrše do te tipke. Korisnik nadzor i naplata događa sloju trgovine Windows Azure.

Pogreške koje je servis nije potrebno je mapirati u HTTP kodovima stanja. U slučaju da servis vraća XML koja sadrži pogrešku te namjeravate mapirati servisa Azure Marketplace da biste HTTP kodovima stanja.

## <a name="soap-based-web-services"></a>SOAP temelji web-servisi

Protokol: **HTTPS samo**

Preduvjeti su jednaki onima OSTALE temelji odjeljak servisa. Jedina razlika je da parametara također se može pružati tijela XML koje se objavljuju u programu Publisher servisa s svaki zahtjev putem servisa Azure Marketplace. To znači da HTTP parametara korisnik unese na sučeljem su se prevesti u XML elemenata XML dokumenta koje se objavljuju sa zahtjevom za davatelja sadržaja web-servisa.

## <a name="odata-based-web-services"></a>OData temelji web-servisi

Protokol: **HTTPS samo**

Podatke možete izložiti kao servis OData Azure Marketplace. Sustav će proslijediti usluge putem i zamjenjuje korijenu servis korijen servisa Azure Marketplace – da biste bili sigurni da Svi naknadni pozivi proći kroz trgovine Windows Azure.

Usluge OData samo ne morate ići bazi podataka u pozadinski. OData podržava sve vrste prostor za pohranu ili poslovne logike na pogon servis.


## <a name="next-steps"></a>Daljnji koraci
Sad kad pregledava prije requisites i izvršiti potrebne zadatke, možete premjestiti naprijed stvaranje ponude za podatkovnog servisa kao detaljne [Podatke servisa vodič za objavljivanje](marketplace-publishing-data-service-creation.md).

Ili, ako želite da biste pregledali cjelokupan postupak i odgovarajući članaka za svaku objavljivanje faze, posjetite članak [Početak rada: kako objaviti ponude Azure Marketplace](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
