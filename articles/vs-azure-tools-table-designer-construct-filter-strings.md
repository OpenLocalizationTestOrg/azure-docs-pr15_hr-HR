<properties
   pageTitle="Izgradnje nizovi filtar za dizajneru tablice | Microsoft Azure"
   description="Izgradnje nizove filtar za dizajnera tablice"
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="storage"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="constructing-filter-strings-for-the-table-designer"></a>Izgradnje nizovi filtar za dizajnera tablice

## <a name="overview"></a>Pregled

Da biste filtrirali podatke u tablici programa Azure koji se prikazuje u Visual Studio **Dizajnera tablice**, sastavljanje filtar niza i unos u polje filtra. Sintaksa niza filtar definira WCF Data Services i je slična je SQL uvjet WHERE, ali na servisu tablice putem HTTP zahtjev. **Dizajnera tablice** rukuje proper kodiranje umjesto vas, tako da biste filtrirali na željenu vrijednost, morate unijeti samo na naziv svojstva operator usporedbe, vrijednost kriterija te po želji Booleov operator u polje filtra. Ne morate uključiti mogućnost upita $filter kao i ako su izgradnje URL-a u tablici putem [Prostora za pohranu servisa REST API Reference](http://go.microsoft.com/fwlink/p/?LinkId=400447)za upite.

WCF Data Services temelje se na [Protokola Open Data](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Detalje o mogućnost upita za sustav filtra (**$filter**) potražite u članku [Specifikacija OData URI konvencije](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Operatori usporedbe

Za sve vrste svojstava podržani su sljedeći logički operatori:

|Logički operator|Opis|Primjer filtar niza|
|---|---|---|
|EQ|Jednako|Grad eq "Redmond"|
|gt|Veće od|Cijena gt 20|
|Spoji|Veće od ili jednako|Spoji cijena 10|
|lt|Manje od|Cijena lt 20|
|u|Manje je od ili jednako|Cijena u 100|
|No|Nije jednako|No Grad "London"|
|i|I|Cijena u 200 i cijena gt 3.5|
|ili|Ili|Cijena u 3.5 ili cijena gt 200|
|ne|Ne|ne isAvailable|

Tijekom izgradnje filtar niza sljedeća pravila su važne:

- Korištenje logičkih operatora za usporedbu svojstvo vrijednosti. Imajte na umu da nije moguće da biste usporedili svojstvo za dinamičku vrijednost; jednu stranu izraza mora biti konstantom.

- Svi dijelovi filtar niza su velika i mala slova.

- Vrijednost konstante moraju biti iste vrste podataka kao svojstva za filtar da biste se vratili valjane rezultate. Dodatne informacije o podržanim svojstvo vrste potražite u članku [Osnove podatkovnog modela servisa u tablici](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Filtriranje na svojstva niza

Kad filtrirate niz svojstava, stavite konstanta niza u jednostruke navodnike.

Sljedeći primjer filtre na svojstva **PartitionKey** i **RowKey** ; dodatna svojstva koje nisu ključ nije moguće dodati i niz filtra:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Iako nije obavezno, smjestite svaki izraz filtra u zagrade:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Imajte na umu da tablice usluga ne podržava zamjenskih upite i nisu podržane u dizajneru tablice ili. No možete izvršiti prefiks koji se podudaraju pomoću operatori usporedbe na željeni prefiks. Sljedeći primjer vraća entiteti s tim Prezime svojstvo započinje slovom "A":

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>Filtriranje na brojčana svojstva

Da biste filtrirali na cijeli broj ili broj s pomičnom točkom, navedite broj bez navodnika.

U ovom se primjeru vraća svi entiteti s svojstvo programa dob čija je vrijednost veća od 30:

    Age gt 30

U ovom se primjeru vraća svi entiteti s svojstvo programa AmountDue čija je vrijednost manja od ili jednaka 100.25:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>Filtriranje na Booleove svojstva

Da biste filtrirali na Booleove vrijednosti, navedite **true** ili **false** bez navodnika.

Sljedeći primjer vraća svi entiteti ondje gdje je svojstvo IsActive postavljeno na **true**:

    IsActive eq true

Možete upisati i ovaj izraz filtra bez logičkih operatora. U sljedećem primjeru servis tablice također vratit će svi entiteti koje je **istinita**IsActive:

    IsActive

Da biste vratili svi entiteti gdje je false IsActive, možete koristiti nije operatora:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Filtriranje na svojstva datuma i vremena

Da biste filtrirali vrijednost datuma i vremena, navedite ključnih riječi za **datetime** , nakon čega slijedi konstanta datuma/vremena u jednostruke navodnike. Konstanta datuma/vremena mora biti u obliku kombinirane UTC-a, kao što je opisano u [Oblikovanje vrijednosti nekretnina datuma i vremena](http://go.microsoft.com/fwlink/p/?LinkId=400449).

Sljedeći primjer vraća entiteti gdje je svojstvo CustomerSince jednak je srpanj 10, 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
