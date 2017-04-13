<properties
   pageTitle="Pristup Azure virtualnim strojevima s poslužitelja Explorer | Microsoft Azure"
   description="Dohvati pregled uputa za prikaz stvaranje i upravljanje Azure virtualnim strojevima (VMs) u programu Explorer poslužitelja u Visual Studio."
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

# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Pristup Azure virtualnim strojevima iz programa Explorer poslužitelja

Pomoću programa Explorer poslužitelja u Visual Studio može prikazati informacije o virtualnim računalima sustava hostira tvrtka Azure.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Pristup virtualnim strojevima u programu Explorer poslužitelja

Ako imate virtualnih računala koje hostira tvrtka Azure, možete im pristupati u programu Explorer poslužitelja. Morate najprije prijave u pretplatu za Azure da biste pogledali mobilnih usluga. Da biste se prijavili, otvorite izbornički prečac za Azure čvor u programu Explorer poslužitelja pa odaberite **Poveži se s Microsoft Azure**.

### <a name="to-get-information-about-your-virtual-machines"></a>Da biste saznali o virtualnim strojevima

1. U programu Explorer poslužitelj, odaberite virtualnog računala, a zatim tipku F4 da biste prikazali njegov prozor svojstva.

    U sljedećoj tablici prikazane su koje svojstva dostupna, ali su sve samo za čitanje. Da biste ih promijenili, koristite [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885).

  	|Svojstvo|Opis|
  	|---|---|
  	|Naziv DNS-a|URL adresa s internetsku adresu virtualnog računala.|
  	|Okruženje|Za virtualnog računala vrijednost ovo svojstvo uvijek je proizvodnje.|
  	|Ime|Naziv virtualnog računala.|
  	|Veličina|Veličina virtualnog računala koja odražava količinu memorije i prostora na disku koja je dostupna. Dodatne informacije potražite u članku kako s: Konfiguriranje veličine virtualnog računala.|
  	|Status|Vrijednosti uključivati početni, rada, zaustavljanje, zaustavljanja i dohvaćanje Status. Ako se pojavi dohvaćanje Status, trenutni status nije poznat. Vrijednosti za to svojstvo se razlikovati od vrijednosti koje se koriste za [portal za Azure klasični](http://go.microsoft.com/fwlink/?LinkID=213885).|
  	|SubscriptionID|ID pretplate za vaš račun za Azure. Prikaži ove podatke za [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885) prikazivanja svojstava za pretplatu.|

1. Odaberite čvor na krajnjoj točki, a zatim pogledajte prozor **Svojstva** .

1. U sljedećoj tablici opisane dostupnih svojstava krajnje točke, ali su samo za čitanje. Dodavanje ili uređivanje krajnje točke za virtualnog računala, pomoću [Azure klasični portal](http://go.microsoft.com/fwlink/?LinkID=213885). 

  	|Svojstvo|Opis|
  	|---|---|
  	|Ime|Identifikator za krajnju točku.|
  	|Privatna priključak|Priključak za pristup mreži interne u aplikaciji.|
  	|Protokol|Protokol koji koristi sloj prijenosa za ovu krajnju točku, TCP i UDP.|
  	|Javno priključak|Priključak koji se koristi za pristup javnim aplikacije.|

## <a name="next-steps"></a>Daljnji koraci

Da biste saznali više o korištenju Azure uloge u Visual Studio, potražite u članku [Korištenje udaljenu radnu površinu s Azure uloge](vs-azure-tools-remote-desktop-roles.md).
