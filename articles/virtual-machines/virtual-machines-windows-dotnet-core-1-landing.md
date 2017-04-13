<properties
   pageTitle="Azure virtualnog računala DotNet Core vodiča 1 | Microsoft Azure"
   description="Praktični vodič DotNet Core Azure virtualnog računala"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="automating-application-deployments-to-azure-virtual-machines"></a>Automatizacija implementacijama aplikacije virtualnim računalima sustava za Azure

Ovaj niz od četiri dijela detalje o implementaciji i konfiguraciji Azure resursa i aplikacije koje koriste Azure resursa Upravljanje predlošcima. U ovoj seriji je implementiran oglednog predloška i pregledavaju uvođenje predloška. Cilj ovog niza je educirajte na odnos između Azure resurse i pružanja ruke sučelju uvođenje potpuno integrirano Voditelj resursa Azure predložaka. Ovaj dokument pretpostavlja osnovni razinu znanja s Azure Voditelj resursa, prije pokretanja ovog praktičnog vodiča Upoznajte se s osnovni koncepti Azure Voditelj resursa.

## <a name="music-store-application"></a>Aplikacije za pohranu glazba

Primjer koristi u ovoj seriji je u .net Core aplikacije simulaciju glazba spremište košarice sučelje. Ove aplikacije koje se može uvesti Linux ili Windows virtualne sustav, ogledni implementacijama stvorene za oba. Aplikacija obuhvaća web-aplikacije i SQL baze podataka. Prije nego što čitanje članaka u ovoj seriji implementacija aplikacije pomoću implementacije gumb koji se nalazi na ovoj stranici. Kada potpuno implementiran, aplikacije / Azure arhitektura izgleda kao na sljedećem su dijagramu. 

Voditelj resursa spremište glazbu predložak Ovdje možete pronaći, [Glazba iz trgovine Windows predloška](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

![Aplikaciju glazbu pohrane](./media/virtual-machines-windows-dotnet-core/music-store.png)

Svaki od tih komponenti, uključujući Pridruži predložak JSON se pregledavaju u sljedećim člancima četiri.

- [**Arhitektura aplikacije**](./virtual-machines-windows-dotnet-core-2-architecture.md) – aplikacije komponente kao što je web-mjesta i bazama podataka moraju se nalaziti na resurse za Azure računala kao što su virtualnih računala i baze podataka Azure SQL. Ovaj dokument vodi kroz mapiranje računalnim treba li vam, Azure resurse i implementacija tih resursa pomoću predloška Azure Voditelj resursa. 

- [**Access i sigurnost**](./virtual-machines-windows-dotnet-core-3-access-security.md) – kada nalaze aplikacije u Azure, je potrebno uzeti u obzir načinu pristupa aplikacije i funkcioniranja različitih aplikacija komponente access međusobno povezani. Ovaj dokument detalje o pružanje i zaštita pristup internetu u aplikaciju i pristup između aplikacija komponente.

- [**Dostupnost i skaliranje**](./virtual-machines-windows-dotnet-core-4-availability-scale.md) – dostupnosti i skaliranje odnosi mogućnost aplikacije da biste bili pokretanje tijekom nedostupnost infrastrukture i mogućnost skaliranja računalnim resursi koji zadovoljavaju zahtjev za aplikaciju. U ovom komponente potrebne za implementaciju rasporediti opterećenje detalja o dokumentu i vrlo dostupnih aplikacija.

- [**Implementacija aplikacije**](./virtual-machines-windows-dotnet-core-5-app-deployment.md) - prilikom implementacije programa virtualnim računalima sustava na Azure metodu binarne datoteke iz aplikacije koji su instalirani na virtualnog računala moraju smatrati. Ovaj dokument detalje o Automatizacija pomoću Azure virtualnog računala prilagođene skripte proširenja instalaciju aplikacije.

Cilj prilikom razvoja Voditelj resursa Azure predložaka je da biste automatizirali implementacije infrastruktura za Azure i instalacije i konfiguracije sve programe koji se nalazi na ovom Azure infrastrukture. Rad pomoću ovih članaka nudi primjera tog problema.

## <a name="deploy-the-music-store-application"></a>Implementacija aplikacije pohrane glazbu

Aplikacije za glazbu pohrane može uvesti pomoću ovaj gumb.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-windows%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Voditelj resursa Azure predložak zahtijeva sljedeće vrijednosti parametara.

|Naziv parametra |Opis   |
|---|---|
|ADMINUSERNAME   | Administrator korisničkog imena koje se koristi u virtualnog računala i baze podataka SQL Azure.  |
|ADMINPASSWORD | Lozinka koja se koristi na Azure virtualnog računala i baze podataka SQL.  |
|NUMBEROFINSTANCES | Broj virtualnim strojevima će biti stvoren. Svaki od tih virtualnim strojevima hostira glazbu spremište web-aplikaciju, a sve promet je rasporediti preko njih opterećenje. |
|PUBLICIPADDRESSDNSNAME | Globalno jedinstveni naziv za DNS pridružene javnu IP adresa. |

Kada se dovrši uvođenje predloška, idite na javnu IP adresu pomoću bilo kojeg preglednika internet. .Net prikazat će se glazba Core web-mjesto.

## <a name="next-steps"></a>Daljnji koraci

<hr>

[Korak 1 - arhitektura aplikacije s predlošcima Azure Voditelj resursa](./virtual-machines-windows-dotnet-core-2-architecture.md)

[Korak 2 - pristupa i sigurnosti u predlošcima Azure Voditelj resursa](./virtual-machines-windows-dotnet-core-3-access-security.md)

[Korak 3 – dostupnosti i skaliranje u predlošcima Azure Voditelj resursa](./virtual-machines-windows-dotnet-core-4-availability-scale.md)

[Korak 4 - implementaciju aplikacije s predlošcima Azure Voditelj resursa](./virtual-machines-windows-dotnet-core-5-app-deployment.md)


