<properties
    pageTitle="Oblak Services najčešća pitanja vezana uz | Microsoft Azure"
    description="Najčešća pitanja o servise u Oblaku."
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="adegeo"/>

# <a name="cloud-services-faq"></a>Najčešća pitanja vezana uz servise u oblaku
U ovom se članku navedeni odgovori na najčešća pitanja o Microsoftovim servisima u Oblaku Azure. Možete posjetiti i [Azure podržava najčešća pitanja vezana uz](http://go.microsoft.com/fwlink/?LinkID=185083) općenite Azure informacije cijene i podrška. [Oblak Services VM veličina stranice](cloud-services-sizes-specs.md) možete pogledajte i za podatke o veličini.

## <a name="certificates"></a>Certifikati

### <a name="where-should-i-install-my-certificate"></a>Gdje instalirati Moje certifikat?

- **Moj**  
Aplikacija certifikat i privatni ključ (\*.pfx, \*.p12).

- **CA**  
U ovom spremištu (pravila i Sub CA) otvorite vaše Srednja potvrde.

- **KORIJENSKA**  
Korijenska CA pohraniti, tako da na glavnom korijenski CA certifikata treba izvor informacija.

### <a name="i-cant-remove-expired-certificate"></a>Nije moguće ukloniti istekli certifikat

Azure onemogućuje uklanjanje certifikat dok se koristi. Trebate izbrisati implementacije koja koristi certifikat ili ažurirati implementacijskih certifikatom za različite ili obnovljeni.

### <a name="delete-an-expired-certificate"></a>Brisanje istekli certifikat

Sve dok se ne potvrdi koristi, koristite cmdlet ljuske PowerShell [Ukloni AzureCertificate](https://msdn.microsoft.com/library/azure/mt589145.aspx) da biste uklonili certifikat.

### <a name="i-have-expired-certificates-named-windows-azure-service-management-for-extensions"></a>Li istekla certifikati pod nazivom Upravljanje servisom za Windows Azure za proširenja

Ove potvrde stvaraju se svaki put kada datotečni nastavak dodaje se u oblaku kao što su proširenje udaljene radne površine. Ove potvrde samo se koriste za šifriranje i dešifriranje privatne konfiguraciju datotečni nastavak. Nije važno ako istječe ove potvrde. Datum isteka nije prijavljen.

### <a name="certificates-i-have-deleted-keep-reappearing"></a>Certifikati su izbrisane stalno ponovo pojavljuju

Oni stalno ponovo pojavljuju najvjerojatnije zbog alat koji koristite, kao što su Visual Studio. Kada se ponovno povežete s alat koji koristi potvrdu, je moguće ponovno prenijeti na Azure.

### <a name="my-certificates-keep-disappearing"></a>Certifikate zadržati nestaje za vrijeme

Kada recycles instancu virtualnog računala, sve lokalne promjene se gube. Koristite [pokretanje zadatka](cloud-services-startup-tasks.md) da biste instalirali certifikati virtualnog računala svakom pokretanju ulogu.

### <a name="i-cannot-find-my-management-certificates-in-the-portal"></a>Ne možete pronaći certifikate upravljanja na portalu

[Upravljanje certifikati](..\azure-api-management-certs.md) su dostupne samo na portalu klasični Azure. Trenutni portal za Azure pomoću upravljanja certifikata. 

### <a name="how-can-i-disable-a-management-certificate"></a>Kako onemogućiti upravljanje certifikat?

[Upravljanje certifikata](..\azure-api-management-certs.md) ne može se onemogućiti. Možete ih izbrisati putem klasične portala za Azure kada ih se može koristiti više ne želite.

### <a name="how-do-i-create-an-ssl-certificate-for-a-specific-ip-address"></a>Kako stvoriti SSL certifikata za određene IP adrese?

Slijedite upute u [Stvaranje certifikata vodič](cloud-services-certs-create.md). Naziv DNS upotrijebite IP adresa.

## <a name="security"></a>Sigurnost

### <a name="disable-ssl-30"></a>Onemogućivanje SSL 3.0

Da biste onemogućili SSL 3.0 i korištenje TLS sigurnosti, stvorite zadatak pokretanja koji se na blogu: https://azure.microsoft.com/en-us/blog/how-to-disable-ssl-3-0-in-azure-websites-roles-and-virtual-machines/

## <a name="scale-a-cloud-service"></a>Promjena veličine servis u oblaku

### <a name="i-cannot-scale-beyond-x-instances"></a>Li ne možete mijenjati veličinu izvan X instanci

Vaša pretplata Azure ima ograničenje broja jezgri možete koristiti. Promjena veličine neće funkcionirati ako ste koristili sve jezgri koje su dostupne. Ako, na primjer, ako imate ograničenje od 100 jezgri, to znači da nije instaliran 100 instance A1 veličine virtualnog računala za servis u oblaku ili 50 A2 veličine instance virtualnog računala.

## <a name="troubleshooting"></a>Otklanjanje poteškoća

### <a name="i-cant-reserve-an-ip-in-a-multi-vip-cloud-service"></a>Se ne možete rezervirati IP u više VIP u oblaku

Prvo, provjerite je li uključena virtualnog računala instancu koju pokušavate rezervirati IP za. Drugo, provjerite koristite li Rezervirana IP-ovi za zamarati implementacijama pripremna i radnih. **Ne** promijenite postavke dok je implementaciju nadogradnje.

