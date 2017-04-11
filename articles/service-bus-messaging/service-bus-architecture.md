<properties 
    pageTitle="Servis Bus arhitektura | Microsoft Azure"
    description="U članku se opisuje poruka i prijenos obrade arhitektura Bus servisa Azure."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="07/11/2016"
    ms.author="sethm" />

# <a name="service-bus-architecture"></a>Servis Bus arhitekture

U ovom se članku opisuju poruke i prijenos obrade arhitektura Bus servisa Azure.

## <a name="service-bus-scale-units"></a>Servis Bus skaliranje jedinice

Servis Bus je organiziran prema *Skaliranje jedinice*. Skaliranje jedinica je jedinica implementacije i sadrži sve potrebne se komponente pokrenuti servis. Svako područje uvodi jednu ili više jedinica skaliranje Bus servisa.

Prostor naziva servisa Bus mapirani skaliranje jedinicu. Jedinica skaliranje rukuje sve vrste servisa Bus entiteti: preusmjeravanje i brokered razmjenu entiteti (redovi, teme, pretplate). Skaliranje jedinica servisa Bus sastoji se od sljedeće komponente:

- **Skup čvorove pristupnika.** Čvorovi pristupnika autentičnost dolaznih zahtjeva i obradu zahtjeva za prijenos. Svaki čvor pristupnik je javna IP adresa.

- **Skup poruka broker čvorove.** Razmjenu broker čvorove obrade zahtjeva za razmjenu entiteti koji se odnose.

- **Spremište jedan pristupnik.** Spremište pristupnika sadrži podatke za svaki entitet koji je definiran u toj jedinici mjerilo. Spremište pristupnik je implementirana pri vrhu bazom podataka SQL Azure.

- **Više poruka trgovine.** Razmjenu trgovine držite poruke svih redova, teme i pretplate koji su definirani u toj jedinici mjerilo. Sadrži sve podatke o pretplati. Osim ako je omogućeno u [particioniranom poruka entiteti](service-bus-partitioning.md) , reda čekanja ili temu mapirani jedan razmjenu Store. Pretplate se pohranjuju u isto razmjenu spremište kao njihove nadređenog temu. Osim Bus servisa [Premium poruka](service-bus-premium-messaging.md), razmjenu trgovine primjenjuju pri vrhu baze podataka SQL Azure.

## <a name="containers"></a>Spremnika

Svaki razmjenu entitet se dodjeljuje određene kontejner. Spremnik je konstrukta logičke koji koristi točno jedan poruka iz trgovine za spremište sve relevantne podatke za ovaj kontejner. Razmjenu broker čvor je dodijeljen svakog spremnika. Obično nema više spremnika od poruka broker čvorove. Zbog toga svaki razmjenu čvor broker učita više spremnika. Distribucija spremnika za razmjenu čvor broker organizirani tako da sve razmjenu broker čvorove ravnomjerno učitavaju. Ako mogućnost Učitaj uzorak promjene (na primjer, jedan od vrlo zauzet dobije spremnika) ili ako razmjenu čvor broker postati privremeno nedostupan, spremnike su daljnja distribucija među broker čvorove razmjene poruka.

## <a name="processing-of-incoming-messaging-requests"></a>Obrada zahtjevi za razmjenu poruka

Kada klijent šalje zahtjev Bus servisa, Azure opterećenja je preusmjerava na bilo koju od čvorove pristupnika. Čvor pristupnika neadministratorskog zahtjev. Ako se zahtjev odnosi razmjenu entitet (red, teme, pretplatu), čvor pristupnika traži entitet u trgovini pristupnika i određuje koje razmjenu pohrana je entitet. Zatim traži koje razmjenu broker čvor je trenutno održavanje ovaj spremnik i šalje zahtjev te razmjenu broker čvor. Razmjenu čvor broker obrađuje zahtjev i ažurira stanje entitet u spremištu kontejner. Razmjenu čvor broker šalje natrag čvor pristupnika koje šalje odgovarajuće odgovor natrag na klijentu koja je izdala izvorni zahtjev za odgovor.

![Obrada zahtjevi za razmjenu poruka](./media/service-bus-architecture/IC690644.png)

## <a name="processing-of-incoming-relay-requests"></a>Obrada zahtjevi za prijenos

Kada klijent šalje zahtjev Bus servisa, Azure opterećenja je preusmjerava na bilo koju od čvorove pristupnika. Ako je zahtjev za zahtjev za slušanje, čvor pristupnika stvara novi prijenos. Ako je zahtjev za vezu zahtjev za određene preusmjeravanja, vezu zahtjev čvor pristupnika vlasništvu prijenos prosljeđuje čvor pristupnika. Čvor pristupnika vlasništvu prijenos šalje zahtjev za rendezvous listening klijenta s pitanjem ga slušatelj stvaranje privremene kanala čvor pristupnika koje primiti zahtjev za povezivanje.

Nakon prijenosa veze, klijente možete razmjenjivati poruke putem čvor pristupnika koji se koristi za na rendezvous.

![Obrada zahtjevi za prijenos](./media/service-bus-architecture/IC690645.png)

## <a name="next-steps"></a>Daljnji koraci

Sad kad ste pročitali pregled usluge Bus arhitektura, za početak potražite na sljedećim vezama:

- [Bus usluge SMS pregled](service-bus-messaging-overview.md)
- [Osnove Bus servisa](service-bus-fundamentals-hybrid-solutions.md)
- [U redu čekanja razmjenu rješenje pomoću servisa Bus redovi](service-bus-dotnet-multi-tier-app-using-service-bus-queues.md)
