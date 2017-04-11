<properties
    pageTitle="Korištenje programa Outlook u Azure RemoteApp | Microsoft Azure" 
    description="Upute za konfiguriranje i korištenje programa Outlook u Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Pomoću programa Microsoft Outlook Azure RemoteApp

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

Azure RemoteApp podržava Microsoft Outlook O365. Saznajte više o korištenju [sustava Office u aplikaciji Azure RemoteApp](remoteapp-officesubscription.md). Postoji nekoliko preporučenih postavki za Outlook kad se koristi u Azure RemoteApp.

## <a name="cached-mode"></a>Predmemorirani način
Predmemorirani način je preporučena konfiguracija prilikom korištenja programa Outlook u Azure RemoteApp. Kada konfigurirate račun programa Outlook 2013 da biste koristili predmemorirani Exchangeov način, Outlook 2013 radi iz lokalne kopije poštanskog sandučića Microsoft Exchange korisnika koji je spremljen u izvanmrežne podatkovne datoteke programa (OST datoteke) na računalu korisnika, zajedno s u izvanmrežni adresu adresar (izvanmrežni Adresar). Predmemorirani poštanskog sandučića i izvanmrežni Adresar ažuriraju povremeno iz servisa za O365. Saznajte više o [razlikama između predmemorirani i mrežni način rada](https://technet.microsoft.com/library/jj683103.aspx).

Korisnik može odabrati **Predmemoriranog načina sustava Exchange** ili **Mrežni način rada** tijekom postavljanja računa ili tako da promijenite postavke računa. Pomoću alata za prilagodbu sustava Office (OCT) ili pravilnik grupe možete implementirati i jednom načinu ili na drugi.  

[Korak po korak upute za omogućivanje predmemorirani način](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc)za čitanje.

## <a name="search"></a>Pretraživanje
RemoteApp Azure pomoću pretraživanja unutar programa Outlook ima ograničenja. Azure RemoteApp koristi zajedničke VMs kako bi odgovarao korisničke sesije. Indeksiranje pretraživanja ovisi o računalu ID koji se razlikuje za različite VMs. Moguće je da svaki put kada se korisnik prijavljuje u Azure RemoteApp, oni su usmjereni na novu VM. Koja bi srednja vrijednost, ako ne možemo omogućiti lokalno pretraživanje, indeksiranje pokrenut će se prilikom svake promjene ID računala (kada je korisnik na različitim VM). Ovisno o veličini u. OST datoteku indeksiranje može potrajati da biste dovršili i korištenje resursima koji su potrebni za druge aplikacije. Pretraživanje ne samo bi sporo, ali može dati rezultate. Korištenje profila za račun programa mrežni način rada činite to može zaobići, ali performanse cjelokupnog će se zbog nedovoljne lokalne predmemorije (vidi iznad vezu za dodatne informacije o razlikama između predmemorirani i mrežni način rada). Nažalost, ne može se onemogućiti indeksirana/lokalno pretraživanje i internetsko pretraživanje ne može se omogućiti po zadanom u programu Outlook 2013.
