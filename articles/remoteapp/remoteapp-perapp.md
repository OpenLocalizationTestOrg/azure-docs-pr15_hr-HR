<properties
   pageTitle="Objavljivanje aplikacija za pojedinačne korisnike u zbirci Azure RemoteApp (pretpregled) | Microsoft Azure"
   description="Saznajte kako možete objaviti aplikacije za pojedinačne korisnike, umjesto ovisno o grupama u Azure RemoteApp."
   services="remoteapp-preview"
   documentationCenter=""
   authors="piotrci"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="piotrci"/>

# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Objavljivanje aplikacija za pojedinačne korisnike u zbirci Azure RemoteApp (pretpregled)

> [AZURE.IMPORTANT]
> Ukinute Azure RemoteApp je u tijeku. Čitanje [Objava](https://go.microsoft.com/fwlink/?linkid=821148) detalje.

U ovom se članku objašnjava kako objaviti pojedinačnim korisnicima u zbirci Azure RemoteApp. To su nove funkcije RemoteApp Azure trenutno pretpregledu"Privatno" i dostupne samo da biste odabrali Prijevremeni adopters za procjenu svrhe.

Koji su izvorno omogućeni Azure RemoteApp samo jedan od načina "Objavljivanje" aplikacija: administrator želite objaviti aplikacije iz slike i će biti vidljiv svim korisnicima u zbirci.

Uobičajeni scenarij tako da sadrže mnoge aplikacije u jednu sliku, a zatim implementacija jednu zbirku da bi se smanjiti troškove upravljanja. Oftentimes sve aplikacije relevantne za sve korisnike – administratori radije da biste objavili aplikacije pojedinačnim korisnicima tako da ne vide nepotrebne aplikacije u svoje aplikacije sažetka sadržaja.

To sada je moguće RemoteApp Azure – trenutno kao značajka ograničeni pretpregled. Ovdje je kratak sažetak nove funkcije:

1. Zbirka moguće je postaviti u jedan od dva načina:
 
  - izvorni "način zbirke", gdje mogu vidjeti svi korisnici u zbirci sve objavljene aplikacije. To je zadani način.
  - Novi "aplikacije način rada", gdje korisnici vidjeti samo aplikacije koje su izričito dodijeljene

2. U trenutku način aplikacije mogu se omogućiti pomoću cmdleta ljuske RemoteApp PowerShell Azure.

  - Kada je postavljeno na aplikacijskom načinu rada, Dodjela korisnika u zbirci nije moguće upravljati putem portala za Azure. Dodjela korisnik mora se upravlja pomoću cmdleta ljuske PowerShell.

3. Korisnici će vidjeti samo aplikacije objavili izravno na njih. Međutim, i dalje može za korisnika da biste pokrenuli u druge aplikacije koje su dostupne na slici tako da ih pristupa izravno u operacijskom sustavu.
  - Ta značajka ne nudi na sigurne zaključati da biste aplikacija; samo se ograničava vidljivost u aplikaciji sažetka sadržaja.
  - Ako vam je potrebna izdvajanja korisnika iz aplikacije, morat ćete koristiti odvojene zbirke za to.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Kako nabaviti cmdleta ljuske RemoteApp PowerShell Azure

Da biste isprobali nove funkcije pretpregled, morat ćete pomoću cmdleta ljuske PowerShell Azure. Trenutno nije moguće koristiti portal za upravljanje Azure da biste omogućili objavljivanje način nove aplikacije.

Najprije provjerite imate [Modul Azure PowerShell](../powershell-install-configure.md) instaliran.

Zatim pokrenite konzolu PowerShell u načinu rada za administratora i pokrenite sljedeći cmdlet:

        Add-AzureAccount

Ponudit će vam Azure korisničko ime i lozinku. Kada se prijavite, će moći pokrenuti Azure RemoteApp cmdleta pretplate Azure.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Kako provjeriti koji način zbirka je u

Pokrenite sljedeći cmdlet:

        Get-AzureRemoteAppCollection <collectionName>

![Provjera način zbirke](./media/remoteapp-perapp/araacllelvel.png)

Svojstvo AclLevel može imati sljedeće vrijednosti:

- Zbirka: Izvorna objavljivanje načina rada. Svi korisnici vidjeti sve objavljene aplikacije.
- Aplikacija: novi objavljivanje način rada. Korisnici vidjeti samo aplikacije koje su objavljene izravno na njih.

## <a name="how-to-switch-to-application-publishing-mode"></a>Kako se prebaciti na aplikacijskom objavljivanje načinu rada

Pokrenite sljedeći cmdlet:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Aplikacija objavljivanje stanje će se sačuvati: prethodno svi korisnici će vidjeti sve izvorne objavljene aplikacija.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Kako se popis korisnika koji mogu vidjeti određenu aplikaciju

Pokrenite sljedeći cmdlet:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Sadrži popis svih korisnika koji mogu vidjeti aplikacije.

Napomena: Aplikacija pseudonima (pod nazivom "pseudonim za aplikaciju" u sintaksi gore) možete vidjeti tako da pokrenete Get-AzureRemoteAppProgram - CollectionName <collectionName>.

## <a name="how-to-assign-an-application-to-a-user"></a>Kako dodijeliti aplikaciju za korisnika

Pokrenite sljedeći cmdlet:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Korisnik će se pojaviti aplikacije u klijent za Azure RemoteApp i moći ćete povezati s njim.

## <a name="how-to-remove-an-application-from-a-user"></a>Kako ukloniti aplikaciju za korisnika

Pokrenite sljedeći cmdlet:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Pružanje povratnih informacija
Cijenimo vaše povratne informacije i prijedlozi za značajka pretpregleda. Popunite [upitnika](http://www.instant.ly/s/FDdrb) da biste nam što mislite.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Još niste imali mogućnost da biste isprobali značajke pretpregleda?
Ako ste su ne sudjelovali u pretpregledu još, koristite ovaj [upitnik](http://www.instant.ly/s/AY83p) da biste zatražili pristup.
